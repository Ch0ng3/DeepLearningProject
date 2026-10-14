# Final Report

IEEE 2-column conference format, **at most 6 pages** (course requirement).

- `main.tex`: the report draft (currently 3 pages). Written with real results: Milestone 1, the linear probes of Milestone 2 (DINOv2 94.5, CLIP 96.7), error analysis and calibration. Red `[TODO]` markers and red dashes show what is still missing: LoRA and full fine-tuning results, Milestone 3, conclusion.
- `refs.bib`: DomainBed, PACS, DINOv2, CLIP, LoRA, WiSE-FT.

## Compile

- **Overleaf**: *New Project → Upload Project*, upload `main.tex` and `refs.bib` (IEEEtran is available there).
- **Locally**: `pdflatex main`, `bibtex main`, `pdflatex main`, `pdflatex main` (needs the `IEEEtran`, `cite`, `multirow`, `pgfplots` packages). Build files are ignored by git.

## Where the numbers come from

Every number in the report comes from the executed notebooks in `Milestone_1/` and `Milestone_2/` (the `RESULT` lines and the official `collect_results` output) and from the experiment log in `Milestone_2/README.md`. When the remaining Milestone 2 runs finish, fill Table II, Fig. 1 and the TODO paragraphs from `M2_results.ipynb`.
