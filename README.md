# :eyeglasses:  GLAS-BEV: A Geometry-Aware Lifting and Structural Refinement Framework for Camera-Only BEV Perception

[![Python 3.8+](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.0%2B-orange)](https://pytorch.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green)](LICENSE)

> **📢 TODO: The full codebase will be open-sourced upon paper acceptance. This repository currently serves as a preview of the project structure, model zoo results, and usage documentation.**

## To-do List

- [ ] Release full code
- [x] Preview


Official implementation of **"A Geometry-Aware Lifting and Structural Refinement Framework for Camera-Only BEV Perception"**.

We propose two complementary modules on top of the Lift-Splat-Shoot (LSS) framework:

- **PAND** (Perspective-Aware Non-uniform Depth Discretization): replaces metric-uniform depth bins with log-domain (SID) spacing, motivated by the ∂u/∂d ∝ 1/d² perspective geometry. With only K=41 bins it matches or exceeds the performance of K=61 uniform bins.
- **FEC-BEV** (Frequency-Edge Cooperative BEV Refinement): a lightweight three-branch BEV feature enhancement module (low-frequency, high-frequency edge, coordinate-gate) that improves segmentation boundary sharpness with negligible overhead.

Together, the full model achieves **37.8% vehicle IoU** (224 x 480 resolution) and **39.5% vehicle IoU** (480 × 800 resolution) on nuScenes BEV segmentation, while requiring only **0.30 GiB** GPU memory at inference — **4.2× less** than contemporary camera-BEV baselines.

---

## :hammer: Build Environments

**Requirements:** Python ≥ 3.8 · PyTorch ≥ 2.0 · CUDA ≥ 11.7

### Clone the repo

```bash
git clone https://github.com/tyjcbzd/GLAS-BEV.git
cd GLAS-BEV
```

### Create a conda environment

```bash
conda create -n glas_bev python=3.10 -y
conda activate glas_bev
```

### Install dependencies

```bash
pip install -r requirements.txt
```

## :open_hands: Data Preparation

### Download nuScenes

Register and download **nuScenes v1.0** from [nuscenes.org](https://www.nuscenes.org/download):
- **Trainval** split(blobs only)
- **Map expansion**

### Organize the directory

Extract everything into a single `data_nuscenes/` folder (set as `data.dataroot` in configs):

```
data_root/
├── maps/
│   ├── basemap/
│   └── expansion/
├── samples/
│   ├── CAM_BACK/
│   ├── CAM_BACK_LEFT/
│   ├── CAM_BACK_RIGHT/
│   ├── CAM_FRONT/
│   ├── CAM_FRONT_LEFT/
│   └── CAM_FRONT_RIGHT/
├── sweeps/
│   └── ...
├── v1.0-trainval/
│   ├── calibrated_sensor.json
│   ├── ego_pose.json
│   ├── sample.json
│   ├── sample_data.json
│   ├── scene.json
│   └── ...
└── v1.0-mini/          # optional, for quick sanity checks
```

---

## :paperclip: Model Zoo

> **📢 TODO: Checkpoint links will be added upon paper acceptance and code release.**

All models are evaluated on the nuScenes BEV vehicle segmentation benchmark.
- **no-vis**: all validation scenes (no visibility filtering)
- **vis-filt**: only visible (non-occluded) vehicle annotations counted

### Full model

| Config | Backbone | Depth | K | FEC-BEV | Input | IoU↑ (no-vis) | IoU↑ (vis-filt) | FPS | Mem (GiB) | Ckpt |
|---|---|---|---|---|---|---|---|---|---|---|
| `EN-b4_pand_4-45-41_fec_bev.yaml` | EfficientNet-B4 | PAND | 41 | ✓ | 224×480 | 37.8 | 42.23 | — | — | [TODO] |
| `EN-b4_pand_4-45-41_fec_bev_448x800.yaml` | EfficientNet-B4 | PAND | 41 | ✓ | 448×800 | 39.5 | 42.23 | — | — | [TODO] |

### Module ablation (EfficientNet-B4, 224x480)

| Config | PAND | FEC-BEV | IoU↑ (no-vis) | IoU↑ (vis-filt) | Ckpt |
|---|---|---|---|---|---|
| `EN-b4_pand_4-45-41.yaml` | ✓ | ✗ | 38.20 | — | [TODO] |
| EN-b4_uniform_4-45-41_Fec_BEV.yaml` | ✗ | ✓ | 38.30 | — | [TODO] |
| EN-b4_pand_4-45-41_fec_bev.yaml` | ✓ | ✓ | 38.40 | — | [TODO] |

---

## :rocket: Train from Scratch

All experiments use `train_pl.py` with a YAML config:

```bash
python train_pl.py --config <path/to/config.yaml>
```
---

## :white_check_mark: Test

```bash
python evaluate_pl.py --config configs/ablation/EN-b4_pand_4-45-41_fec_bev.yaml
```

The checkpoint path is read from `eval.ckpt_path` in the config, or can be overridden:

```bash
python evaluate_pl.py \
    --config xxxx.yaml \
    --ckpt_path xxxx.ckpt
```

---

## :framed_picture: Visualize



## :pray: Acknowledgements

This project builds on the following outstanding works:

- **[Lift, Splat, Shoot](https://github.com/nv-tlabs/lift-splat-shoot)** — Jonah Philion & Sanja Fidler, ECCV 2020. Core view transformer and QuickCumsum voxel pooling.
- **[FIERY](https://github.com/wayveai/fiery)** — Anthony Hu et al., ICCV 2021. Static BEV segmentation baseline and evaluation protocol.
- 

We also thank the [nuScenes](https://www.nuscenes.org/) team for the dataset and annotation tools.

---

## :mortar_board: Citation

If you find this work useful, please cite:

```bibtex
@article{tang2025glasbev,
  title   = {A Geometry-Aware Lifting and Structural Refinement Framework for Camera-Only BEV Perception},
  author  = {Yi Tang and others...},
  journal = {IEEE Transactions on ...},
  year    = {2025},
}
```

*(BibTeX will be updated upon publication.)*
