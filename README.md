# Drug release modeling using physics-informed neural networks

Code and data for

> D. A. Qureshi, K. Shukla, V. Srivastava, "Drug release modeling using Physics-Informed Neural Networks", *Journal of Drug Delivery Science and Technology* 125 (2026) 108654. [doi:10.1016/j.jddst.2026.108654](https://doi.org/10.1016/j.jddst.2026.108654)

Division of Applied Mathematics, School of Engineering, and Institute for Biology, Engineering and Medicine, Brown University. Corresponding author: Vikas Srivastava (Vikas_Srivastava@brown.edu).

The paper fits physics-informed neural networks (PINNs) that embed Fick's second law to drug release measurements from planar, 1D wrinkled and 2D crumpled graphene oxide films, and compares them with the Higuchi, Peppas and Fick release models. The article reports that for the planar film the PINN reaches RMSE below 0.05 from the first 4% of the 48 h release data, and that the 1D and 2D films reach the same threshold at n = 11 points. Under added noise, a Bayesian PINN with Monte Carlo dropout (100 forward passes) is compared with an ensemble of 50 PINNs.

## Contents

| Path | What it is |
|------|------------|
| `drug_release_pinn.ipynb` | The notebook. It writes Figures 3 to 9 to `figures/` |
| `Data/data.csv` | Release measurements from Liu et al. (2021), see `Data/README.md` |
| `figures/published/` | Figures 2 to 9 as they appear in the article |
| `requirements.txt` | The versions the notebook was tested with |
| `CITATION.cff` | Citation metadata |

Figure 1 of the article is reproduced from Liu et al. (2021) with permission of the American Chemical Society and is not included here. The inverse estimate of effective diffusivity (Section 2.6 and Table 2 of the article) is not part of this repository.

## Running it

```bash
git clone https://github.com/SrivastavaResearchLab/drug_release_modeling_PINN-2025.git
cd drug_release_modeling_PINN-2025
python -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook drug_release_pinn.ipynb
```

Start Jupyter from the repository root so the notebook finds `Data/data.csv`. Every random source is seeded, so a rerun on the same versions gives the same numbers.

## Method

The network takes normalized (x, t) and predicts the concentration u. The loss is the mean squared error on the measurements plus the mean squared residual of Fick's law, f = u_t - D u_xx, at Latin hypercube collocation points. Settings, as in Table 1 of the article:

| Setting | PINN | 50-PINN ensemble | BPINN | Limited data |
|---------|------|------------------|-------|--------------|
| Hidden layers x units | 5 x 20 | 5 x 20 | 5 x 20 | 5 x 20 |
| Activation | tanh | tanh | tanh with dropout, keep probability 0.975 | tanh |
| Optimizer | Adam, 1e-3 | Adam, 1e-3 | Adam, 1e-3 | Adam, 1e-3 |
| Epochs | 2,500 | 5,000 | 10,000 | 2,000 |
| Collocation points | 10,000 | 10,000 | 10,000 | 1,000 |
| D | 0.01 | 0.01 | 0.01 | 0.01 |

## Citation

```bibtex
@article{qureshi2026drug,
  title   = {Drug release modeling using Physics-Informed Neural Networks},
  author  = {Qureshi, Daanish Aleem and Shukla, Khemraj and Srivastava, Vikas},
  journal = {Journal of Drug Delivery Science and Technology},
  volume  = {125},
  pages   = {108654},
  year    = {2026},
  doi     = {10.1016/j.jddst.2026.108654}
}
```

## License

The code is released under the MIT License (`LICENSE`). The measurements in `Data/` belong to Liu et al. (2021). The figures in `figures/published/` are from the article, which is open access under CC BY 4.0.
