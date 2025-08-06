# 📁 Data Anonymisation Project

**A beginner-friendly data anonymisation project using Python, pandas, and Faker.**

This project simulates a real-world data privacy task based on the [Commonwealth Bank Virtual Experience on Forage](https://www.theforage.com/). The dataset used is entirely fictional and strictly for educational purposes.

---

## 📌 Project Overview

The goal of this project is to anonymise personal data using common data privacy techniques. The dataset contains sensitive information about fictional mobile customers and is cleaned and anonymised using the following methods:

- **Column removal**
- **Data masking**
- **Synthetic data generation**
- **Hashing (tokenisation)**
- **Date perturbation**
- **Binning (bucketing)**

---

## 📂 Files in This Repository

| File                          | Description |
|-------------------------------|-------------|
| `original_dataset.xlsx`       | Original (fictional) dataset |
| `mobile_customers_cleaned.xlsx` | Anonymised version of the dataset |
| `data_anonymisation.ipynb`    | Jupyter Notebook with all transformation steps |
| `README.md`                   | This file |
| `.gitignore`                  | Git ignore rules |
| `LICENSE`                     | License for usage/sharing |

---

## ⚙️ Libraries Used (pip install ___)

```python
pandas
numpy
faker
hashlib
datetime
random

```

## 🔐 Steps in the Anonymisation Process

### ✅ Step 1: Drop Irrelevant Columns
Columns like **Unnamed: 0**, **customer_id**, and **current_location** were removed. These were either non-essential or contained personally identifiable information (PII).

- **Unnamed: 0** is an index column from CSV export.
- **customer_id** and current_location could reveal user identity or location.

*Purpose*: To reduce privacy risk and keep only relevant data for anonymisation.

✨ Step 2: Mask username
A custom masking function replaces characters with * to partially hide the username, preserving only the first and last characters.

👤 Step 3: Replace Names with Fake Data
Generate fake names using the Faker library while retaining the structure of the dataset.

📧 Step 4: Mask Email Addresses
Emails are partially masked to obscure identifying information while keeping the domain.

📆 Step 5: Add Noise to Dates
Random noise (± days) is added to date_registered and birthdate columns to protect temporal patterns.

📊 Step 6: Group Age and Salary
age and salary are grouped into bins using pandas.cut() to reduce precision and increase privacy.

🔒 Step 7: Hash Credit Card Provider and Expiry
Apply SHA-256 hashing (shortened) to credit card provider and expiry dates for tokenisation.

💳 Step 8: Mask Credit Card Info
Randomize credit card numbers and security codes into pseudo values to completely anonymise them.

🏢 Step 9: Hash Employer and Job
Tokenise employer and job titles with hashing to preserve structure but anonymise values.

🏠 Step 10: Generate Fake Addresses
Use Faker to replace both residence and address columns with synthetic values.



