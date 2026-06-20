# Dissertation draft

This folder contains an evidence-based first draft for the University of Birmingham MSc project:

- `dissertation_draft.md`: editable source
- `references.bib`: programmatically verified bibliography metadata
- `style.css`: A4 print styling
- `figures/`: vector architecture diagrams
- `build/dissertation_draft.html`: generated review copy
- `../output/pdf/melanoma_multimodal_msc_dissertation_draft.pdf`: generated PDF

## Rebuild

From the repository root:

```sh
pandoc dissertation/dissertation_draft.md \
  --standalone --toc --toc-depth=3 \
  --citeproc --bibliography=dissertation/references.bib \
  --css=../style.css \
  --resource-path=.:dissertation \
  -o dissertation/build/dissertation_draft.html
```

Open the HTML in a browser or print it to PDF with an A4 page size. The committed PDF is a review artifact; edit the Markdown source, not the generated HTML.

## Submission warning

This is a first draft, not a submission-ready dissertation. Search for `INSERT`, `AUTHOR TO COMPLETE`, `Verification requirement`, `Missing implementation detail`, and unchecked items in Appendix A. The exact 2025/26 School of Computer Science project handbook on Blackboard overrides the provisional formatting used here.
