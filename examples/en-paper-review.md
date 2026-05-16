# English Example: Reviewer-Mode Method Critique

## Input

```text
Use research-mentor in reviewer mode.
Please critique this method paragraph:

We propose a Transformer-based model for building energy consumption forecasting. We compare it with LSTM and ARIMA on one campus dataset. The proposed model achieves the lowest RMSE, showing that attention mechanisms are effective for energy systems forecasting.
```

## Expected reviewer response

### Major concerns

1. **The claim is too broad for the evidence.** One campus dataset and two baselines do not justify the statement that attention mechanisms are effective for energy systems forecasting in general.
2. **The baseline set is weak.** LSTM and ARIMA are useful but insufficient. You need stronger modern baselines such as XGBoost/LightGBM, Temporal Fusion Transformer, N-BEATS/N-HiTS, or recent domain-specific load forecasting models.
3. **The experiment design is underspecified.** The paragraph omits forecasting horizon, train/validation/test split, seasonality handling, weather variables, hyperparameter budget, and repeated runs.
4. **The mechanism is untested.** Better RMSE does not prove attention is the reason. Add ablations that remove attention, change context length, and compare against parameter-matched alternatives.

### Required fixes

- Narrow the claim to the tested dataset and horizon.
- Add a fair baseline table and justify why each baseline matters.
- Report MAE/RMSE/MAPE where appropriate, plus confidence intervals or repeated seeds.
- Add ablation and error analysis for peak demand, weekends, holidays, and extreme weather.

### Verdict

High rejection risk unless the experimental design is strengthened and the claims are narrowed.
