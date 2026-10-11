# 📊 Sales Dashboard (Power BI)

An interactive, single-page **Power BI** dashboard that analyses the 2018 sales performance of a multi-category retail business across India. It answers questions such as: *How much did we sell? How much profit did we make? Which products, cities and payment modes drive the business? And in which months did we make a loss?*

---

## 📁 Project Files

| File | Description |
|------|-------------|
| `First_SALES_DASHBOARD.pbix` | The Power BI report (data model + 1 dashboard page) |
| `Orders.csv` | Order-level data: who ordered, when, and from where |
| `Details.csv` | Line-item data: what was sold, amount, profit, quantity, payment mode |

---

## 🗂️ Dataset Overview

### `Orders.csv` — 500 rows (one row per order)

| Column | Description |
|--------|-------------|
| `Order ID` | Unique order identifier (e.g. `B-26055`) — **primary key** |
| `Order Date` | Date of order, format `DD-MM-YYYY` (1 Jan 2018 – 31 Dec 2018) |
| `CustomerName` | Name of the customer (336 unique customers) |
| `State` | State of delivery (19 states) |
| `City` | City of delivery (25 cities) |

### `Details.csv` — 1,500 rows (one row per product line in an order)

| Column | Description |
|--------|-------------|
| `Order ID` | Links to `Orders.csv` — **foreign key** |
| `Amount` | Sales value of the line item (₹) |
| `Profit` | Profit (or loss, if negative) of the line item (₹) |
| `Quantity` | Units sold |
| `Category` | Product category: **Electronics, Clothing, Furniture** |
| `Sub-Category` | 17 sub-categories (e.g. Printers, Saree, Bookcases, Phones) |
| `PaymentMode` | **COD, Credit Card, Debit Card, EMI, UPI** |

### How the tables are connected

```
Orders (1)  ──── Order ID ────  (many) Details
```

One order can contain many line items (average ≈ 3 items per order, max 12). This **one-to-many relationship** on `Order ID` lets the dashboard combine *where/when/who* (Orders) with *what/how much* (Details).

> Data quality: no missing values and no duplicate rows were found in either file. Every `Order ID` in Details has a matching order in Orders.

---

## 🖥️ Dashboard Layout

The dashboard is a single page (1280 × 720) with a green theme. It has **11 visuals**:

```
┌───────────────┬──────────────────────────┬───────┬───────┬───────┐
│ Title         │ Quarter Slicer           │  Qty  │Profit │Amount │
│ "Sales        │                          │ card  │ card  │ card  │
│  Dashboard"   │                          │       │       │       │
├───────────────┼──────────────────────────┴───────┴───────┴───────┤
│ Category      │ Payment Mode   │ Top 6 Sub-Categories by Profit   │
│ Slicer        │ (Donut)        │ (Bar chart)                      │
├───────────────┼────────────────┼──────────────────────────────────┤
│ Category      │ Sales by City  │ Monthly Profit                   │
│ (Donut)       │ (Combo chart)  │ (Line chart)                     │
└───────────────┴────────────────┴──────────────────────────────────┘
```

### Visual-by-visual explanation

| # | Visual | Type | Fields used | What it tells us |
|---|--------|------|-------------|------------------|
| 1 | **Sales Dashboard** | Text box | — | Dashboard title |
| 2 | **Quarter slicer** | Slicer (tiles) | `Orders[Order Date]` → Quarter | Filter the whole page by Qtr 1 – Qtr 4 |
| 3 | **Sum of Quantity** | KPI card | `Details[Quantity]` | Total units sold |
| 4 | **Sum of Profit** | KPI card | `Details[Profit]` | Total profit |
| 5 | **Sum of Amount** | KPI card | `Details[Amount]` | Total sales value |
| 6 | **Category slicer** | Advanced slicer | `Details[Category]` | Filter by Electronics / Clothing / Furniture |
| 7 | **Payment mode donut** | Donut chart | `PaymentMode` by `Quantity` | Share of units sold per payment mode |
| 8 | **Category donut** | Donut chart | `Category` by `Quantity` and `Amount` | Share of each category |
| 9 | **Top 6 Sub-Categories** | Bar chart | `Sub-Category` by `Profit` (Top N = 6) | Most profitable sub-categories |
| 10 | **Sales by City** | Line + stacked column combo | `Orders[City]` by `Amount` (Top N filter on customers) | Which cities generate the most sales |
| 11 | **Monthly Profit** | Line chart with markers & data labels | `Order Date` → Month by `Profit` | Month-wise profit trend |

### Interactivity
All visuals are cross-filtered. Clicking a slice of a donut, a bar, or a slicer button filters every other visual on the page (`drillFilterOtherVisuals` is on).

### Design choices
- Green colour theme (background `#286E38`, cards `#049225`, accents `#046A07`) with yellow `#FBFF00` highlights
- Semi-transparent cards with rounded corners (13 px radius) and soft drop shadows
- Large 35 pt numbers on KPI cards for quick reading

---

## 📈 Key Findings (from the data)

**Overall (full year 2018)**

| Metric | Value |
|--------|-------|
| Total Sales (Amount) | **₹4,37,771** |
| Total Profit | **₹36,963** |
| Total Quantity Sold | **5,615 units** |
| Total Orders | **500** |
| Unique Customers | **336** |
| Profit Margin | **≈ 8.4 %** |
| Average Order Value | **≈ ₹876** |

**By category**

| Category | Sales | Profit | Margin |
|----------|-------|--------|--------|
| Electronics | ₹1,66,267 | ₹13,162 | 7.9 % |
| Clothing | ₹1,44,323 | ₹13,325 | 9.2 % |
| Furniture | ₹1,27,181 | ₹10,476 | 8.2 % |

