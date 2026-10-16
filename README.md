
"""
Task-01: Distribution of a categorical/continuous variable
Goal: Create a bar chart and a histogram to visualize
the distribution of ages and genders in a population.
"""

import os
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# ---------- 1. Load data ----------
csv_path = "population.csv"

if os.path.exists(csv_path):
    df = pd.read_csv(csv_path)
else:
    print(f"'{csv_path}' not found — generating a synthetic sample dataset instead.")
    rng = np.random.default_rng(42)
    n = 500

    df = pd.DataFrame({
        "Age": rng.normal(
            loc=35, scale=15, size=n
        ).clip(0, 90).round().astype(int),

        "Gender": rng.choice(
            ["Male", "Female"], size=n, p=[0.52, 0.48]
        )
    })

print(df.head())
print("\nShape:", df.shape)

# ---------- 2. Plot ----------
fig, axes = plt.subplots(1, 2, figsize=(12, 5))

# Histogram: continuous variable (Age)
axes[0].hist(
    df["Age"], bins=15,
    color="#4C72B0", edgecolor="black"
)
axes[0].set_title("Distribution of Age")
axes[0].set_xlabel("Age")
axes[0].set_ylabel("Frequency")

# Bar chart: categorical variable (Gender)
gender_counts = df["Gender"].value_counts()

axes[1].bar(
    gender_counts.index,
    gender_counts.values,
    color=["#55A868", "#C44E52"]
)
axes[1].set_title("Distribution of Gender")
axes[1].set_xlabel("Gender")
axes[1].set_ylabel("Count")

plt.tight_layout()

# ---------- 3. Save output ----------
out_path = "task1_distribution.png"
plt.savefig(out_path, dpi=150)
print(f"\nSaved chart to {out_path}")

plt.show()
