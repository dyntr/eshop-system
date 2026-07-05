<!-- 1) BANNER -->
<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:7C5CFF,100:B69CFF&height=170&section=header&text=eshop-system&fontColor=ffffff&fontSize=44&desc=E-shop%20system%3A%20customers%2C%20orders%2C%20MySQL&descAlignY=62&descSize=16&animation=fadeIn" width="100%"/>
</p>

<!-- 2) TYPING SVG -->
<p align="center">
  <a href="https://github.com/dyntr">
    <img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&size=18&pause=1000&color=7C5CFF&center=true&vCenter=true&width=680&lines=Flask+storefront+%2B+CLI+admin+console%2C+one+MySQL+schema;CRUD+factories%2C+CSV%2FJSON%2FXML+import%2C+sales+reports" alt="typing"/>
  </a>
</p>

<!-- 3) BADGES -->
<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white"/>
  <img src="https://img.shields.io/badge/Flask-000000?style=flat-square&logo=flask&logoColor=white"/>
  <img src="https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white"/>
  <img src="https://img.shields.io/badge/pytest-0A9EDC?style=flat-square&logo=pytest&logoColor=white"/>
  <img src="https://img.shields.io/badge/status-school%20project-6e7681?style=flat-square"/>
</p>

## Overview

A larger "ročníková práce" school project built as two apps sharing one MySQL database: a Flask storefront for customers (browse, cart, checkout, account) and a separate command-line admin console (`src/`) for full CRUD on the same data — customers, products, orders, order items and transactions.

## ✦ Features

- **Flask storefront** (`website/`) — product listing & search, filter by product type, session-based cart, checkout, order history, login/registration, account editing. Templates are rendered with Jinja2 via `render_template` despite the `.php` file extension — a naming leftover from an earlier plan to use PHP, not actual PHP execution.
- **CLI admin console** (`src/`) — an interactive menu-driven tool built around Factory classes (`CustomerFactory`, `ProductFactory`, `OrdersFactory`, `OrderItemFactory`, `TransactionFactory`) that each implement create/read/update/delete for their table.
- **`Sc_Factory.create_order()`** performs a full checkout server-side in one place: inserts the order, inserts each order item, deducts the customer's credit-point balance by the order total, and logs a `Transaction` row.
- **`GenerateReportFactory`** builds a plain-text summary report — total credit points and customer count per city, total sales per product type.
- **`ImportFactory`** bulk-loads customers, products, orders, order items or transactions from CSV, JSON, or XML into any table.
- **Singleton DB connection** (`DbConnection`) reads `config/config.ini` and hands out one shared `mysql-connector-python` connection.
- **Tests** with `pytest` covering the DB connection, auth flows, and views.
- Ready-to-run MySQL dump (`sql/script.sql`) defining the schema: `customer`, `product`, `orders`, `orderitem`, `transaction`.

## 🛠 Built with

Flask, Flask-Bootstrap, Flask-WTF, Flask-Session, `mysql-connector-python`, pytest.

## 🧭 Order flow

```mermaid
flowchart TD
    A[Customer / admin places an order] --> B[Sc_Factory.create_order]
    B --> C[INSERT Orders row]
    C --> D[INSERT OrderItem per product]
    D --> E[Look up Product.Price]
    E --> F[Sum total price]
    F --> G[Deduct CreditPoints from Customer]
    G --> H[INSERT Transaction: -total]
    H --> I[Commit]
```

## 📦 Getting started

Requires Python 3 and a local MySQL server.

```bash
pip install -r config/requirements.txt

# creates the `rocnikovka` schema, tables and seed data
mysql -u root -p < sql/script.sql

# storefront -> http://127.0.0.1:5000 (run from Database_app/, NOT from inside website/)
python3 -m website.main

# admin console (separate terminal, run from Database_app/src)
cd src && python3 main.py
```

Both apps read the same `config/config.ini` for the MySQL connection — no credentials are included here; point it at your own local MySQL instance.

## 🎓 What I learned

Splitting a customer-facing app and an admin tool over one shared database, the Factory pattern for table-level CRUD, Flask blueprints/sessions/auth, and writing tests (pytest) alongside the application instead of after it. Also a concrete lesson in what to fix first on a revisit: this version stores customer passwords in plaintext, whereas the `order-management-gui` project (built later) hashes them with SHA-256 — a good example of the same author's security habits improving between projects.

## 🚀 Status

Built during studies at SPŠE Ječná (larger year project) — a school project, not a real store; noted honestly rather than hidden.

<!-- FOOTER -->
---
<p align="center">
  <sub>Part of <a href="https://github.com/dyntr">Patrick Dyntr's</a> portfolio · Built by <a href="https://github.com/dyntr">@dyntr</a></sub>
</p>
