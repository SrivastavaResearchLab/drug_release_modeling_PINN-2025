# Drug Release Modeling using Physics-Informed Neural Networks

> **Daanish Aleem Qureshi, Khemraj Shukla, Vikas Srivastava**
> Division of Applied Mathematics & School of Engineering, Brown University

This repository contains the code and data for the paper *"Drug Release Modeling using Physics-Informed Neural Networks"*, which proposes a hybrid framework combining Fick's diffusion law with neural networks to predict controlled drug release from graphene oxide nanosheet films.

## Key Results

- **PINN reduces mean error by up to 40%** relative to classical baselines (Higuchi, Peppas, Fick) across all film types.
- **RMSE < 0.05 with only 9 data points** (2 hours of release data) for the planar film, compared to 12--13 points (1--1.5 days) for classical models.
- For 1D wrinkled and 2D crumpled films, the PINN reaches RMSE < 0.05 at n = 11 (12 hours), saving 12--36 hours of experimental time.
- **Bayesian PINNs (BPINNs)** provide tighter and more reliable uncertainty quantification under noisy conditions via Monte Carlo Dropout.

## Repository Structure

```
drug_release_modeling_PINN/
├── drug_release_pinn.ipynb   # Main notebook (reproduces all paper figures)
├── Data/
│   └── data.csv              # Experimental release data (Liu et al., 2021)
├── figures/                  # Generated figures (created by the notebook)
├── requirements.txt          # Python dependencies
└── README.md
```

## Quick Start

```bash
# Clone the repository
git clone https://github.com/<username>/drug_release_modeling_PINN.git
cd drug_release_modeling_PINN

# Install dependencies
pip install -r requirements.txt

# Run the notebook
jupyter notebook drug_release_pinn.ipynb
```

The notebook runs sequentially and produces six figures saved to `figures/`:

| Figure | Description | Output File |
|--------|-------------|-------------|
| Fig. 3 | PINN vs classical model release curves | `fig3_pinn_vs_classical.png` |
| Fig. 4 | MAE/RMSE bar charts (classical vs PINN) | `fig4_error_comparison.png` |
| Fig. 5 | Uncertainty bounds under noise (PINN/BPINN) | `fig5_noise_uncertainty.png` |
| Fig. 6 | Error comparison under noise | `fig6_noise_error_comparison.png` |
| Fig. 7 | Best-case PINN at minimum data points | `fig7_best_case_pinn.png` |
| Fig. 8 | RMSE vs number of training points | `fig8_rmse_vs_datapoints.png` |

**Note:** The ensemble PINN (50 models x 5000 epochs) and BPINN (10000 epochs) sections are computationally intensive. On a standard machine, expect ~1--2 hours total runtime. Sections can be run independently if prior cells have been executed.

## Methods

The approach integrates Fick's second law of diffusion ($u_t = D \, u_{xx}$) into a neural network loss function alongside experimental drug release measurements:

$$\mathcal{L} = \mathcal{L}_{\text{data}} + \mathcal{L}_{\text{physics}}$$

- **Data loss:** MSE between predicted and measured concentrations
- **Physics loss:** MSE of the Fick's law residual evaluated at 10,000 Latin-hypercube-sampled collocation points

The PINN architecture uses 5 hidden layers with 20 neurons each, tanh activation, and Adam optimization (lr = 1e-3). BPINNs add Monte Carlo Dropout (keep probability = 0.975) for uncertainty quantification.

Three film morphologies are studied using data from Liu et al. (2021):
- **Planar:** uniform basal-plane diffusion
- **1D wrinkled:** anisotropic edge-enhanced release
- **2D crumpled:** isotropic edge-enhanced release

## Data

Experimental drug release data is from:

> M. Liu, D. C. C. Fernandes, Z. S. Saleeba, R. H. Hurt, "Controlled Release of Molecular Intercalants from Two-Dimensional Nanosheet Films," *ACS Nano*, 15(12), 20105--20115, 2021. [DOI: 10.1021/acsnano.1c07954](https://doi.org/10.1021/acsnano.1c07954)

The dataset (`Data/data.csv`) contains cumulative Rhodamine B release concentrations (ppm) at 14 time points over 48 hours for three GO nanosheet film types.

## Citation

If you use this code, please cite:

```bibtex
@article{qureshi2026drug,
  title={Drug Release Modeling using Physics-Informed Neural Networks},
  author={Qureshi, Daanish Aleem and Shukla, Khemraj and Srivastava, Vikas},
  journal={Preprint submitted to Elsevier},
  year={2026}
}
```

## License

This project is licensed under the MIT License.
