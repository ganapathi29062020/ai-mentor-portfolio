# ai-mentor-portfolioDay 6 — Capstone Sprint 1: PlacementDataProcessor
Engineer Answer
PROBLEM — JDs from Naukri / LinkedIn are messy text — placement cells need structured data to filter ("which JDs want Java + CGPA 7+?"). Manual extraction is unscalable for 50+ JDs.

ARCHITECTURE — JD URL → BeautifulSoup scraper (extract clean text) → Gemini structured-output call (response_schema=JD Pydantic) → JSON Lines file. Validation at each step; retry on schema fail.

TRADE-OFFS —

Cost: free Gemini ~1 JD/sec on average; ~30K tokens/day quota → ~5K JDs/day.
Accuracy: Pydantic catches schema violations but not semantic errors (e.g., model says skill is "Python" when source says "Python 3.12 specifically").
Latency: ~2-5s per JD (Gemini call dominant).
Complexity: scraping fragile (sites block automation). Cached fallback is mandatory.
SCALE —

10 JDs/day: trivial. Today's lab.
100 JDs/day: still in free quota. Add overnight batch + sleep between calls.
10K JDs/day: free tier breaks. Move to paid Gemini OR self-host an open model.
INTERVIEW ANSWER — "I built a structured-output pipeline that turns scraped JDs into clean filterable JSON, using free Gemini and Pydantic. Schema-first design with retry-on-failure made it production-shaped on a free-tier API."

Files
Day6_PlacementProcessor.ipynb — the notebook
data/jds.jsonl — output of this sprint, input for Day 7 RAG
