
# Islamic Geometric Patterns through Symmetry-Structured Neural Completion

*Islamic Geometric Patterns through Symmetry-Structured Neural Completion*, by H. Ugail.

Islamic geometric patterns are governed by exact rotational symmetry and precise constructional rules, so a completion that merely looks plausible may still be structurally invalid if its symmetry is broken. Generic neural generators treat symmetry as a soft visual property and cannot certify it. **This is an open-source reference implementation of a symmetry-structured neural completion system that takes sparse control geometry, expands it into a candidate lattice organised into rotational orbits by an exact geometric partition, and predicts edge selections and bounded curve refinements that are exactly $N$-fold symmetric by construction.** Symmetry is enforced either by an orbit-tied architecture or by inference-time orbit projection, and the orbit-tied variant carries a construction guarantee of exact $C_N$ symmetry, anchor preservation, and bounded refinement for any input and any orbit-level selection policy. The guarantee is verified by a label-free rotation audit that is independent of the identifiers the system uses internally. The repository contains the trained model checkpoints and every precomputed table, so the complete evaluation reproduces without any training.

<img width="3329" height="2250" alt="raw_topology_gallery_seed7" src="https://github.com/user-attachments/assets/5b6d1f21-8704-4c57-9d6e-e72ecd7019ad" />

## What does this show?

We reproduce, across three seeds and 360 paired test patterns, the following.

1. **Exact structural validity is free.** The orbit-tied model reaches a mean edge-selection $F_1$ of 0.519, selection projection reaches 0.523, and unstructured free decoding reaches 0.520. The paired per-pattern analysis gives an orbit-tied minus unstructured difference of $-0.001$ with a bootstrap 95% confidence interval of $[-0.009, 0.007]$ and a Wilcoxon $p$ of 0.84, entirely inside a pre-specified equivalence margin of two $F_1$ points. The structured methods produce exactly zero orbit violations, while the unstructured model violates orbit consistency on 3.3 orbits per pattern even on clean input.

2. **The symmetry guarantee is verified label-free.** Rotating every selected edge by exactly $2\pi/N$ and checking that its image is also selected gives zero violations for the orbit-tied model across every one of the 360 test patterns, independently of the orbit identifiers the system uses internally. The rotational self-Chamfer distance of the rendered output agrees, placing the orbit-tied model at the sampling floor of 0.006 against 0.009 for free decoding and 0.010 for the template.

3. **Missing control geometry separates fidelity from validity.** With 30% of the input structure removed, the clean-trained unstructured model falls from 0.524 to 0.415 in $F_1$ and its violations climb from 3.3 to 37.5 per pattern, while the structured methods move only from 0.518 to 0.502 and from 0.527 to 0.509 with zero violations throughout. Training the unstructured model with missing-geometry augmentation restores its fidelity to 0.517 but leaves about nine violations per pattern, whereas the augmented structured variants hold fidelity flat near 0.52 with exactly zero violations. Augmentation recovers fidelity, symmetry structure guarantees validity, and the two compose.

4. **Orbit-tied training and inference-time projection are equivalent.** Across all 42 combinations of seed, corruption mode, and severity, the mean $F_1$ difference between the orbit-tied model and full projection is $-0.008$ with a standard deviation of 0.008, positive in only five cells. The operative ingredient is the orbit-organised candidate geometry, not the particular enforcement device, and the same fidelity holds at a held-out symmetry order where both methods reach 0.524.

5. **Overlap metrics alone cannot assess this domain.** A procedural template attains the leading $F_1$ of 0.534 through precision near 0.42 and recall near 0.78, with an edge-density error of about 0.27 against roughly 0.09 for the neural methods. Weighting precision more heavily reverses the ranking, with an $F_{0.5}$ of about 0.46 for the template against about 0.51 for each neural predictor, so calibration and validity measures are necessary companions to overlap.

## Contents

