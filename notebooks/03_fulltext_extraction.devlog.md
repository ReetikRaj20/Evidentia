# Devlog — `03_fulltext_extraction.ipynb`

Changes to this notebook and what I learned from them.
Times are local (Europe/Berlin). Newest entries at the top.
Project-wide log: [../DEVLOG.md](../DEVLOG.md)

---

**Purpose:** Go beyond titles and abstracts: find which search results have downloadable full text, download one paper's GROBID XML, and parse it into sections and paragraphs so the full text can later be searched for evidence.

**Status:** Search, availability check, download and XML parsing work (`lxml` installed). Body extracted as sections → paragraphs → sentences. The XML has no real abstract (needs the OpenAlex fallback).

**Next**
1. Add an abstract fallback: use the OpenAlex abstract when the XML abstract is missing or too short.
2. Clean the few mis-labelled section headings (equation fragment, list item).
3. Add `data/` to `.gitignore` before the next commit (the downloaded paper is in `data/fulltext/` and must not go to GitHub).
4. Reuse the word-overlap method from notebook 01 on the full-text paragraphs and compare with the abstract-only result.

## 2026-09-27

### 19:18 — New extraction: body → sections → paragraphs → sentences

**Problem:** the end of my extracted body text wasn't the Conclusion but a side note and figure captions, so it looked as if part of the paper was missing.

**Why:** GROBID puts the **figures** (`<figure>` with their captions) and side notes at the end of `<body>`, after the Conclusion. `find_all("div")` / `find_all("s")` search *every* level, so they also picked up the text inside those figures.

