# gw-kn-multimodal-preview

[中文](README.md) | [English](README_EN.md)

`gw-kn-multimodal` is a multimodal research repository for kilonova (KN) identification, covering the full workflow from simulated data generation and HDF5 dataset construction to GW+optical joint training, optical-only baseline training, and evaluation.

This repository currently provides a preview description only and does not include source code yet. The full source code will be released at an appropriate time.

## Repository Goals

This repository mainly addresses three tasks:

1. Organize gravitational-wave event information and optical photometric sequences into a unified training dataset.
2. Train joint GW + optical models for KN matching, retrieval, and classification.
3. Train optical-only baselines (without GW input) and analyze their generalization on real/simulated negative samples.

The core research focus in the current code includes:

- Simulated BNS / NSBH events and their corresponding kilonova light curves.
- GW scalar parameters and skymap representations.
- Optical sequence representations based on luptitude, first-detection alignment, and time-shift modeling.
- Multimodal ALBEF / contrastive-learning training and optical-only classification baselines.

## Dependencies and Runtime Environment

Python dependencies are listed in `requirements.txt` and can be installed with:

```bash
pip install -r requirements.txt
```

Main dependencies: Python 3.10+, PyTorch, h5py, numpy, pandas, astropy, healpy, ligo.skymap, tqdm, matplotlib, optuna, tensorboard, graphviz.

Additional system dependencies: jq, Slurm (HPC environment), SNANA + opsimsummaryv2 (required only for data generation).

Running in an HPC/Slurm environment is recommended. Existing `.sh` entry scripts are written for Slurm submission and resource allocation by default.

## Data and Path Conventions

### Data Directory Structure

| Purpose | Path |
|------|------|
| Multimodal training HDF5 | `$BASE_DIR/data/ALBEF_dataset/` |
| Optical-only training HDF5 | `$BASE_DIR/data/Optical_Only_dataset/` |
| Model checkpoints | `$BASE_DIR/data/model/` |
| Skymap / SNANA data | `$BASE_DIR/data/` and `$BASE_DIR/SNANA/` |

## Common Workflows

### 1. Build GW + Optical Joint Dataset

This workflow organizes GW parameters, skymaps, and optical light curves for BNS / NSBH into a single HDF5.

### 2. Train Multimodal GW + Optical Model

The training script reads data paths, negative-sample paths, time-shift settings, model hyperparameters, and checkpoint directories from JSON.

### 3. Evaluate Multimodal Model

Supports retrieval, classification, OOD monitoring, and negative-sample time-shift evaluation.

### 4. Build Optical-only Dataset

This workflow applies first-detection alignment, same-band merging within 2-hour windows, and luptitude transformation to produce optical-only HDF5.

### 5. Train Optical-only Baseline

After training, the script automatically calls the optical-only evaluation script.

## Data Access

This repository does not include training data or large simulation files. Datasets can be obtained as follows:

- **GW simulated events**: Generate with scripts under `dataset/KN_sim/` together with SNANA/OpSim.
- **Training HDF5**: Build from simulated data using `Model/script/create_dataset_bns_nsbh.py` and `optical_only/create_optical_only_datasets.py`.
- For prebuilt datasets, please contact the author.

## Citation

If you use code from this repository, please cite our paper:

```
[Paper in preparation]
```

See `CITATION.cff` for details.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
