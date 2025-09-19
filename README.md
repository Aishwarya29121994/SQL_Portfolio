STEP 1:

# SQL Portfolio — Aishwarya Bhangarshettra

This repository is a **step-by-step SQL practice portfolio** built with **SQLite + Jupyter Notebooks**.  
It’s written so that even complete beginners can clone, set up, and run it easily.  

---

## 🌟 What this repo shows
- How to set up Git, Python, and Jupyter locally  
- How to organize a portfolio repo for recruiters  
- How to run **normal SQL** queries on SQLite inside Jupyter  
- Clear examples: **Problem → SQL → Result → What this shows**

---

## 🛠️ Step 1. Prerequisites
Make sure you have:
- **Python 3.8+** (`python3 --version`)  
- **Git** (`git --version`)  
- A GitHub account with this repo cloned  

Configure Git (first time on your computer):
```bash
git config --global user.name  "Your Name"
git config --global user.email "YourEmail@example.com"
git config --global credential.helper osxkeychain

STEP 2:

mkdir -p ~/projects
cd ~/projects
git clone https://github.com/Aishwarya29121994/SQL_Portfolio.git
cd SQL_Portfolio

STEP 3:

python3 -m venv .venv
source .venv/bin/activate

pip install --upgrade pip
pip install jupyter ipython-sql sqlalchemy pandas matplotlib

Ignore files we don’t want in Git:

echo ".venv/" >> .gitignore
echo ".ipynb_checkpoints/" >> .gitignore
echo "*.db" >> .gitignore

STEP 4:

We’ll practice with a small orders table.

mkdir -p data notebooks sql

Create the dataset file:

# data/orders.csv
order_id,customer_id,order_date,total_amount
1,101,2024-01-02,59.90
2,102,2024-01-05,120.00
3,101,2024-02-01,39.50
4,103,2024-02-10,200.00
5,104,2024-03-03,77.25


Step 5:

Create your first Jupyter notebook

jupyter notebook

	Open the notebooks/ folder

	Click New → Python 3

	Rename it 01_sql_basics.ipynb

Add these cells inside:

Cell A — Configure SQL magic:

%load_ext sql
%sql sqlite:///portfolio.db
%config SqlMagic.autopandas = True   # avoids prettytable errors

Cell B — Load CSV into a SQLite table

import pandas as pd
from sqlalchemy import create_engine

engine = create_engine("sqlite:///portfolio.db")
df = pd.read_csv("../data/orders.csv")
df.to_sql("orders", con=engine, if_exists="replace", index=False)

Cell C — Run your first SQL query

%%sql
SELECT customer_id,
       COUNT(*) AS n_orders,
       SUM(total_amount) AS revenue
FROM orders
GROUP BY 1
ORDER BY revenue DESC;


Expected result (with sample CSV):

customer_id | n_orders | revenue
------------+----------+---------
103         |    1     | 200.00
102         |    1     | 120.00
104         |    1     |  77.25
101         |    2     |  99.40

Cell D — Save query to .sql file

from pathlib import Path
Path("../sql/01_select_basics.sql").write_text("""
SELECT customer_id,
       COUNT(*) AS n_orders,
       SUM(total_amount) AS revenue
FROM orders
GROUP BY 1
ORDER BY revenue DESC;
""".strip())


Cell E — Markdown explanation:

# SQL Basics — Customer Revenue

**Problem:** Who are our top customers by revenue?  
**SQL:** GROUP BY + SUM + ORDER BY  
**What this shows:** simple aggregation to rank customers by spend.

Step 6. Commit and push

git add .
git commit -m "Add first SQL notebook, dataset, and beginner README"
git push

STEP 7 :
Share clean view links

https://nbviewer.org/github/Aishwarya29121994/SQL_Portfolio/blob/main/notebooks/01_sql_basics.ipynb