**Fix:** take only the `<div>`s **directly** inside `<body>`:
```python
body = soup.find("body")
for div in body.find_all("div", recursive=False):   # sections only, figures skipped
    ...
```
Each section is stored as a heading plus a list of paragraphs, and each paragraph as a list of sentences (GROBID's `<s>` tags):
```
sections[i]["heading"]           → "VI. RESULTS"
sections[i]["paragraphs"][j]     → paragraph j (list of sentences)
sections[i]["paragraphs"][j][k]  → sentence k
```
**Result:** the body is complete. All sections I–VIII are there, and the last section is **VIII. CONCLUSION**. This structure lets Evidentia later point to evidence exactly: section, paragraph, sentence.

**Minor parsing issues still visible** (in headings only, not missing text): an equation fragment (`sa (t)).`) and a list item (`6) Presentation mode:`) were labelled as section headings.

### 19:11 — Issue: the GROBID XML has no real abstract

Read the saved file `data/fulltext/W2408166852.grobid.xml` directly.

- `<abstract>` contains only *"for improved EEG-based auditory attention detection in a cocktail party scenario"*, the second half of the **title**, not the abstract.
- The real abstract is **not anywhere in the file**.
- Likely cause: the source PDF had a repository cover page (the keywords include *"Wouter Biesmans (2016)"* and *"Neuro-steered auditory prostheses"*), which confused GROBID's layout detection.

**What this means for Evidentia:** GROBID XML is an automatic conversion from PDF, so the abstract can be missing or wrong. Use the **OpenAlex abstract** (`abstract_inverted_index`) when the XML abstract is missing or suspiciously short (e.g. fewer than ~50 words).

### 18:42 — Error: `FeatureNotFound: Couldn't find a tree builder with the features you requested: xml`

**What happened:** Parsing the XML with `BeautifulSoup(f, "xml")` failed.

**Why:** BeautifulSoup can't read XML by itself; for the `"xml"` parser it needs the `lxml` library, which isn't installed in the notebook's environment (`bs4` itself loads from `d:\Evidentia\.venv`, so the environment is right, only `lxml` is missing).

**Fix (done, `lxml==6.1.3` installed and added to `requirements.txt`):**
```python
%pip install lxml        # installs into the environment the notebook is running
```
then **restart the kernel** (new libraries are only picked up on restart).

**Lesson:** `%pip install ...` inside the notebook always installs into the notebook's own environment. `pip install` in a terminal without `.venv` activated installs into a *different* Python, a common cause of "I installed it but it's not found". Check with `import sys; print(sys.executable)`.

### 18:38 — Downloaded the first full-text paper (GROBID XML)

Downloaded `W2408166852.grobid.xml` (~107 KB, *Auditory-Inspired Speech Envelope Extraction Methods for Improved EEG-Based Auditory Attention Detection in a Cocktail Party Scenario*, 2016) into `data/fulltext/` via its `content_urls["grobid_xml"]` link. Costs $0.01 per file.

**Improvements to the download cell**
- `os.makedirs(folder, exist_ok=True)` creates the folder if it's missing.
- **Caching:** `if os.path.exists(path)` reuses a file that's already downloaded, so rerunning the cell costs nothing (the rerun printed "Using saved file").
- Checks `status_code == 200` before saving, and prints the error text otherwise.
- Saved as `.grobid.xml`, since it is XML, not PDF.

### 16:09–18:42 — Error: `FileNotFoundError: No such file or directory: '../data/fulltext/W4415433665.pdf'`

**Three problems in one cell:**
1. **Missing folder:** `open()` can create a file but not the folders around it. `../data/fulltext/` didn't exist. Fix: `os.makedirs(..., exist_ok=True)`.
2. **Wrong paper in the filename (silent bug):** the URL came from `d["results"][0]`, but the filename used `w`, a variable left over from an earlier `for w in ...` loop. After a loop, `w` is the *last* item, so the file would have been named after a different paper (W4415433665 instead of W2408166852), with no error at all. Fix: take one `paper` variable and use it for both URL and filename.
3. **Wrong file type:** downloaded the `grobid_xml` link but saved it as `.pdf`.

**Lesson:** after a loop, the loop variable still exists and holds the last item. Reusing it later is an easy, silent bug.

### 16:09–18:42 — Challenge: most papers have no downloadable full text

Ran semantic search (top 20) and checked `has_content` for each result:

| | Count |
|---|---|
| Results checked | 20 |
| With GROBID XML | 7 (35%) |
| With PDF | 7 (the same 7 papers) |
| No full text | 13 (65%) |

**Why:** OpenAlex can only store papers it is allowed to collect, mostly open access. Many auditory attention decoding papers are in subscription journals (e.g. IEEE journals), so only the abstract is available.

**Options noted for later** (decided to solve access later and continue with XML for now):
- check all `locations` of a paper for a free preprint/author copy (`pdf_url`), not just the best one
- Europe PMC open-access full text
- the researcher's own PDFs (university access, e.g. a Zotero library), kept locally in `data/` and never pushed
- otherwise judge on the abstract and label it clearly: "assessed on abstract only; full text not available"

**Design decision:** don't filter search results by full-text availability (that would silently drop relevant papers). Search first, then check `has_content` per paper.

**Also noticed:** semantic search results were much more on topic than keyword search. No MNE-Python software paper or motor-imagery papers, and several directly relevant AAD papers, including *Overestimated performance of auditory attention decoding caused by experimental design* (2025), a potential source of **contradicting/qualifying** evidence.

### 16:09–18:42 — Error: semantic search + `has_content` filter → `400` and `KeyError: 'meta'`

**What happened:** `search.semantic` with `filter=has_content.grobid_xml:true` crashed with `KeyError: 'meta'`.

**Debugging:** printed `r.status_code` and `d` instead of guessing:
```
Status: 400
Filter 'has_content.grobid_xml' is not supported with semantic search.
Supported filters: author.id, ..., has_abstract, has_fulltext, is_oa, is_retracted, language, publication_year, type, ...
```

**Why the KeyError:** a failed request returns an error message (`error`, `message`) instead of results, so there is no `meta` key. The `KeyError` was only a symptom; the real problem was the `400`.

**Fix:** run semantic search without that filter and check `has_content` for each result afterwards.

**Lessons**
- Read a traceback **bottom-up**: last line = the error, `---->` = the failing line; the cause is often a line or two earlier.
- Always check `r.status_code` before using `r.json()`.
- Look-alike filters mean different things: `has_fulltext` = OpenAlex used the full text for its own search (not downloadable); `is_oa` = a free copy exists somewhere (OpenAlex may not have the file). Only `has_content` means "downloadable from OpenAlex".

### 16:09–18:42 — Found papers with full text (keyword search + filter)

`search` + `filter=has_content.grobid_xml:true` works with keyword search: **14,730** matching papers have GROBID XML. The top results again included MNE-Python and off-topic EEG papers (same ranking issue as notebook 01).

**What OpenAlex offers:** 50M+ papers as PDF and ~43M as **GROBID XML** (already split into sections and paragraphs, so much easier to use than PDF). Download via `content_urls` (`https://content.openalex.org/works/{id}.grobid-xml` or `.pdf`) with the API key, **$0.01 per file** (~100 files/day on the free $1 budget, shared with searches). Downloaded papers keep their original copyright.

Sources: [OpenAlex full text](https://help.openalex.org/access/fulltext/) · [download guide](https://developers.openalex.org/download/full-text-pdfs) · [work attributes](https://help.openalex.org/data/works/attributes/)

### 16:09–18:42 — Created the notebook

Set up with the same key loading, claim and `run()` helper as notebook 02 (`n` changed to 10).
