# Cu Au nanocluster DeepMD research data

This package contains the configurations, reference labels, trained potentials,
saved prediction statistics and numerical figure data supporting the manuscript
on development and DFT-based assessment of a DeepMD potential for Cu–Au
nanoclusters. The principal model is `models/fit259.pb`. Earlier `fit235` and
`fit243` models support the explicitly attributed contact-response results.

## Data and roles

`dataset/frames.json` identifies all 285 configurations and the path to each
DeepMD NumPy system. There are 259 training configurations, 20 exposed development
configurations and 6 exposed diagnostic configurations. No blind independent test
set is included. The latter two roles were not fitted but were already available
to development and interpretation. Near-related geometries are not independent
samples. Periodic systems have 32 atoms; isolated systems have 55 or 147 atoms.

The saved arrays use angstroms for coordinates and cells, eV for total energies,
and eV/angstrom for Cartesian forces. `type_map.raw` defines Cu then Au.
An empty `nopbc` marker identifies an isolated system; it must be respected when
using DeepMD inference. Finite isolated-cell metadata must not be treated as
periodic boundary conditions. Each system contains one configuration.

The training dataset contains 219 ordinary configurations, 32 prescribed-contact
configurations and 8 collective shear configurations. The 16 latest geometries
are 8 pair contractions and 8 separate 4% xz shear deformations from 8 TRAIN parents.
They are static, correlated training configurations, not new independent tests.
Per-configuration reference protocols and lineage are retained in the manifest;
the reference population includes historical and later QE protocols.

## Reproducing reported numbers

Run `python scripts/verify_metrics.py` from the package root. This reads the saved
per-frame energy errors and force squared-error sums, reproduces the principal
cohort RMSEs, checks counts and validates every packaged file against its SHA256.
No DFT, training or model inference is performed by this command.

Energy RMSE is the square root of the equal-frame mean of
`((predicted_total_energy - reference_total_energy) / atom_count)^2`.
Force RMSE is the square root of the sum of Cartesian force-error squares divided
by the total number of Cartesian force components. No fitted energy offset,
force projection or training sampling weight is applied to these metrics.

`results/*_frame_errors.json` contains saved per-frame prediction energies and
force-error sums. It does not contain full predicted force arrays. All reference
force arrays and the equivalent model graphs are included, allowing fresh
inference with a compatible DeepMD installation. Saved derivatives and cluster
energy increments are in `results/*_contact_and_shear.json`. Their units and
sign conventions are retained: dE/dr is negative for a repulsive pair response;
the plotted positive response is its negative. Energy increments concern the
whole cluster, not a fitted pair interaction.

`figures/numeric_data.json` contains the exact plotted values; `figures/*.png`
contains the publication figure payloads. `python scripts/plot_figures.py`
produces portable SVG versions from those values, preserving numerical content
but not promising pixel-identical typography.

## Models and training information

The research fits were produced using DeepMD-kit 3.1.3 with TensorFlow 2.21.0.
Publication graph copies differ from the frozen originals only in the
`train_attr/training_script` string constant: training-system paths were changed
to package-relative locations or opaque source aliases for historic locations.
Every other graph field and numerical tensor was
checked byte-for-byte unchanged. Original and publication hashes are recorded
in `models/publication_metadata_changes.json`. Original model files were not
modified. No model execution was required to make this metadata-only copy.

`settings/*_training_input.json` preserves each fit's configuration and ordered
sampling probabilities with portable dataset paths. The models were each
initialized from the same pretrained model with a fresh optimizer, not trained
from random weights or sequentially continued from the previous fit.
The common initializer is included as `models/initialization.pb`; it is supplied
for reproducibility, not as a comparison result. The 87 protected controls are
recorded in `settings/*_protected_controls.json` and are contained numerically in
the initializer. `scripts/check_protected_controls.py` checks these exact
controls in the packaged graphs without executing a TensorFlow session.
`REPRODUCING.md` describes the training recipe and an optional fresh-inference
command. Bitwise training reproducibility across different hardware or library
versions is not promised. The provided graphs and labels support inference and
reproduction of the saved statistical reductions.

These are research models. This package does not establish independent
validation, long-trajectory stability, experimental agreement or unrestricted
transferability. Models have not been promoted to a production potential by
publication of this package.

## QE references and third-party material

The exact portable QE inputs, accepted label data and geometries for the latest
16 labels are in `qe/jobs`. They use PBE, 80/320 Ry cutoffs, Marzari–Vanderbilt
smearing 0.02 Ry, SCF threshold 1e-7 Ry, Davidson diagonalization, 328 bands,
non-spin-polarized neutral Gamma-point isolated calculations with the
Martyna–Tuckerman treatment. This is not the protocol of every historical label.
Raw wavefunctions, scratch files and optimizer checkpoints are omitted.

Third-party pseudopotential binaries are not redistributed. Their exact names,
hashes and official library links are in `qe/pseudopotentials.json`.
PSLibrary: https://dalcorso.github.io/pslibrary/
GBRV: https://www.physics.rutgers.edu/gbrv/
Place the hash-matched UPFs in `qe/pseudo` before independently running the
included QE inputs. No external binary or third-party Python library is bundled.

## Rights and citation

Use the associated manuscript citation when available. No reuse license has
been selected by the author for this package yet; public accessibility does not
itself grant a reuse license. Third-party software and potential libraries retain
their own licenses. Dataset and model provenance is described here without
redistributing private administrative records or unrelated research.
