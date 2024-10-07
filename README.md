# MTA Sample Pipeline

This pipeline aggregates data from two sources in the MTA Open Data Portal: MTA Wifi Locations (https://data.ny.gov/Transportation/MTA-Wi-Fi-Locations/pwa9-tmie/about_data) and Subway Ridership Data (https://data.ny.gov/Transportation/MTA-Subway-Hourly-Ridership-Beginning-February-202/wujg-7c2s/about_data).

I first uploaded the Wifi Locations data to Azure Data Lake Storage in CSV form, and then created a Databricks notebook to combine it with the Hourly Ridership data, which I accessed directly from the API endpoint. In my Databricks notebook I used Pandas to manipulate the data, first joining the two datasets on the common column "station_complex" to add station information to each rider transaction. I then created a new column, "provider_available," which notes whether any of the four phone providers (AT&T, Verizon, Sprint,  or Mobile) are available at that station. The final dataset includes three columns: "transit_date," "is_historical," and "provider_available." The "is_historical" column comes from the station data and indicates whether any part of the station is considered a historical artifact. Therefore, the final data will note when a ride took place, whether there was any kind of phone service at the station at which it originated, and whether that station has any historical significance.

This repo contains the code for the Azure Data Factory Pipeline that calls the Databricks notebook and then writes the cleaned data to an Azure SQL Database. The cleaned data goes to a SQL table called "aggregated_data," which includes the final three columns with SQL data types: "transit_date" (datetime), "is_historical" (varchar), and "provider_available" (bit).
