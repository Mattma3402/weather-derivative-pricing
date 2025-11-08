# Weather Derivatives for Risk Management
**Pricing a Cooling Degree Days (CDD) Option using Real Weather Data**

This project models and prices a temperature-based derivative that helps companies hedge against weather-related risks.  
By analyzing historical temperature data and simulating future scenarios, we estimate the fair price of a **monthly Cooling Degree Days (CDD)** option — a common tool for **energy** and **agricultural** firms to manage temperature exposure.

---

## Project Goal

Weather derivatives are financial contracts whose payoffs depend on weather variables such as temperature, rainfall, or snowfall.  
In this project, we focus on a **July Cooling Degree Days (CDD)** derivative for **Des Moines, Iowa**, and estimate its price using historical and simulated temperature data.

**Practical motivation:**
- **Energy companies** use CDD derivatives to hedge revenue fluctuations from increased electricity demand for cooling.
- **Agricultural producers** use them to offset losses during unusually hot seasons.

---

## Methodology

### 1. Data Source
- **Historical daily temperature data** from the [Meteostat API](https://dev.meteostat.net/).
- Time range: **2000–2024**  
- Location: **Des Moines, IA (USW00014933)**

### 2. Cooling Degree Days (CDD)
\[
\text{CDD}_{day} = \max(T_{\text{mean}} - 65°F, 0)
\]
Monthly CDD = sum of daily CDD values.

### 3. Contract Payoff
\[
\text{Payout} = \text{Notional} \times \max(\text{CDD}_{total} - K, 0)
\]
- Strike \( K = 200 \)  
- Notional = $100 per CDD  

### 4. Statistical Modeling
- **Seasonal model:** \( T_t = a + b \sin(2\pi t / 365) + c \cos(2\pi t / 365) + \epsilon_t \)
- **Residual model:** \( \epsilon_t = \phi \epsilon_{t-1} + \sigma \eta_t \) (AR(1))
- Simulate 5,000 temperature paths for July.
- Compute expected payouts and price via **Monte Carlo simulation**.

---

## Key Findings

- Historical July CDDs (2000–2024) range roughly **150–310 CDD** in Des Moines.
- Simulated mean July CDD ≈ **230**, consistent with historical mean.
- **Estimated option price ≈ $1,250** (for strike 200, notional $100/CDD).
- The distribution of simulated CDD totals closely matches the historical one.

<p align="center">
  <img src="figures/cdd_histogram.png" alt="Histogram of simulated CDDs" width="500"/>
</p>

---


## How to Run

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/weather-derivative.git
   cd weather-derivative
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Run the notebook**
   ```bash
   jupyter notebook weather.ipynb
   ```

4. *(Optional)* Modify parameters in the first cell:
   - `station_id`, `month_to_price`
   - `base_temp`, `strike`, `notional`
   - `n_sim` for number of Monte Carlo runs

---

## Dependencies

- Python ≥ 3.10  
- pandas  
- numpy  
- matplotlib  
- statsmodels  
- meteostat  
- tqdm  

Install them via:
```bash
pip install pandas numpy matplotlib statsmodels meteostat tqdm
```

---

## Insights & Lessons Learned

- Weather derivatives are valuable **risk management tools** for non-financial sectors.
- Even simple models (seasonality + AR(1)) can capture realistic temperature dynamics.
- Monte Carlo simulation provides intuitive **pricing and risk insights**.
- Challenges remain in calibration and determining **market risk premia**, since weather is **non-tradable**.

---

## References

- Meteostat Python Library: [https://dev.meteostat.net/](https://dev.meteostat.net/)
- Hull, J. C. *Options, Futures, and Other Derivatives*, 11th Edition.  
- Cao, M. and Wei, J. (2000). *Pricing the Weather*. *Risk Magazine*.  
- Jewson, S., Brix, A., & Ziehmann, C. (2005). *Weather Derivative Valuation*. Cambridge University Press.

---

**Author:** Tao Ma  
**Program:** Fall 2025 Quant Finance Boot Camp — The Erdős Institute  
**Duration:** ~3-minute presentation + Jupyter notebook demonstration  
**Last updated:** November 2025
