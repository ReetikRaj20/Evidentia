# Devlog — `02_openalex_search_features.ipynb`

Changes to this notebook and what I learned from them.
Times are local (Europe/Berlin). Newest entries at the top.
Project-wide log: [../DEVLOG.md](../DEVLOG.md)

---

**Purpose:** Try the search options from the OpenAlex documentation on the same claim as notebook 01, to see which ones find better evidence.

## 2026-09-27

### 15:42 — Created the notebook

```python
claim = "Auditory attention decoding can be decoded using EEG"
```

**Sections**
1. **Setup**: API key from the environment variable, plus a helper function `run(params, n=5)` that sends one search and prints status, number of matches, cost, and the top papers (year, citations, score, title). Keeps each experiment to one line.
2. **Three search modes**: `search` vs `search.exact` vs `search.semantic`.
3. **Query syntax**: phrases `"..."`, `AND`/`OR`/`NOT`, excluding a topic (`NOT "motor imagery"`), proximity `"..."~5`.
4. **Filters**: `has_abstract:true,type:article`, `from_publication_date:2020-01-01`, `cited_by_count:>100`.
5. **Sort**: `cited_by_count:desc`, `publication_date:desc`.
6. **Select**: compares response size with and without `select`.
7. **Group by**: number of matching papers per publication year.
8. **Paging**: `per_page` + `page` (papers 6–10).
9. **What I noticed**: empty, to fill in after running.

**What I learned from the docs while building it**
- Only **one** search mode per request: `search`, `search.exact` *or* `search.semantic`.
- `search.semantic` compares **meaning**, using an embedding model (GTE Large) over **titles and abstracts only**.
  - Max **50 results** per query, **1 request per second**, API key required.
  - Its `relevance_score` is a **cosine similarity**, so it is on a different scale from keyword-search scores.
  - Some filters (e.g. `cited_by_count`) don't work with it.
- **Cost:** normal search ≈ $0.10 per 1,000 calls, semantic ≈ $1 per 1,000. My free key gives **$1/day**, shared between both.

Sources: [search](https://developers.openalex.org/guides/searching) · [semantic search](https://help.openalex.org/api/semantic-search/) · [filters](https://developers.openalex.org/guides/filtering) · [sort](https://developers.openalex.org/guides/sort) · [costs](https://help.openalex.org/access/example-costs/) · [budget](https://help.openalex.org/access/buying-and-renewing/)
