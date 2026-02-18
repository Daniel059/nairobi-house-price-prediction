# Nairobi House Price Prediction – LT Labs Fellowship Sprint

**Day 1 – Data Collection & Structuring**  
**Date**: February 18, 2026

## Objective (Day 1)

Collect 300–800 Nairobi house listings with the required fields:  
- Location  
- Property Type  
- Bedrooms  
- Bathrooms  
- Size  
- Amenities  
- Price (KES)  
- Listing Date  

Save as `raw_listings.csv`  
Create a data dictionary  
Initialize GitHub repository

## What Was Achieved

- Successfully collected **407 house listings** from https://www.buyrentkenya.com/houses-for-sale/nairobi  
- Scraped using Python (`requests` + `BeautifulSoup`) with polite delays (3–8 seconds between pages)  
- Handled pagination correctly:  
  - Page 1: no query parameter  
  - Pages 2+: `?page=N`  
- Raw dataset saved:  
  `data/raw/raw_listings_buyrentkenya_2026-02-18.csv` (407 rows)

### Key Data Coverage (raw)

- **Price (KES)**: 405 / 407 listings successfully parsed (very strong result)  
- **Bedrooms**: 407 / 407 filled  
- **Bathrooms**: mostly filled (some missing)  
- **Size**: partial (some in m², ft², acres; many empty)  
- **Property Type**: mostly filled (house, townhouse, villa)  
- **Amenities**: filled (short description text)  
- **Listing Date**: empty (not visible on list pages)  
- **Location**: **empty in raw data** — but actually present inside the Amenities column!

### Important Fix (already documented in notebook)

The **Location** field came out blank because the scraper captured location info inside the **Amenities** column instead of a dedicated location tag.

**Quick fix applied in notebook**:
- Dropped the empty Location column  
- Renamed Amenities → Location  
- Dropped constant Scrape_Date column  

→ Result: usable Location column with suburbs like Lavington, Karen, Runda, Westlands, etc. (dozens of unique values)

## Data Dictionary – Raw Dataset

| Column         | Description                                      | Status                  | Notes                              |
|----------------|--------------------------------------------------|-------------------------|------------------------------------|
| Location       | Suburb / area                                    | Empty                   | Data was in Amenities column       |
| Property Type  | house / townhouse / villa                        | Mostly filled           | Parsed from title                  |
| Bedrooms       | Number of bedrooms                               | 407/407                 | String – clean to int later        |
| Bathrooms      | Number of bathrooms                              | Mostly filled           | String – clean to int later        |
| Size           | Plot / built-up area                             | Partial                 | Units vary (m², ft², acres)        |
| Amenities      | Short description / features                     | Filled                  | Contains location + some phones    |
| Price          | Asking price in KES                              | 405/407                 | Good coverage                      |
| Listing Date   | When the listing was posted                      | Empty                   | Not on list pages                  |
| Source_URL     | Page where listing was found                     | Filled                  | For traceability                   |
| Scrape_Date    | When we collected the data                       | Constant (2026-02-18)   | Not useful for modeling            |

## GitHub Deliverables (Day 1)

- Repository initialized  
- Raw CSV pushed: `data/raw/raw_listings_buyrentkenya_2026-02-18.csv`  
- Notebook: `notebooks/01_day1_data_collection.ipynb`  
  - Scraping code with pagination fix  
  - Debug prints  
  - Basic data preview  
  - Cleaning steps (renaming Amenities → Location)  
- README.md updated with Day 1 summary  

Repo is ready to share with the LT Labs team.

## Challenges Faced & How They Were Solved

- First attempt (kenyapropertycentre.com) → 400 rows but **0 prices** → switched source  
- BuyRentKenya page=1 returned 404 → fixed by removing `?page=1` for first page  
- Location field empty → discovered it was inside Amenities → renamed column  
- Minor parsing issues (phones in text, inconsistent units) → basic regex cleaning applied

## Next Steps (Day 2 Preview)

- Finalize cleaning notebook:  
  - Standardize location names  
  - Convert bedrooms/bathrooms to integers  
  - Parse & standardize size (to m²)  
  - Remove remaining phone numbers  
  - Handle missing values & outliers  
- Basic EDA visuals (price distribution, price by location, etc.)  
- Save clean CSV for modeling

Day 1 complete — solid raw dataset with strong price coverage and usable locations after quick fix.  
Ready for cleaning & feature engineering.