# 🛒 ZipMart — Orders & Products ETL Pipeline

A Python-based **Extract → Transform → Load (ETL)** pipeline for ZipMart's e-commerce order data. The project cleans and standardizes raw orders and product data, merges them into a unified report, and answers six key business questions through data analysis and visualizations.

---

## 📁 Project Structure

```
zipmart/
│
├── zipmart.ipynb            # Main notebook (ETL + Analysis)
├── orders.csv               # Raw orders dataset (input)
├── products.json            # Raw products dataset (input)
├── Cleaned_orders.csv       # Cleaned orders (output)
├── Cleaned_products.csv     # Cleaned products (output)
├── orders_report.csv        # Final merged report (output)
│
├── requirements.txt
└── README.md
```

---

## 🔄 Pipeline Overview

```
orders.csv          products.json
     │                    │
     ▼                    ▼
  Extract              Extract
     │                    │
     ▼                    ▼
  Transform            Transform
  (Clean &             (Clean &
  Standardize)         Standardize)
     │                    │
     ▼                    ▼
Cleaned_orders.csv   Cleaned_products.csv
          │               │
          └──── MERGE ────┘
                  │
                  ▼
          orders_report.csv
                  │
                  ▼
          Business Analysis
          (6 Queries + Charts)
```

---

## 🗂️ Datasets

| File             | Format | Description                                              |
|------------------|--------|----------------------------------------------------------|
| `orders.csv`     | CSV    | Raw order records (order_id, product_id, quantity, city, payment_mode, order_date, order_status) |
| `products.json`  | JSON   | Product master data (product_id, product_name, category, unit_price) |

---

## 🧹 Transformations Applied

### Orders (`orders.csv`)

| Column          | Transformation                                                                 |
|-----------------|--------------------------------------------------------------------------------|
| `order_id`      | Lowercase, strip whitespace; fill nulls with mode                              |
| `product_id`    | Lowercase, strip whitespace, remove hyphens                                    |
| `quantity`      | Coerce to numeric, clip negatives to 0, replace 9999 sentinel with 0          |
| `order_date`    | Parse to `datetime` (mixed format support)                                     |
| `customer_city` | Lowercase, normalize aliases (Bengaluru→Bangalore, Madras→Chennai, etc.)      |
| `payment_mode`  | Normalize aliases (COD, cc, dc → standard snake_case format)                  |
| `order_status`  | Lowercase                                                                      |

**Payment mode normalization:**

| Raw Value               | Standardized         |
|-------------------------|----------------------|
| cod, cash on delivery   | `cash_on_delivery`   |
| cc, credit card         | `credit_card`        |
| dc, debit card          | `debit_card`         |
| netbanking, net banking | `net_banking`        |
| u.p.i                   | `upi`                |

### Products (`products.json`)

- `product_id`, `product_name`, `category` — lowercased and stripped.
- Duplicate rows removed.

---

## 📊 Business Analysis — 6 Queries

| # | Question                                                              | Visualization         |
|---|-----------------------------------------------------------------------|-----------------------|
| Q1 | What is the total revenue generated across all orders?               | Line chart (by month) |
| Q2 | Which product category contributed the highest total revenue?         | Bar chart (top 5)     |
| Q3 | Which product was ordered the most by total quantity sold?            | Bar chart (top 5)     |
| Q4 | Which city placed the highest number of orders?                       | Bar chart (top 5)     |
| Q5 | How many orders had zero quantity after null-filling, and what % is that? | Bar chart          |
| Q6 | What is the average order value (average total revenue per order)?    | Printed output        |

> Revenue is calculated as `quantity × unit_price` and only counted for **delivered** orders.

---

## ⚙️ Setup & Usage

### 1. Clone the repository

```bash
git clone https://github.com/your-username/zipmart.git
cd zipmart
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Add input files

Place `orders.csv` and `products.json` in the project root directory.

> Update the file paths in the notebook if needed — currently set to absolute local paths.

### 4. Run the notebook

```bash
jupyter notebook zipmart.ipynb
```

Run all cells top to bottom. Cleaned CSVs and the merged report will be saved in the same directory.

---

## 📦 Output Files

| File                  | Description                                     |
|-----------------------|-------------------------------------------------|
| `Cleaned_orders.csv`  | Standardized and null-handled orders data       |
| `Cleaned_products.csv`| Cleaned product master data                     |
| `orders_report.csv`   | Merged dataset with `total_revenue` column      |

---

## 🛠️ Tech Stack

| Tool           | Purpose                        |
|----------------|--------------------------------|
| Python 3.x     | Core language                  |
| pandas         | Data loading, cleaning, merging|
| matplotlib     | Visualizations                 |
| seaborn        | Extended chart styling         |
| Jupyter        | Interactive notebook environment|

---

## 📄 License

This project is for educational and portfolio purposes.
