import numpy as np
import matplotlib.pyplot as plt
import math

# ==========================================
# 1. SET THE OPTION AND MARKET PARAMETERS
# ==========================================

S0 = 100 # Current stock price (£)
K = 100 # Strike price (£)
T = 1.0 # Time to expiry (years)
r = 0.05 # Risk-free interest rate (5%)
sigma = 0.20 # Annual volatility (20%)

N = 252 # Trading-time steps per year
M = 100_000 # Number of simulated paths

# Make results reproducible
np.random.seed(42)


# ==========================================
# 2. DISCRETISE TIME
# ==========================================

dt = T / N
sqrt_dt = math.sqrt(dt)

print("Time step:", dt)


# ==========================================
# 3. SIMULATE STOCK PRICE PATHS
# ==========================================

# Start all simulations at today's stock price
S = np.full(M, S0, dtype=float)

# Keep 10 paths so we can plot them later
sample_paths = np.zeros((10, N + 1))
sample_paths[:, 0] = S0

for t in range(1, N + 1):

    # Generate one independent standard normal
    # random variable for each simulation
    Z = np.random.standard_normal(M)

    # Exact GBM update under risk-neutral pricing
    S = S * np.exp(
        (r - 0.5 * sigma**2) * dt
        + sigma * sqrt_dt * Z
    )

    # Save the first 10 paths for the chart
    sample_paths[:, t] = S[:10]


# ==========================================
# 4. CALCULATE THE EUROPEAN CALL PAYOFF
# ==========================================

# Payoff = max(final stock price - strike price, 0)
payoffs = np.maximum(S - K, 0)


# ==========================================
# 5. DISCOUNT AND AVERAGE THE PAYOFFS
# ==========================================

discount_factor = math.exp(-r * T)

option_price = discount_factor * np.mean(payoffs)

# Estimate the simulation's statistical uncertainty
standard_error = (
    discount_factor * np.std(payoffs, ddof=1)
    / math.sqrt(M)
)


# ==========================================
# 6. CALCULATE BLACK-SCHOLES FOR COMPARISON
# ==========================================

d1 = (
    math.log(S0 / K)
    + (r + 0.5 * sigma**2) * T
) / (sigma * math.sqrt(T))

d2 = d1 - sigma * math.sqrt(T)

def normal_cdf(x):
    return 0.5 * (1 + math.erf(x / math.sqrt(2)))

black_scholes_price = (
    S0 * normal_cdf(d1)
    - K * math.exp(-r * T) * normal_cdf(d2)
)


# ==========================================
# 7. DISPLAY THE RESULTS
# ==========================================

print("\n--- OPTION PRICING RESULTS ---")
print(f"Monte Carlo price: £{option_price:.4f}")
print(f"Black-Scholes price: £{black_scholes_price:.4f}")
print(f"Absolute difference: £{abs(option_price - black_scholes_price):.4f}")
print(f"Estimated standard error: £{standard_error:.4f}")
print(
    f"Approx. 95% interval: "
    f"£{option_price - 1.96 * standard_error:.4f} "
    f"to £{option_price + 1.96 * standard_error:.4f}"
)


# ==========================================
# 8. PLOT THE SIMULATED STOCK PATHS
# ==========================================

days = np.linspace(0, T, N + 1)

for path in sample_paths:
    plt.plot(days, path, alpha=0.8)

plt.axhline(K, linestyle="--", label="Strike price")
plt.xlabel("Time (years)")
plt.ylabel("Stock price (£)")
plt.title("Monte Carlo: Simulated Stock Price Paths")
plt.legend()
plt.grid(True)
plt.show()
