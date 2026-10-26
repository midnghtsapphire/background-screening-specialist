# Background Screening Specialist

Research plan for a modular, FOSS-first background-screening research service.

## Principles
- Public-source leads are not verified background checks. Label output as "possible match" or "insufficient evidence".
- Preserve source URL, retrieval timestamp, jurisdiction, record type, disposition, and match rationale for every finding.
- Software licenses do not grant data permissions. Check dataset licenses and provider terms before use.
- Comply with applicable law (for example FCRA and consent requirements) for any employment or tenant screening.

## Candidate components
| Tool | Use | Interfaces | License / notes |
|---|---|---|---|
| SpiderFoot | Broad public-source OSINT | CLI, web UI | MIT; some modules need separate API keys |
| OpenSanctions + yente | Sanctions, PEP, and watchlist matching | HTTP API, Python client, MCP server | Software open source; curated dataset needs a commercial license |
| CourtListener / RECAP | Federal court opinions, dockets, filings | REST API v4 | Open legal data; PACER document retrieval can cost money; not a complete criminal-record check |
| Maigret | Username-based account discovery | CLI | MIT; matches are leads only |
| KarmaCheck | Purchased ID, driving, and criminal checks | Vendor MCP | Commercial |
| Checkr | Purchased screening workflows | API, Candidate MCP | Commercial; scoped authorization |

## Proposed architecture
CLI, agents, and applications call one authenticated research API. The API dispatches to adapters for CourtListener, yente, SpiderFoot, and optional Maigret workers. Results go to an evidence store and review queue. Expose the same backend through MCP tools. Add A2A only if a separate research agent needs multi-step task delegation.

## Proposed MCP tools
- search_court_records
- screen_watchlists
- discover_public_accounts
- retrieve_source_document
- compare_candidate_matches
- export_evidence_report

## Next steps
1. Define the authorized use cases and jurisdictions.
2. Build the adapter interface and evidence schema.
3. Add tests with synthetic records.
4. Add secret scanning and access controls before handling real personal data.