- Electronics earns the **highest sales**, but **Clothing earns the highest profit** and has the best margin. Clothing is also the volume leader (3,516 units).

**Top 6 sub-categories by profit:** Printers (₹8,606) → Bookcases (₹6,516) → Saree (₹4,057) → Accessories (₹3,353) → Tables (₹3,139) → Trousers (₹2,847).

**Loss-making sub-categories:** Electronic Games (−₹644), Furnishings (−₹806), Kurti (−₹401), Leggings (−₹130), Skirt (−₹315).

**Payment mode**
- **COD** is the most used mode (2,456 units, ~44 %), followed by UPI (1,157 units).
- **Credit Card** gives the best profit (₹12,612) from far fewer units (672), while UPI has high volume but low profit (₹3,286).

**Geography**
- Top states by sales: **Maharashtra** (₹1.02 L), **Madhya Pradesh** (₹87 K), **Uttar Pradesh** (₹38 K).
- Top cities by sales: **Indore** (₹63.7 K), **Mumbai** (₹58.9 K), **Pune** (₹43.6 K).
- Mumbai has high sales but very low profit (₹803), whereas Pune earns ₹6,160 on lower sales.
- Rajasthan and Andhra Pradesh are overall **loss-making** states.

**Time trend**
- **Q1 is the strongest quarter** (₹1.61 L sales, ₹25.9 K profit). Q2 profit drops sharply (₹882) and **Q3 is negative (−₹1,469)**; Q4 recovers (₹11.6 K).
- Months with a net loss: **May, July, September, December**. Best profit month: **November** (₹10,253).
- 529 of the 1,500 line items (≈ 35 %) were sold at a loss — the main reason the margin is only ~8 %.

---

## 🛠️ Tools & Skills Used

- **Microsoft Power BI Desktop** — data modelling and report design
- **Power Query** — loading and typing the CSV data
- **Data modelling** — one-to-many relationship between Orders and Details
- **Auto date hierarchy** — Year / Quarter / Month / Day from `Order Date`
- **Visual features** — slicers, Top N filters, cross-filtering, conditional colours, KPI cards

---

## ▶️ How to Open the Project

1. Install [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows).
2. Download all three files into the same folder.
3. Open `First_SALES_DASHBOARD.pbix`.
4. If Power BI asks for the data source path, go to **Home → Transform data → Data source settings** and point `Orders` and `Details` to your local `Orders.csv` and `Details.csv`.
5. Click **Refresh** to reload the data.

---

## ⚠️ Notes & Possible Improvements

- The saved report has some filters applied: the Quarter slicer is set to **Qtr 1** and the Category slicer excludes **Clothing**. Click *Select all* / clear the slicers to see full-year numbers.
- The "Sales by City" chart has a **Top 6 customers** filter applied on it, so it does not show all cities. Remove this filter to see every city.
- The city *Chandigarh* appears under both **Punjab** and **Haryana** in `Orders.csv` — worth cleaning or confirming.
- The city *Kashmir* is used as a city name (state: Jammu and Kashmir).
- Ideas for the next version:
  - Add **Profit Margin %** as a DAX measure (`DIVIDE(SUM(Profit), SUM(Amount))`)
  - Add a **map visual** for state-level sales
  - Add **Year-over-Year** comparison when more years of data are available
  - Add a page focused on **loss-making products** and discount/pricing analysis
  - Add titles/axis labels to every visual (some currently show default names like "Sum of Quantity")

---

## 🗣️ Explain Karne Ke Liye Tips (Hinglish — sirf aapke liye)

*(GitHub par upload karte time ye section hata sakte ho.)*

**30-second pitch:**
> "Ye ek Sales Dashboard hai jo maine Power BI me banaya hai. Isme 2018 ka data hai — 500 orders aur 1,500 products ki lines. Dashboard me total sales, profit aur quantity dikhti hai, aur ye bhi dikhta hai ki kaunsi category, city, payment mode aur month me business kaisa chala. Quarter aur Category slicer se pura dashboard filter ho jata hai."

**Kaise banaya (step-by-step):**
1. Do CSV files li — `Orders` (kisne, kab, kahan se order kiya) aur `Details` (kya bika, kitna amount, profit).
2. Power BI me load kiya aur `Order ID` ke basis par dono tables ko **one-to-many relationship** se jodha.
3. 3 KPI cards banaye (Amount, Profit, Quantity).
4. Charts banaye — donut (category, payment mode), bar (top 6 sub-category), combo (city-wise sales), line (monthly profit).
5. Slicers lagaye (Quarter, Category) taki user khud filter kar sake.
6. Green theme aur rounded cards se design ko clean rakha.

**Interviewer ke possible sawal:**

| Sawal | Jawab |
|-------|-------|
| Do tables kyun use kiye? | Data normalized hai — order ki info ek baar Orders me, aur uske products Details me. Isse duplication nahi hota. |
| Relationship kaunsi hai? | One-to-many, `Orders[Order ID]` → `Details[Order ID]`. |
| Sabse zyada profit kahan se aaya? | Category me Clothing (₹13.3 K), sub-category me Printers (₹8.6 K), city me Indore (₹6.8 K). |
| Business ki kya problem dikhi? | 35% line items loss me bechi gayi, Q2–Q3 me profit bahut gira, aur COD/UPI me margin kam hai. |
| Profit margin kitna hai? | Lagbhag 8.4% (₹36,963 / ₹4,37,771). |
| Aage kya improve karoge? | Margin % ka DAX measure, map visual, loss-making products ka alag page. |

**Yaad rakhne wali 5 numbers:** ₹4.38 L sales • ₹36.9 K profit • 5,615 units • 500 orders • 8.4% margin

---

## 👤 Author

**Your Name** — *Add your name, LinkedIn and GitHub links here.*
