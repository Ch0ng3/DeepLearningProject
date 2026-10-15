# Final Report

IEEE 2-column conference format, **at most 6 pages** (course requirement).

- `main.tex`: the report draft (currently 4 pages). Written with real results: Milestone 1 and all of Milestone 2 (linear probe, LoRA and full fine-tuning on DINOv2 and CLIP: Table II, Fig. 1, cost, error analysis and calibration). Red `[TODO]` markers show what is still missing: Milestone 3, conclusion, last sentence of the abstract and of the contributions.
- `refs.bib`: DomainBed, PACS, DINOv2, CLIP, LoRA, WiSE-FT.

## Compile

- **Overleaf**: *New Project → Upload Project*, upload `main.tex` and `refs.bib` (IEEEtran is available there).
- **Locally**: `pdflatex main`, `bibtex main`, `pdflatex main`, `pdflatex main` (needs the `IEEEtran`, `cite`, `multirow`, `pgfplots` packages). Build files are ignored by git.

## Where the numbers come from

Every number in the report comes from the executed notebooks in `Milestone_1/` and `Milestone_2/` (the `RESULT` lines and the official `collect_results` output) and from the experiment log in `Milestone_2/README.md`. The training-curve numbers in the full fine-tuning paragraph (sketch accuracy per checkpoint) come from the DomainBed log printed in section 6 of each run notebook.
