# Battery State-of-Health & Degradation Models (NASA PCoE data)

An open, reproducible notebook comparing a Decision Tree, KNN and XGBoost on two tasks, using four cells (B0005, B0006, B0007, B0018) from the NASA Prognostics Center of Excellence Li-ion battery data set (B. Saha and K. Goebel, 2007).

- **Task 1: SoH estimation** from the shape of one discharge curve (SoH = capacity / 2.0 Ah).
- **Task 2: degradation forecasting**, SoH 10 cycles ahead from the last 5 SoH values.

## Evaluation
Leave-one-battery-out: train on three cells, test on the fourth. No cycle from the test cell is used for training. `duration_s` is excluded from the features because it is almost proportional to capacity under constant-current discharge (label leakage). No hyper-parameter search was done on test cells.

## Results (mean over the 4 held-out cells, in SoH percentage points)

| Task 1: SoH estimation | Mean MAE | Mean RMSE |
|---|---|---|
| XGBoost | 2.01 | 2.58 |
| Decision Tree | 2.46 | 3.18 |
| Linear, cycle number only | 4.07 | 4.48 |
| KNN (k=5) | 5.01 | 5.72 |
| Mean baseline | 8.59 | 9.81 |

| Task 2: forecast SoH 10 cycles ahead | Mean MAE |
|---|---|
| Decision Tree | 1.37 |
| XGBoost | 1.37 |
| Persistence (no change) | 2.22 |
| Linear extrapolation | 3.18 |

## Limitations (please read)
- Only 4 cells, one lab, constant-current discharge, room temperature. With 4 cells the averages have high variance.
- SoH comes from full-discharge capacity. Real EV packs rarely get full discharges, so this is easier than in-vehicle estimation.
- Not validated on pack or fleet data. Not suitable for a safety-critical cutoff on its own.
- Next: add more NASA cells and CALCE data, test partial-discharge inputs, compare with an equivalent-circuit estimator.

## Run it
```
pip install -r requirements.txt
jupyter notebook Battery_SoH_Degradation_NASA.ipynb
```
The notebook downloads the public `.mat` files into `data/` (not stored in this repo). Credit: NASA Ames Prognostics Center of Excellence, Battery Data Set, B. Saha and K. Goebel.
