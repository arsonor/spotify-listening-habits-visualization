# Spotify Charts Analysis - Emerging Markets Study

## Overview

This project aims to analyze Spotify streaming data, focusing on emerging markets and their music consumption patterns. Using data from various regions over the last three years, the goal is to provide insights into the evolving music trends in these countries and visualize the findings using Power BI.

<p align="center">
  <img src="images/Dashboard Overall Streams.png">
</p>
<p align="center">
  <img src="images/Dashboard Track Charts performance.png">
</p>
<p align="center">
  <img src="images/Dashboard Audio Distribution.png">
</p>

## Project Structure

The project follows the process from data acquisition to modeling and visualization, as described below:

### 1. Data Scraping
- **Source**: The primary data comes from Spotify's daily and weekly top 200 charts, categorized by country and globally.
- **Script**: A Python script was used to scrape the Spotify charts data.
- **API Integration**: Additional track and artist information were retrieved using the Spotify API, requiring valid API credentials and access tokens.

### 2. Data Handling
- **Collected Files**: The raw data is stored in CSV files:
  - `charts.csv`: Contains streaming chart data, including rank, peak rank, weeks on chart, and trends such as 'moved up,' 'new entry,' and 're-entry.'
  - `tracks.csv`: Holds track metadata such as track name, release date, and more.
  - `artists.csv` & `genres.csv`: Include artist and genre information for further enrichment.
  
- **Data Cleaning**: The scraped data underwent a cleaning process to remove duplicates, rename columns, and ensure uniformity across different files.

### 3. Data Modeling
- **Fact and Dimension Tables**: The project follows a star schema approach:
  - **Fact Table (FACT_CHART)**: Consolidates all `charts.csv` data across 15 countries and global charts for three years. Each row represents a track’s rank for a specific country and week.
  - **Dimension Tables**:
    - `DIM_Track`: Consolidates track data from `tracks.csv` and links with the fact table to retrieve track names and release dates.
    - `DIM_Artist` and `DIM_Genre`: Created by merging artist and genre information with the fact table.
    - `DIM_Region`: Adds regional identifiers to the fact table for easier analysis across different countries.

### 4. Data Visualization
- **Power BI Dashboard**: 
  - Visualizes key metrics such as the number of streams, top-performing artists, and weekly chart movements across different regions.
  - Includes filters by region, time period, artist, and genre to allow in-depth analysis.
  - Highlights trends in music consumption in countries like Nigeria, Indonesia, Brazil, and more.
  
  Key insights include:
  - Growth in streams across emerging markets.
  - Popularity of genres like pop, reggaeton, hip hop, and regional styles.
  - Detailed artist performance, tracking streams and chart positions over time.

### 5. Limitations
- The data focuses on mainstream tracks and does not fully represent the diversity of music within each region.
- User-specific data (such as demographic information or user behavior) was not accessible for this study.
  
### 6. Future Work
- Extending the dataset to cover up to 10 years of streaming data for historical trend analysis.
- Implementing an ETL pipeline using SSIS to automate weekly data updates in Power BI.

## Conclusion
This project presents an insightful analysis of global and regional music trends using Spotify data. The findings help understand the growth and popularity of music genres across various emerging markets, visualized effectively through Power BI.

## Tools & Technologies
- **Data Scraping**: Python
- **Data Storage**: CSV
- **Data Visualization**: Power BI
- **API**: Spotify API

Thank you for exploring this project!


