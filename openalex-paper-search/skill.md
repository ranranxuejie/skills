---
name: openalex-paper-search
description: "Academic paper search via OpenAlex (240M+ scholarly works). Use for: find papers on X, academic research, literature review, citations, scholarly articles, papers by author, highly cited papers. NOT for general web search."
---

# OpenAlex Paper Search

Free REST API, no SDK. Use `curl` with URL construction. All responses are JSON.

**Docs:** https://docs.openalex.org

**Base URL:** `https://api.openalex.org`

**Rate limits:** Add `mailto=agent@kortix.ai` to every request for 10 req/s (polite pool). Without it: 1 req/s. Optional: `&api_key=YOUR_KEY` for premium access.

## Quick Reference

```bash
# Basic search
curl -s "https://api.openalex.org/works?search=transformer+architecture&per_page=5&mailto=agent@kortix.ai"
```

## Endpoints

| Entity | Endpoint | Use for |
|--------|----------|---------|
| Works | `/works` | Papers, articles, books, datasets |
| Authors | `/authors` | People who create works |
| Sources | `/sources` | Journals, conferences |
| Institutions | `/institutions` | Universities, orgs |
| Topics | `/topics` | Research topics |

## Key Work Fields

`id`, `doi`, `display_name` (title), `publication_year`, `cited_by_count`, `fwci` (field-weighted impact), `type` (article/preprint/review/book), `open_access.is_oa`, `open_access.oa_url`, `authorships` (authors + institutions), `abstract_inverted_index`, `referenced_works`, `cited_by_api_url`, `topics`, `keywords`, `primary_location`, `best_oa_location`

## Search

```bash
# Keyword
curl -s "https://api.openalex.org/works?search=large+language+models&per_page=10&mailto=agent@kortix.ai"

# Boolean (AND, OR, NOT, parens, "quoted phrases")
curl -s "https://api.openalex.org/works?search=(reinforcement+learning+AND+%22robot+control%22)+NOT+simulation&mailto=agent@kortix.ai"

# Field-specific
curl -s "https://api.openalex.org/works?filter=title.search:transformer&mailto=agent@kortix.ai"
curl -s "https://api.openalex.org/works?filter=abstract.search:protein+folding&mailto=agent@kortix.ai"
curl -s "https://api.openalex.org/works?filter=title_and_abstract.search:neural+scaling+laws&mailto=agent@kortix.ai"
curl -s "https://api.openalex.org/works?filter=fulltext.search:climate+tipping+points&mailto=agent@kortix.ai"
```

## Filters

Combine with `,` (AND) or `|` (OR). Prefix `!` for NOT.

```bash
# Publication year
?filter=publication_year:2024
?filter=publication_year:2020-2024
?filter=publication_year:>2022

# Citations
?filter=cited_by_count:>100
?filter=cited_by_count:>1000

# Open access
?filter=is_oa:true
?filter=oa_status:gold

# Type
?filter=type:article|preprint|review

# Other
?filter=language:en
?filter=is_retracted:false
?filter=has_abstract:true
?filter=has_content.pdf:true
?filter=author.id:A5023888391
?filter=institutions.id:I27837315
?filter=doi:https://doi.org/10.1038/s41586-021-03819-2
?filter=indexed_in:arxiv|pubmed|crossref
```

**Combining:** `?filter=publication_year:>2022,cited_by_count:>50,is_oa:true,type:!preprint`

## Sorting

`?sort=cited_by_count:desc` | `?sort=publication_date:desc` | `?sort=relevance_score:desc`

Multiple: `?sort=publication_year:desc,cited_by_count:desc`

## Pagination

```bash
# Basic (max 10k results)
?page=1&per_page=25

# Cursor (unlimited)
?per_page=100&cursor=*           # first page
?per_page=100&cursor=<next>      # cursor from response.meta.next_cursor
```

## Select Fields

Reduce response size: `?select=id,display_name,cited_by_count,doi,publication_year`

## Reconstruct Abstracts

OpenAlex stores abstracts as inverted indexes. To get plaintext:

```python
inv_idx = work["abstract_inverted_index"]
if inv_idx:
    words = [""] * (max(max(positions) for positions in inv_idx.values()) + 1)
    for word, positions in inv_idx.items():
        for pos in positions: words[pos] = word
    abstract = " ".join(words)
```

## Citation Graph

```bash
# What a paper cites (outgoing)
curl -s "https://api.openalex.org/works?filter=cited_by:W2741809807&per_page=25&mailto=agent@kortix.ai"

# What cites this paper (incoming)
curl -s "https://api.openalex.org/works?filter=cites:W2741809807&sort=cited_by_count:desc&per_page=25&mailto=agent@kortix.ai"

# Related works
curl -s "https://api.openalex.org/works?filter=related_to:W2741809807&per_page=25&mailto=agent@kortix.ai"
```

## Author Lookup

```bash
curl -s "https://api.openalex.org/authors?search=Yann+LeCun&mailto=agent@kortix.ai"
curl -s "https://api.openalex.org/works?filter=author.id:A5064850633&sort=cited_by_count:desc&per_page=10&mailto=agent@kortix.ai"
curl -s "https://api.openalex.org/authors/orcid:0000-0001-6187-6610?mailto=agent@kortix.ai"
```

## Lookup by External ID

```bash
curl -s "https://api.openalex.org/works/doi:10.1038/s41586-021-03819-2?mailto=agent@kortix.ai"
curl -s "https://api.openalex.org/works/pmid:14907713?mailto=agent@kortix.ai"
# Batch (up to 50): filter=doi:DOI1|DOI2|DOI3
```

## Open Access PDF

```bash
curl -s "https://api.openalex.org/works?search=quantum+computing&filter=is_oa:true,has_content.pdf:true&select=id,display_name,open_access,best_oa_location&per_page=5&mailto=agent@kortix.ai"
```

## Workflow Examples

```bash
# Literature survey
curl -s "https://api.openalex.org/works?search=retrieval+augmented+generation&sort=cited_by_count:desc&filter=publication_year:>2020,type:article,has_abstract:true&per_page=20&select=id,display_name,publication_year,cited_by_count,doi,authorships,abstract_inverted_index&mailto=agent@kortix.ai"

# Landmark papers
curl -s "https://api.openalex.org/works?search=attention+mechanism&filter=cited_by_count:>500,type:article&sort=cited_by_count:desc&per_page=10&select=id,display_name,publication_year,cited_by_count,doi&mailto=agent@kortix.ai"

# Recent preprints
curl -s "https://api.openalex.org/works?search=multimodal+LLM&filter=type:preprint,publication_year:2025&sort=publication_date:desc&per_page=15&mailto=agent@kortix.ai"

# Review articles
curl -s "https://api.openalex.org/works?search=federated+learning&filter=type:review,cited_by_count:>20&sort=cited_by_count:desc&per_page=10&mailto=agent@kortix.ai"
```

## Tips

- Use `select` to reduce response size
- Use `per_page=100` to minimize requests
- Use cursor paging for >10k results
- Batch DOI lookups: `filter=doi:D1|D2|D3` (up to 50)
- Reconstruct abstracts from `abstract_inverted_index`
- Follow citation chains: get seminal paper -> find its references -> find who cites it
- Filter `has_abstract:true` when abstracts are needed
- Filter `indexed_in:arxiv` or `indexed_in:pubmed` for specific repos
- Sort `cited_by_count:desc` for most influential papers
