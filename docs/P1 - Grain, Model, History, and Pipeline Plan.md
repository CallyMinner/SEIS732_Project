# P1 - Grain, Model, History, & Pipeline Plan
### Cally Minner
### SEIS 732, Fall 2026

## Establish the Question and Check the Source
The acquisition committee for an indie publishing house needs a way to view historical book trends in the last 20 years to determine the projected trends to guide the manuscript acquisition process and to make informed decisions that would benefit the company.

The primary questions are what are the historical book trends over the last 20 years and what are the project future trends based on this historical data?

I will be using the `books_project` dataset and the `books` table in BigQuery. This dataset includes 2 million rows with 100,000 rows of aggregated data for each year between 2006-2025. This means I am not looking at individual checkout transactions, but rather the amount of checkouts per month for a given book.

Proposed grain: One row represents the number of `checkouts` for a specific `title`, by `creator`, during a particular `checkoutmonth` and `checkoutyear`, categorized by `materialtype`, `checkouttype`, and `usageclass`.
- The issue currently is that there are 514 identified rows that are possible duplicates and there is no unique ID for each row.

## Define the Model and Predict a History Consequence


## Plan the Build and its Checks


## Diagnose and Revise the Plan



