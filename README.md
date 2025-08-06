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

---

### ✨ Step 2: Mask username
A custom masking function replaces characters with * to partially hide the username, preserving only the first and last characters.

🔍 Explanation of the code:

| Code                          | Description |
|-------------------------------|-------------|
| `len(name)`                   | Checks how many characters are in the username |
| `if len(name) <= 2`           | If the name has 1 or 2 letters, it’s fully masked with * |
| `name[0]`                     | Gets the first character of the username |
| `name[-1]`                    | Gets the last character of the username |
| `'*' * (len(name) - 2)`       | Creates a string of asterisks to replace the middle characters |
| `.apply(mask_name)`           | Applies this function to every row in the username column |

Example: *For a 5-letter name like "susan", it generates "***". *The final result joins the first letter + asterisks + last letter (e.g., "susan" → "s***n").

---

### 👤 Step 3: Replace Names with Fake Data
Generate fake names using the Faker library while retaining the structure of the dataset.

🔍 Explanation of the code:

| Code                                      | Description |
|-------------------------------------------|-------------|
| `Faker()`                                 | A tool that generates fake data like names, addresses, email, etc |
| `Faker.seed(42)`                          | Generates the same set of fake names every time when run the code |
| `range(len(df))`                          | Creates a loop equal to the number of rows in the DataFrame |
| `[fake.name() for _ in range(len(df))]`   | Gets the last character of the username |
| `df['name']`                              | This list is then assigned to the column replacing all original names with fake ones |

---

### 📧 Step 4: Mask Email Addresses
This helps protect personal information while maintaining a valid email format for analysis.

- The **mask_email()** function partially hides the username part of the email while keeping the domain visible (e.g., a****z@gmail.com).
- The email is split into two parts: the name before the **@** and the domain after.
- The name is masked by keeping the first and last characters and replacing the middle characters with asterisks just like Step 2.

---

### 📆 Step 5: Add Noise to Dates
Random noise (± days) is added to date_registered and birthdate columns to protect temporal patterns.

🔍 Explanation of the code:

| Code                                      | Description |
|-------------------------------------------|-------------|
| `pd.to_datetime()`                        | Converts strings to date format |
| `errors='coerce'`                         | Turns invalid dates into NaT |
| `timedelta(days=...)`                     | Lets us shift dates forward or backward |
| `random.randint(-max_days, max_days)`     | Picks a random number of days |
| `add_noise_to_date()`                     | Adds this random shift to each date |
| `.apply(...)`                             | Applies it to every row in the column |
| `.dt.strftime('%-d/%-m/%Y')`              | Formats the date like 1/8/2025 |

---

### 📊 Step 6: Group Age and Salary
Age and salary are grouped into bins to reduce precision and increase privacy.

📘 Explanation of the code:

| Code                                      | Description |
|-------------------------------------------|-------------|
| `pd.cut()`                                | Groups continuous numbers into ranges or “bins” |
| `bins1 = [...]`                           | Defines age ranges (e.g. 20–30, 30–40, etc.) |
| `df['age'] = pd.cut(df['age'], bins1)`    | Replaces each exact age with the range it falls into |

Same goes for salary using **bins2**.

---

### 🔒 Step 7: Hash Credit Card Provider and Expiry
This hides sensitive data (like “Visa” or “12/26”) while keeping a consistent format.

📘 Explanation of the code:

| Code                                      | Description |
|-------------------------------------------|-------------|
| `hashlib.sha256()`                        | Turns text into a fixed, unreadable string (called a hash) |
| `hexdigest()[:10]`                        | Keeps only the first 10 characters of that long hash (to shorten it) |
| `str(x)`                                  | Ensures the data is a string before hashing |
| `apply(lambda x: ...)`                    | Runs the **hash_token()** function on every row in the column |

---

### 💳 Step 8: Mask Credit Card Info
Randomize credit card numbers and security codes into pseudo values to completely anonymise them and removing any real financial data.

📘 Explanation of the code:

| Code                                      | Description |
|-------------------------------------------|-------------|
| `np.random.uniform(a, b)`                 | Creates a random number between a and b |
| `-1e18`                                   | Means -1 followed by 18 zeros (a very large number) |
| `apply()`                                 | Runs the masking function on every value in the column |

This replaces real credit card info with fake, random numbers.

---

### 🏢 Step 9: Hash Employer and Job
Tokenise employer and job titles with hashing to preserve structure but anonymise values. Similar as Step 7.

---

### 🏠 Step 10: Generate Fake Addresses
Use Faker to replace both residence and address columns with synthetic values. Similar as Step 3.

📘 Explanation of the code:

| Code                                      | Description |
|-------------------------------------------|-------------|
| `fake.address()`                          | Gives a full fake address |
| `replace("\n", " ")`                      | Makes sure the address stays on one line |
| `for _ in range(len(df))`                 | Repeats this for every row in the DataFrame |

---

## 💾 Saving the Output
The cleaned and anonymised dataset is saved in a new Excel file.

---

## 🚀 How to Run the Code

1. Clone this repository.
2. Open data_anonymisation.ipynb in Jupyter Notebook or VS Code.
3. Run each cell sequentially.
4. The anonymised dataset will be saved as mobile_customers_cleaned.xlsx.

---

## 📚 Disclaimer
This project is a learning exercise based on a public virtual internship from Forage. All data used is fictional and not linked to real individuals.

---

## 📃 License
This project is licensed under the MIT License. See the LICENSE file for more information.

---

## 🤝 Acknowledgements

- Forage
- Commonwealth Bank Virtual Experience
- Python open-source community 💙



