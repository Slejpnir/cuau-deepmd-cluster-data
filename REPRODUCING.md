# Reproducing the numerical results

Use Python 3.12, NumPy, DeepMD-kit 3.1.3 TensorFlow backend and TensorFlow 2.21.0
for compatibility with the research fits. Downloading this package starts no
calculation. The saved metric verification needs only Python's standard library:

```
python scripts/verify_metrics.py
```

To request new inference explicitly, use:

```
python scripts/infer_model.py --model models/fit259.pb --output new_inference.json
```

This optional command evaluates the 285 included configurations and saves the
full predicted energies and forces, together with the three dataset-role RMSEs.
It does not train or run DFT. Its implementation follows the same isolated vs
periodic boundary handling as the original evaluation. Numerical roundoff may
depend on software build and hardware. Current roles are not independent tests.

The recipe used for each research fit was a fresh initialization from the common
pretrained graph, with no optimizer or step continuation, the ordered sampling
weights in the corresponding input file, and one fixed 10000-step endpoint.
The conceptual DeepMD commands, run from the package root in a separate copy,
are:

```
dp --tf train settings/fit259_training_input.json --init-frz-model models/initialization.pb -o out.json
dp --tf freeze -c model.ckpt-10000 -o reproduced_fit259.pb
```

These commands are shown for reproducibility, not run by the package verifier.
Perform training only on a machine with resources selected by its owner.
The source workflow used CPU execution, TF intra/inter thread counts 4/1,
OMP and BLAS thread counts 1. No minibatch validation, post-training bias change,
checkpoint choice or seed search was used. After any retraining, compare the
87 normalization/indexing/cutoff/atomic-reference control tensors to the included
initializer. Reproducing the reported endpoint is not established by successful
training alone.

```
python scripts/check_protected_controls.py
```

This checks all three packaged endpoints. Final-layer fitting biases were allowed
to train; they must not be confused with the protected atomic-energy reference
biases. The descriptor and fitting weights were trainable in all three fits.

The historic initialization graph's original training data are not separately
reproduced as a distinct training recipe here; the exact learned numerical state
needed to initialize these research fits is provided. References for the current
285 systems are all supplied, along with each system's protocol metadata.

The latest 16 QE inputs are provided as exact inputs, not as a complete binary
reconstruction of every historical reference calculation. Their execution used
the recorded QE 7.6 augmentation-only-v2 build. This executable is not bundled.
The accepted labels and reference arrays, rather than a claim of bitwise DFT
reproduction across builds, are the numerical references for model evaluation.
