# Interactive demos

Two self-contained notebooks that run from the shipped checkpoints with no training. CPU is sufficient, and on Google Colab the repository is cloned automatically.

**`visual_showcase.ipynb`** renders the method's behaviour rather than tabulating it. Demo A completes the same control geometry with the orbit-tied model and with unstructured free decoding, rendering both as strapwork with line weights and colour and printing the label-free rotation audit for each. Demo B exposes an adjustable `MISSING_FRACTION` parameter, so the effect of removing control geometry on fidelity and validity can be explored interactively. Demo C shows failure cases honestly, including symmetry-broken chaotic output from the unstructured model under severe corruption, the valid-but-sparse structured recovery, and the weakest structured completion from a small batch shown as-is. Demo D builds a tile-field application mockup from the vector polylines of a curated completion and exports it as PNG and SVG.

**`generate_svg_assets.ipynb`** regenerates the scalable vector asset library in `Results/svg/`, exporting ornament hero panels as SVG and EPS, plain line-completion SVGs for every symmetry order, and reference galleries. Every SVG is parse-validated after export. The SVG files are true vector paths derived from the model's selected, refined polylines, suitable for laser cutting, plotting, print, and editing in Inkscape or Illustrator.

**`historical_case_study.ipynb`** completes control geometry transcribed by hand from documented construction systems at orders eight, ten, and twelve, using the root-two, golden-ratio, and root-three proportions of the octagonal, decagonal, and dodecagonal polygonal techniques. The transcribed proportions are editable parameters, every completion is verified by the label-free rotation audit, and the notebook produces the qualitative case-study figure together with SVG exports of each completion.

All showcase outputs are written to `demo/outputs/`.
