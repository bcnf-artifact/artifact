# Artifact: Efficient Subset-Repair Checking for a Relation Schema in BCNF

Code, data, and results for the paper of that title. The repository holds both
components the paper depends on, as sibling directories:

- `experiments/` — the checker implementations, the incremental-maintenance
  engine, the generators, the experiment entry points, and every measurement
  behind Figs. 2-5 and the 165.7x speedup in Section 4.4.
- `llm_study/` — the frozen question-answering dataset with its SHA256 manifest,
  the evaluation code, and the per-request LLM records behind Fig. 6 and
  Tables 1-2.

They must stay siblings: `llm_study` imports the checker from `../experiments`.

## Verify without an API key or network access

```bash
pip install -r experiments/requirements.txt
cd experiments && python -m pytest tests -q          # 623 tests
cd ../llm_study && python -m pytest tests -q         # 13 tests
python scripts/validate_frozen_dataset.py            # dataset SHA256

# Redraw Figs. 2-6 from the shipped measurements
cd ../experiments && python supplement/figures/plot_results.py
```

Everything above runs offline. No model credentials are needed to reproduce any
number, table, or figure in the paper.

## Where the reported numbers live

| Paper item | Data |
|---|---|
| Fig. 2 (static scalability, key-count control) | `experiments/supplement/data/static_summary.csv`, `static_separate_allocation.csv` |
| Fig. 3 (incremental maintenance) | `experiments/supplement/data/fig4_incremental_summary.csv` |
| Fig. 4 (update locality) | `experiments/supplement/data/hot_summary.csv` |
| Fig. 5 (SQL baseline) | `experiments/supplement/data/tpch_summary.csv` |
| Fig. 6, Tables 1-2 | `llm_study/results/final/` |

`experiments/results/final/static.csv` is an earlier static run whose timings
include allocation tracking. The paper reports the separate-instrumentation
numbers in `supplement/data/`; the two sets are not interchangeable.
`experiments/supplement/README.md` documents the protocol and the exact values
quoted in the paper.

## License

MIT, see the `LICENSE` file in each directory.
