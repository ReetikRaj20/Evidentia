# Devlog — `01_openalex_search.ipynb`

Changes to this notebook and what I learned from them.
Times are local (Europe/Berlin). Newest entries at the top.

---

**Purpose:** Send a claim to OpenAlex, understand the structure of the response, and see how far the search ranking gets me towards finding evidence.

**Next:** Try `search` vs `search.semantic` vs a short keyword query for the same claim and compare the top 10 papers. Improve the claim wording (currently "Auditory attention decoding can be decoded using EEG", which says "decoding" twice; probably "Auditory attention can be decoded from EEG").

## 2026-09-27

### 14:59 — Added a Markdown cell: "Relevance score vs. evidence"

Written into the notebook by Claude at my request. Summarises what `relevance_score` is and isn't, its drawbacks for Evidentia, my sentence-overlap workaround and its limitations, and what that points to next (stemming → embeddings → a model that judges support/contradiction).

### 13:44–14:59 — Tried to find *which sentence* matches the claim (cell 13)

**What I did:** Rebuilt abstracts from `abstract_inverted_index`, split them into sentences, and ranked sentences by how many (non-stop) words they share with the claim.

**What I learned**
- OpenAlex does **not** say which sentence or words produced the `relevance_score`. It is a whole-paper ranking score, not a confidence that the paper supports the claim.
- My word-overlap ranking is a crude first version of "evidence highlighting":
  - The most relevant paper (*Attentional Selection in a Cocktail Party Environment Can Be Decoded from Single-Trial EEG*) had its best sentence share only 2 words with the claim.
  - A less central paper (*A Comparison of Regularization Methods…*) shared 4 words.
  - So word overlap ≠ relevance, and it can't see negation ("could **not** be decoded") or paraphrases.

### 13:44–14:59 — Looked at `relevance_score` for the top 10 results (cell 12)

**Observations**
- #1 was *Attentional Selection in a Cocktail Party Environment Can Be Decoded from Single-Trial EEG* (score ≈ 1523): exactly on topic.
- #2 was *MEG and EEG data analysis with MNE-Python* (≈ 964): a software paper, not evidence for the claim. Likely ranked high because it is very highly cited (the score is boosted by citation count).
- Some results are off-topic (e.g. motor-imagery decoding, short-term memory), matched only because they share words like "decoding" and "EEG".
- The scores have no fixed scale, so they can't be compared across different searches.

### 13:44–14:59 — Explored the shape and size of the response (cells 4–10)

**Findings**
- `data` is a `dict` with keys `meta`, `results`, `group_by`.
- `meta["count"]` = **16,765** matching works for this claim.
- `meta["x_query"]` shows that `search` was run as a **full-text** search (`fulltext.search`).
- `meta["cost_usd"]` = **$0.001** per request (so ~1,000 searches use my $1 daily free budget).
- 10 results = **~321 KB**, returned in **~0.65 s**. Each paper is large, which is why the `select` parameter will be useful later.
- Nested fields: `authorships` is a list of dicts (one per author), `primary_location` says where it was published, `abstract_inverted_index` is a dict of word → positions (not plain text).

### 13:44–14:59 — Fixed `per-page` → `per_page`

**Problem:** I first used `"per-page": 10` (a wrong spelling). OpenAlex uses snake_case, so the correct parameter is `per_page`. A misspelled parameter can be silently ignored (no error, just the default of 25 results).

**Check:** After the fix, `len(data["results"])` = 10 and `meta["per_page"]` = 10.

Added a Markdown cell explaining what `per_page` is and how paging works.

### 13:32 — Changed the test claim

```python
claim = "Auditory attention decoding can be decoded using EEG"
```

### 13:32 — Renamed notebook to `01_openalex_search.ipynb`

Renamed from `01_semantic_scholar_search.ipynb` with `git mv`, so Git records it as a rename and keeps the file's history, and so the name matches what the notebook now does.

**Error on the way:** the rename left a stale `.git/index.lock` file behind (the tool doing the rename wasn't allowed to delete files). A leftover lock makes the next Git command fail with *"Another git process seems to be running"*. Fix: delete `.git/index.lock` once you're sure no Git command is running.

### 12:53 — Created the notebook

Notebooks go in their own `notebooks/` folder, named with a two-digit number plus the question they explore, so they sort in the order I made them.
