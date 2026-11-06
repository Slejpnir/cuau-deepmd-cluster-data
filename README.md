# Cu–Au nanocluster DeepMD research data

Supporting data for the development and DFT-based assessment of a DeepMD potential for Cu–Au nanoclusters.

## Download the data

[Download the supporting-data archive](https://github.com/Slejpnir/cuau-deepmd-cluster-data/releases/download/v2026-11-06/CuAu_DeepMD_supporting_data_20261106.zip) from the [dated release v2026-11-06](https://github.com/Slejpnir/cuau-deepmd-cluster-data/releases/tag/v2026-11-06).

The archive contains **285 DFT-labeled configurations**: 259 training configurations, 20 exposed development configurations and 6 exposed diagnostic configurations. Model graphs, training settings, saved prediction statistics, numerical figure data and reproduction scripts are included. The principal model is `models/fit259.pb`; the two earlier research models support explicitly attributed contact-response results.

- Archive size: 33,605,313 bytes.
- Archive SHA-256: `c1cb7b1be144284dcb79741d1945eb25fd878430e31dabc7042584f86c1b333b`.
- Packaged inventory SHA-256: `a5b84dd0164724f24973f89f70ed1031bfb7ff8cc12838a4c9db7615d61056cf`.

The ZIP contains one enclosing directory, `CuAu_DeepMD_supporting_data_20261106/`. Extract it and consult [the data guide](DATA_GUIDE.md) and [reproduction instructions](REPRODUCING.md). Run the following command from the extracted directory to verify file checksums and reproduce the reported errors from saved statistics:

```
python scripts/verify_metrics.py
```

That verification command does not run DFT, training or model inference. Optional commands for fresh inference and training are described separately in the reproduction instructions.

## Scope and provenance

The development and diagnostic configurations are exposed sets, not blind independent tests. Publication of these research models is not a claim of independent validation or unrestricted dynamical transferability. Saved force-error statistics are included; complete predicted force arrays can be generated through the optional inference procedure.

The publication graphs retain all numerical model fields unchanged. Only absolute paths in training-script metadata were replaced with portable paths or opaque source identifiers. Original and publication checksums are recorded in the archive. Research source files were not modified.

Third-party pseudopotential files are not redistributed; their identifiers, checksums and official source links are supplied. Historical raw QE outputs, wavefunctions, optimizer checkpoints and the customized QE executable are not included. The archive contains no private administrative records or unrelated experiments.

## Funding

This research was supported by a grant of the President of Ukraine provided by the National Research Foundation of Ukraine (project No. 2025.04/0003).

## Rights

No author-selected reuse license is currently attached. Public accessibility does not itself grant a reuse license. Third-party software and libraries retain their respective licenses. Cite the associated research article when available and identify the dated release and archive checksum used.
