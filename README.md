# Finding Joy: Sonoma Wine Representation at a Denver Bottle Shop

## What I Set Out To Do

I recently moved from Sonoma, CA to Denver, CO, and found myself drawn to a wine bottle shop in my new neighborhood, Joy Wine & Spirits. Part of the draw is their proximity to me, and part is how much small-scale Sonoma wine they seem to carry. "

This project set out to quantify that instinct: how does Joy Wine's Sonoma County representation compare to what Sonoma County actually produces? Do they actually over-index on small-production labels, or am I just seeing what I want to see? Are those Sonoma labels the kind you'd find anywhere?

## What I Found

There are 221 liquor stores with active liquor licenses in Denver, CO. There are 3 within walking-distance to my house (I defined walking distance as within 15 minutes).

Sonoma County accounts for roughly 5% of West Coast wine production, but makes up about 11% of Joy's West Coast wall.

The median Sonoma label in their inventory produces around 8,750 cases per year.

I wanted to contrast this to the type of Sonoma County wine you'd likely find *anywhere*, at your grocery store, at any given restaurant, at any general liquor store. My desk research identified Jackson Family Wines as the highest-producing Sonoma winery. Jackson Family wines produces upwards of 6 million cases annually. Joy isn't just carrying Sonoma wine, they're carrying a very particular, small-production slice of it.

## Data Collection

- **U.S. wine production by state** — Scraped from [World Population Review](https://worldpopulationreview.com/state-rankings/wine-production-by-state). Data reflects 2023 annual production figures sourced from federal reporting.
- **Sonoma County production estimate** — Derived from acreage data at [sonomacounty.com](https://www.sonomacounty.com/wine/sonoma-wine-facts/), converted to gallons using standard industry ratios (3,958 bottles per acre, 39 oz per bottle, 128 oz per gallon).
- **Joy Wines inventory** — Collected two ways. The composition of the West Coast wall (252 unique labels, 27 from Sonoma) was hand-collected during an in-person visit on June 23, 2026 around 5 p.m. Label-level inventory data was collected by identifying the underlying JSON API endpoint the Joy Wines website uses to render its inventory, and saving the response directly.
- **Annual case production per label** — Researched manually for each Sonoma label in Joy's inventory using [Wine Industry Data](https://www.wineindustrydata.com/lightsearch) and supplementary web searches. Penny Island was excluded as no reliable production data was available.
- **Denver, CO Liquor Store Location Data** - Collected via the [City and County of Denver website](https://www.denvergov.org/Maps/map/liquorlicenses)
- **Location and mapping data** — Collected via the Google Maps API.
- **Walking duration data** — Collected via the Google Places API.

## Analysis Overview

The analysis is documented in `joy_wines_denver_map.ipynb` and `joy_wines_west_coast_wine_analysis.ipynb`. At a high level:

### For mapping

1. Downloaded Denver Liquor License data, then filtered for "active" retail and tasting licenses.
2. Converted the x and y coordinates from the Colorado-specific coordinate plane to latitude and longitude.
3. Used the Google Places API to get walking distance to each of the Denver area liquor stores.
4. Filtered for those within a 15 minute (900 second) walk from my home.

### For wine representation
2. Scraped and cleaned state-level wine production data, then calculated West Coast states' share of national production.
3. Estimated Sonoma County's share of West Coast production using acreage and yield data.
4. Parsed the Joy Wines inventory JSON to extract Sonoma brand names, then concatenated with two hand-collected labels missing from the API response.

### For wine label annual production data
5. Built a dataframe of annual case production per label from manually researched data.
6. Calculated the median case count across Joy's Sonoma inventory and compared it to Jackson Family Wines as a mass-market benchmark.

## New Skills and Approaches

This project pushed me in several directions I hadn't gone before. I learned how to use the **network technique**, where I used the browser's network tab to identify the underlying API endpoint the site was calling and pulled the JSON response directly.

I also used the **Google Maps and Places APIs** for the first time, to collect location data and calculate walking durations between my home and nearby liquor stores. I also learned about local coordinate plane projections and how to convert them to a more useful lat and lon.

## What I'd Do Differently With More Time

The most frustrating limitation was the **manual case count research**. I had hoped to automate this using Playwright to scrape Wine Industry Data, but my unfamiliarity with Playwright coupled with the websites bot protections meant I couldn't quite figure it out. I ended up hand-researching every label, which was slow and doesn't scale.