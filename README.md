# Scraping Data from Wikipedia with BeautifulSoup + Pandas

This project demonstrates how to scrape structured data from a real website (Wikipedia) using **Python, BeautifulSoup, and Pandas**.  
We specifically extract the **second table** from the Wikipedia page:  
[List of largest companies in the United States by revenue](https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue).

---

## 📌 Features
- Send HTTP requests with `requests` and a custom **User-Agent**.
- Parse HTML content using **BeautifulSoup**.
- Extract only the **second `wikitable sortable`** table.
- Retrieve:
  - Table headers (`<th>`)
  - Table rows (`<tr>`)
  - Individual cell values (`<td>`)
- Clean and strip text data.
- Store extracted data into a **Pandas DataFrame** for easy analysis.

---

## 🛠️ Requirements
Make sure you have the following installed:

```bash
pip install requests beautifulsoup4 pandas
```

---

## 🚀 How It Works

### 1. Import dependencies
```python
from bs4 import BeautifulSoup
import requests
import pandas as pd
```

### 2. Send request with headers
```python
url = "https://en.wikipedia.org/wiki/List_of_largest_companies_in_the_United_States_by_revenue"
headers = {"User-Agent": "Mozilla/5.0"}
page = requests.get(url, headers=headers)
```

### 3. Parse with BeautifulSoup
```python
soup = BeautifulSoup(page.text, "html.parser")
tables = soup.find_all("table", class_="wikitable sortable")
second_table = tables[1]
```

### 4. Extract headers
```python
headers = [th.text.strip() for th in second_table.find_all("th")]
print("Headers:", headers)
```

### 5. Extract rows
```python
rows = second_table.find_all("tr")[1:]  # skip header row
data = []

for row in rows:
    cells = row.find_all("td")
    row_data = [cell.text.strip() for cell in cells]
    data.append(row_data)
```

### 6. Convert to Pandas DataFrame
```python
df = pd.DataFrame(data, columns=headers)
print(df.head())
```

---

## 📊 Output Example
The script will produce a DataFrame like this:

| Rank | Name   | Industry | Revenue (USD millions) | Revenue growth | Employees | Headquarters |
|------|--------|----------|-------------------------|----------------|-----------|--------------|
| 1    | Walmart | Retail | 611,289 | 6.7% | 2,100,000 | Bentonville, Arkansas |
| 2    | Amazon  | Retail | 513,983 | 9.4% | 1,540,000 | Seattle, Washington |

---

## 📂 Files
- `Scraping Data from a Real Website + Pandas.ipynb` → Jupyter notebook with full code
- `README.md` → This documentation

---

## 📌 Notes
- Wikipedia may update table structure, so indexes (`tables[1]`) might need adjustment.
- Always set a **User-Agent** in your requests to avoid 403 errors.
- Use responsibly and respect the website’s robots.txt policy.

---

## ✅ Next Steps
- Save the DataFrame to CSV/Excel:
```python
df.to_csv("Companies.csv", index=False)
```
- Perform further analysis (sorting, filtering, visualization).
