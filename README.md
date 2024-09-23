# Spotify new markets listening habits visualization

## Overview

This project aims to analyze Spotify streaming data, focusing on emerging markets and their music consumption patterns. Using data from various regions over the last three years, the goal is to provide insights into the evolving music trends in these countries and visualize the findings using Power BI.

<p align="center">
  <img src="images/Dashboard Overall Streams.png">
</p>
<p align="center">
  <img src="images/Dashboard Track Charts performance.png">
</p>
<p align="center">
  <img src="images/Dashboard Audio distribution.png">
</p>

## Context
The idea of this project took root when I listened last February 2024 a podcast on France Culture, where the vice-president of Deezer were interviewed. What particularly struck me was that he stated that Deezer but also Spotify are not profitable yet but hopes to be in 2025.

While streaming platforms are becoming increasingly important in the music industry, it is clear that their business model is not yet sustainable. How can this be explained? What strategies have been put in place to address this?

The market is extremely competitive, which means that platforms are "forced to invest heavily to continue to develop". And the interview concluded in some way:
"the real key for streaming platforms is to succeed in penetrating countries with a large demographic with a very young audience, particularly in Africa, India or Latin America," taking into account the problem that "each country has its own musical and sound culture: its own media." —> understanding the culture in these emergent countries is fundamental in order to create suitable playlists and recommendations.

## Project Structure
From the data that I will be able to find and retrieve, I want to create a dashboard that helps to better understand music culture and listening habits in these key countries of large demography and young audience.
For this, the project follows the process from data acquisition to modeling and visualization, as described below:

