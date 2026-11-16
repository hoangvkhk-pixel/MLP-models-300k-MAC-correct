# MLP aerodynamic surrogate models, 300k labels

Two MLPs trained on 300,000 AVL labels (25,000 per branch, 12 original12 branches):

- `aero_mlp_normal_macfix_300k/` - normal configurations (a_S_rel <= 0.5), 225,000 labels
- `aero_mlp_duck_macfix_300k/` - duck configurations (a_S_rel > 0.5), 75,000 labels

Each folder: `aero_mlp_v2_best.keras` (the model used), `scaler_X.joblib`, `scaler_Y.joblib`, `metrics.json`, `history.json`, `pred_test.npy`, other checkpoints (`best_combo`, `best_cx`, `best_loss`) and data-correlation tables.

Inputs (20): f_aspect, f_sweep, f_taper, f_twist, a_aspect, a_sweep, a_taper, a_twist, a_x_loc, a_S_rel, v_aspect, v_S_rel, scheme_fuse, scheme_vertical, a_dihedral_mag, S_ref, V, H, alpha, delta

Outputs (6): cx, cy, mz_ref, mx_beta, my_beta, K

Split 80/10/10 (seed 42), 400 epochs, batch 512, cx weight 3, K weight 2.

Test metrics:

| output | R2 normal | R2 duck | MAE normal | MAE duck |
|---|---|---|---|---|
| cx | 0.9982 | 0.9991 | 0.00066637 | 0.00068294 |
| cy | 0.9997 | 0.9997 | 0.009657 | 0.011285 |
| mz_ref | 0.9993 | 0.9994 | 0.027463 | 0.05105 |
| mx_beta | 0.9990 | 0.9991 | 4.0286e-05 | 2.6919e-05 |
| my_beta | 0.9987 | 0.9982 | 6.3471e-05 | 6.297e-05 |
| K | 0.9991 | 0.9992 | 0.32546 | 0.32507 |

The models are used by the optimisation code in the MLP optimisation repository (`MlpBackend` in `src/new20_sizing_eval.py`).