- **`run_pipeline_full.ipynb`** — the complete pipeline in a single notebook. Builds the procedural pattern generator, the candidate lattice, and the exact geometric orbit partition with its label-free rotation audit, trains or reuses all model variants including the corrupted-input augmented ones, calibrates selection policies, and runs the fidelity, validity, robustness, training-control, statistical, per-$N$, and data-efficiency studies before regenerating every figure, ornament rendering, SVG export, and CSV table.

- **`Results/models/`** — the eighteen trained checkpoints: orbit-tied and unstructured predictors for three seeds, their corrupted-input augmented variants, and the held-out-order pairs. The notebook loads any checkpoint it finds and skips that training, so the default run on a fresh clone is evaluation-only.

- **`Results/metrics/`** — the precomputed result tables for the paper. The main fidelity table is `main_results_table.csv`, the validity audit including the label-free rotation check is `structural_validity.csv`, the corruption study is `robustness.csv`, the training control is `robustness_training.csv`, the paired per-pattern records and their statistical analysis are `paired_per_pattern_f1.csv` and `paired_stats.json`, and the complete run record is `metrics_summary.json`. These are sufficient to verify every reported number without re-running anything.

- **`Results/figures/`** — the publication figures, ornament galleries, and hero panels. The strapwork rendering is presentation only, the predicted geometry is never altered, and every styled panel has its unstyled topological completion saved beside it.

- **`Results/external_sanity/`** — qualitative external checks.

- **`requirements.txt`** — dependencies (NumPy, PyTorch, pandas, matplotlib, SciPy).

## Quick start

The fastest way to check the headline numbers is to open the precomputed tables:

```
git clone https://github.com/ugail/Symmetry-Structured-IGP-Completion.git
cd Symmetry-Structured-IGP-Completion
```

Every value reported in the paper is in `Results/metrics/`, and `paired_stats.json` contains the statistical analysis with its confidence intervals and equivalence verdicts.

To reproduce the complete evaluation from the shipped checkpoints:

```
pip install -r requirements.txt
jupyter notebook run_pipeline_full.ipynb
```

Run all cells. The notebook detects the included checkpoints, skips all training, and regenerates every figure and table. No GPU is needed for this evaluation-only run. The same notebook runs unchanged in Google Colab.

## Reproducing the full pipeline from scratch

The notebook is **incremental**. Checkpoints found in `Results/models` are loaded and their training is skipped, so deleting a checkpoint file retrains exactly that model, and deleting `Results/metrics/data_efficiency.csv` recomputes the cached ablation. Emptying `Results/models` entirely retrains the whole system, which takes roughly one GPU session on Google Colab. For a quick functional check before a full run, set `smoke_mode = True` in the configuration cell.

## Method variants and metrics

The system compares **orbit-tied** decoding, which averages predictions within each rotational orbit inside the forward pass and is exactly symmetric by construction, **projection**, which applies the same orbit-level aggregation to an ordinary predictor at inference time, and **unstructured free** decoding, which selects edges independently. Augmented variants of both predictors are trained with missing-control-geometry corruption to separate what symmetry structure contributes from what the training distribution contributes. A procedural template and a nearest-neighbour baseline complete the comparison. Fidelity is measured by edge-selection $F_1$, precision, recall, and edge-density error against held-out ground truth, and validity by selected-orbit violation counts, the label-free rotation audit, anchor drift, and the refinement bound.

## Who is this for?

- **Practitioners and learners of Islamic geometric design who** want a computational companion for exploring completions of a partial construction, with every suggestion guaranteed to respect the exact rotational symmetry their craft demands, offered as an aid to traditional practice rather than a substitute for it.

- **Researchers in geometric and constraint-aware deep learning** who want a worked example of output-level symmetry guarantees on discrete selections, with the guarantee, its independent verification, and its honest cost accounting in one place.

- **Researchers in computational design and cultural heritage** who want structurally valid vector ornament generation with scalable exports suitable for fabrication workflows.


## Citation

If you use this implementation or the precomputed results, please cite the paper:

> H. Ugail, *Islamic Geometric Patterns through Symmetry-Structured Neural Completion*, 2026, Under review.


## License

Released under the MIT License. See `LICENSE` for the full text.
