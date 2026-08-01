# kb-eval-harness

Measuring how the **organisation of knowledge** affects factual fidelity, hallucination
rate and honest refusals in LLM-generated content.

**Status — August 2026:** scaffolding and methodology are in place. No experiment has run yet.
Next up: a synthetic brand with its fact ledger, then the first two arms end to end.

## The question

Comparing a "structured knowledge base" against "RAG" as whole systems answers very little.
The structured side carries several mechanisms at once — knowledge in context, canonical
organisation, an access rule for numbers, integrity validation. If it wins, you cannot tell
which part did the work, and a reviewer will rightly ask whether adding the same guardrail
to the RAG prompt would erase the difference.

So this is built as an **ablation ladder**, where each rung adds exactly one thing:

| Arm | Knowledge is… | The gap from the previous rung answers |
|---|---|---|
| **A0** | chunked, embedded, retrieved (hybrid + rerank) | baseline |
| **A1** | dropped into context, unstructured | does context alone beat retrieval? |
| **A2** *(optional)* | organised into markdown **by a model** | is cheap automatic curation enough? |
| **A3** | organised into markdown **by hand** | **does curation buy anything?** |

Prompt-level rules — for example *"only cite numbers found in the source, otherwise say you
don't know"* — are applied to **every** arm, so the difference cannot come from one side
simply having a guardrail. Properties that cannot exist without structure, such as verifiable
referential integrity and modelled coverage, are reported as findings rather than smuggled in
as configuration.

## Method

Every arm starts from the **same raw material**: the kind of messy input a real client hands
over — a discovery-call transcript, website copy, a few emails, survey results. Brands are
**synthetic**, which gives full control over ground truth and keeps personal data out entirely.

Evaluation runs on three levels:

1. **Automatic checks** — required facts present, invented numbers absent, refusals where the
   answer genuinely is not in the source.
2. **LLM-as-judge** — for what code cannot score, such as brand voice and coherence, against
   an explicit rubric.
3. **Judge validation** — around 40 answers rated blind by three people; agreement with the
   judge is measured and reported before the judge is trusted.

Results will be published here as runs complete. The first full run is targeted for August 2026.

## Repository layout

```
src/kbeval/
  systems/     one shared interface, one file per arm
  eval/        golden dataset, judge, rubric, metrics
data/
  brands/      synthetic brands, three knowledge-base sizes (S / M / L)
  golden/      fact ledger, hallucination traps, task definitions
results/       versioned run outputs
tests/
```

## Getting started

```bash
python -m venv .venv
.venv\Scripts\activate          # Windows
pip install -e ".[dev]"
copy .env.example .env          # then fill in your API keys
pytest
```

## Roadmap

- [ ] Synthetic brand and raw source material, three sizes
- [ ] Fact ledger and hallucination traps (golden dataset)
- [ ] Task set: 5–10 realistic content tasks
- [ ] Arms A1 and A3 with automatic scoring — first end-to-end run
- [ ] Arm A0 (RAG): chunking, embeddings, Chroma, hybrid retrieval, reranking
- [ ] LLM-as-judge and judge validation against blind human ratings
- [ ] Full run across three knowledge-base sizes, cost crossover analysis

## Context

The practical part of a master's thesis in Computer Science (WSB Merito Gdańsk, 2026),
following a Design Science Research methodology. Written with Claude Code as an explaining
and reviewing tool; the implementation is hand-written — see [CLAUDE.md](CLAUDE.md).

The code is MIT-licensed. The thesis text itself is not covered by that licence.
