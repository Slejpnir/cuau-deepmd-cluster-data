# Cu–Au DeepMD independent test and manuscript update

This dated update adds six DFT reference configurations and the revised English and Ukrainian journal manuscripts. The frozen principal model from the [November 6 release](https://github.com/Slejpnir/cuau-deepmd-cluster-data/releases/tag/v2026-11-06) was evaluated without retraining, an energy-offset fit, or test-based checkpoint selection.

## Files

- [English manuscript](CuAu_DeepMD_MSMSE_balanced_English.docx)
- [Ukrainian manuscript](CuAu_DeepMD_MSMSE_balanced_Ukrainian.docx)
- [Six-configuration test archive](CuAu_DeepMD_independent_test6_20261108.zip)
- [File checksums](SHA256SUMS.txt)
- [Machine-readable update manifest](update_manifest.json)

The manuscripts are working journal articles, not records of journal acceptance or peer review. Their scientific content matches the reviewed local revision of 8 November 2026.

## Configuration roles

The original database remains 285 configurations: 259 fitting, 20 exposed development, and 6 exposed diagnostic configurations. The six new test configurations are deposited separately. Across the two deposits there are 291 reference configurations, but the model was fitted to 259, not 291. The original data, model graphs, and dated release have not been changed.

The new test contains four metal-only configurations from Ar13 collision trajectories and two unrelaxed Cu28Au27 cluster seeds. Each contains 55 atoms; all 330 atoms and 990 Cartesian force components are included. The model was frozen before the new reference labels were available. The six records were excluded from fitting and model selection. These are six static configuration tests, not six independent dynamical trajectories or a test of Ar–metal interactions.

## Results and reference protocol

The overall energy RMSE is 31.642 meV/atom and the pooled Cartesian force-component RMSE is 0.17425 eV/Å. The four collision snapshots give 34.951 meV/atom and 0.21123 eV/Å; the two unrelaxed seeds give 23.674 meV/atom and 0.04301 eV/Å. Two collision snapshots contribute 90.43% of the total squared force error. No configuration was removed from the aggregate scores.

QE references use PBE, 80/320 Ry cutoffs, Gamma-only sampling, MV smearing of 0.02 Ry, the Martyna–Tuckerman correction, a 1e-7 Ry SCF threshold, Davidson diagonalization, and 328 bands. A 24 Å cell was adopted. For the first geometry, the 24-to-26 Å comparison differed by 1.098 meV/atom in energy and 0.00402 eV/Å in force-component RMS; the energy difference exceeded the 1 meV/atom target. Full cell-size convergence for all six references is not established. The reported model errors refer to these adopted finite-cell references.

These results measure the unchanged model; they do not demonstrate an accuracy improvement or promote a new potential. If the test errors guide later model changes, this set becomes exposed validation for those later models.

## Reproduce the reported errors

Extract the test ZIP into its own directory and run:

```sh
python recompute_metrics.py
```

Only the saved reference and prediction arrays are read. This script does not launch QE, DeepMD inference, or training. Energy RMSE is calculated from configuration energy errors divided by atom count. Force RMSE pools the Cartesian component errors. The archive includes the coordinates, species, reference and predicted energies and forces, per-configuration errors, provenance hashes, summary metrics, and an internal SHA-256 manifest. Raw QE execution outputs and third-party pseudopotentials are not redistributed in this companion archive.

The original numerical model SHA-256 is `02e35a94635dc716866b104f13d3dd037f44a9c48c199b20ff67c8f5a1053301`. Its public equivalent is `models/fit259.pb` in the November 6 archive. The public graph has portable training metadata; the mapping to the original graph is recorded in `models/publication_metadata_changes.json` in that archive.

## Funding and rights

This research was supported by a grant of the President of Ukraine provided by the National Research Foundation of Ukraine (project No. 2025.04/0003).

No author-selected reuse license has been assigned. Public accessibility does not itself grant a reuse license. The earlier release and its rights statements remain unchanged.