### 1. Data Scraping
- **Source**: The primary data comes from [Spotify's charts](https://charts.spotify.com/charts/view/regional-global-weekly/latest).
  - Weekly ranking (top 200) songs by number of streams (1 stream = minimum 30s of listening)
  - Categorized by users from a specific country (more than 70) and globally (on users worldwide).

  **Choice of study framework:**

  For the scope of this project and considering the deadline, I've decided to limit the data according to:
  - 15 countries + Global
    - USA
    - Europe (France, Turkey)
    - Africa (Nigeria, Egypt)
    - Asia (Pakistan, India, Indonesia, Philippines, Vietnam, Taiwan, Japan)
    - Latin America (Brazil, Mexico, Argentina)

  - over the last 3 years (**156** weeks until 02/22/2024)

- **Web Scraping**: A Python script was used to scrape the Spotify charts data.

  You can read the code in the following notebook: [`01_charts_scrapping.ipynb`](code/01_charts_scrapping.ipynb)

- **API Integration**: Additional track and artist information were retrieved using the [Spotify API](https://developer.spotify.com/documentation/web-api), requiring valid API credentials and access tokens.

  The code is in this notebook: [`02_api_spotify.ipynb`](code/02_api_spotify.ipynb)

### 2. Data Handling
- **Collected Files**: The raw data is stored in CSV files:
  - `charts.csv`: Contains data from the web scraped charts.
  Here a description of the main attibutes collected for a weekly song:
    - **trackName**, **track_ID**, its **image** and **releaseDate**, the **artist** (only the main one) and **artist_ID**
    - **Rank**: ranking in the chart (1 to 200)
    - **PeakRank**: highest ranking reached for this track
    - **PeakDate**: date on which this peak is reached for the first time
    - **WeeksOnChart**: Total weeks of presence in the chart
    - **Streak**: Total consecutive weeks (if equal to WeeksOnChart, the track has never left the charts)
    - **Streams**: target value to aggregate
    - **Trend**: 'moved up', 'moved down', 'no change', 'new entry', 're-entry'
    - **entryRank**: rank at first entry in the chart
    - **entryDate**: date of its first entry
    - **Week**: date of the corresponding week of the chart

  - `tracks.csv`: Holds for each track ID, the track metadata such as track name, release date, and features like:
    - the **external_url** so we can access to spotify and hear the song after clicking on it
    - 5 descriptors: **duration_ms**, **tempo**, **key**, **mode**, **time signature**
    - 4 confidence measures: **acousticness**, **liveness**, **speechiness**, **instrumentalness**
    - 4 perceptual measures: **energy**, **loudness**, **danceability**, **valence**

     You will find the meaning of all these parameters in the [API documentation](https://developer.spotify.com/documentation/web-api/reference/get-audio-features)

  - `artists.csv` & `genres.csv`: Include for each artist ID for further enrichment, its **popularity** and **followers** and **genre** of the music (as strange and inconvenient as it may seem, in Spotify, the genre is attached to the artist and not the track).
  
- **Data Cleaning**: The scraped data underwent a cleaning process to remove duplicates, rename columns, and ensure uniformity across different files.

### 3. Data Modeling
You can read the code in the following notebook: [`03_data_modeling.ipynb`](code/03_data_modeling.ipynb)

- **Fact and Dimension Tables**: The project follows a star schema approach:
  - **Fact Table (FACT_CHART)**: Consolidates all `charts.csv` data across 15 countries and global charts for three years. Each row represents a track’s rank for a specific country and week.
  - **Dimension Tables**:
    - `DIM_Track`: Consolidates track data from `tracks.csv` and links with the fact table to retrieve track names and release dates.
    - `DIM_Artist` and `DIM_Genre`: Created by merging artist and genre information with the fact table.
    - `DIM_Region`: Adds regional identifiers to the fact table for easier analysis across different countries.

Here the final Star schema in Power BI:
<p align="center">
  <img src="images/Star Shema.png">
</p>

### 4. EDA

A first analysis of the datas is done in this notebook: [`04_first_analysis.ipynb`](code/04_first_analysis.ipynb)


### 5. Data Visualization
- **Power BI Dashboard**: 
See pictures above
  - Visualizes key metrics such as the number of streams, top-performing artists, and weekly chart movements across different regions.
  - Includes filters by region, time period, artist, and genre to allow in-depth analysis.
  - Highlights trends in music consumption in countries like Nigeria, Indonesia, Brazil, and more.
  
  Key insights include:
  - Growth in streams across emerging markets.
  - Popularity of genres like pop, reggaeton, hip hop, and regional styles.
  - Detailed artist performance, tracking streams and chart positions over time.

- **Data Analysis**:

**1. Overall Streams: A Gateway to Market Growth**

The data shows that streaming activity has surged significantly in emerging markets like India, Brazil, and Nigeria over the past three years. Countries like Indonesia and Vietnam, traditionally seen as smaller players in the global music scene, have exhibited remarkable growth rates, with streams nearly doubling year over year. For instance, Indonesia's streams rose by 84.8% over the last three years, making it one of the most dynamic markets. This trend underscores the potential of tapping into young, fast-growing populations in these regions.

The data from the emerging markets, with their explosive growth, suggest a path forward: the key to sustainability lies in increasing penetration in these countries. While streams are up, Spotify must tailor its offerings to local tastes and habits, which vary significantly across regions.

**Key Insight:** Platforms like Spotify must localize their strategies—understanding the local cultures and preferences is crucial to unlocking long-term profitability. This includes curating playlists and media that resonate with these audiences, which we will see further in the breakdown of track and artist performance.

**2. Track Charts Performance and Audio Distribution: Navigating Cultural Nuances**

Looking at individual track performance across emerging markets reveals a striking diversity in musical tastes. The charts in regions like Nigeria and India show a preference for local genres such as Afrobeats and Bollywood soundtracks, which are largely absent in Western-dominated markets. For example, Bad Bunny, though globally dominant, takes a back seat in countries like India, where regional hits dominate the charts. An other example is the prominence of acoustic music in central or south Asia countries or in Brazil (with much more "live" music in this country than all others). That demonstrates that local traditional music and folklore specific to these countries is of prime importance in the culture of thses countries, even inside the young population.

This diversity in musical preference highlights the challenge noted in the podcast: “each country has its own musical and sound culture.” If Spotify aims to convert streams into sustainable revenue, it must address the cultural nuances that define listening habits in different countries. The platform’s global strategy—such as focusing heavily on Western genres—won't work in countries where cultural identities are deeply embedded in music consumption.

**Key Insight:** In order to succeed, Spotify needs to customize its content strategy by deeply integrating local music genres, creators, and culture into its offerings. This could involve working with local artists to create authentic content that resonates with native listeners.  

### 6. Limitations
- The data focuses on mainstream tracks and does not fully represent the diversity of music within each region.
- User-specific data (such as demographic information or user behavior) was not accessible for this study.
  
### 7. Future Work
- Extending the dataset to cover up to 10 years of streaming data for historical trend analysis.
- Implementing an ETL pipeline using SSIS to automate weekly data updates in Power BI.

## Conclusion: Navigating the Global Music Landscape
This project presents an insightful analysis of global and regional music trends using Spotify data. The findings help understand the growth and popularity of music genres across various emerging markets, visualized effectively through Power BI.

The analysis of Spotify's streaming data in emerging markets aligns closely with the challenges outlined in the podcast. The rapid rise in streams from countries like Indonesia, Nigeria, and India signals the importance of demographic-rich regions to Spotify's future growth.
The future of streaming platforms like Spotify hinges on creating deeper connections with audiences in these markets—understanding what they listen to, why they listen to it, and how to keep them engaged. For platforms struggling with profitability, this data-driven approach could represent a roadmap to long-term success.

## Tools & Technologies
- **Data Scraping**: Python
- **Data Storage**: CSV
- **Data Visualization**: Power BI
- **API**: Spotify API

Thank you for exploring this project!


