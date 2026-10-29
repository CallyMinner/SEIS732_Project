# P1 - Grain, Model, History, & Pipeline Plan
### Cally Minner
### SEIS 732, Fall 2026

## Establish the Question and Check the Source
The acquisition committee for an indie publishing house needs a way to view historical book trends in the last 20 years to determine the projected trends to guide the manuscript acquisition process and to make informed decisions that would benefit the company.

The primary questions are what are the historical book trends over the last 20 years and what are the project future trends based on this historical data?

I will be using the `books_project` dataset and the `books` table in BigQuery. This dataset includes 2 million rows with 100,000 rows of aggregated data for each year between 2006-2025. This means I am not looking at individual checkout transactions, but rather the amount of checkouts per month for a given book.

Proposed grain: One row represents the number of `checkouts` for a specific `title`, by `creator`, during a particular `checkoutmonth` and `checkoutyear`, categorized by `materialtype`, `checkouttype`, and `usageclass`.
- Interestingly, there are currently 514 identified rows that are repeated key groups.
- There is also no unique ID for each row.

## Define the Model and Predict a History Consequence
Business Process: 

Fact Table: Periodic snapshot. We are looking at the amount of checkouts per month for a given book over a 20 year period.

Proposed Grain: One row represents the number of `checkouts` for a specific `title`, by `creator`, during a particular `checkoutmonth` and `checkoutyear`, categorized by `materialtype`, `checkouttype`, and `usageclass`.

Principle Fact: `checkouts`

Using the proposed schema below, I will look at how library checkout patterns have changed over a 20 year timespan to then apply this understanding for future projections. Knowing what the current trends are will help make the best decision from a business standpoint on what type of manuscripts should be acquired for the publishing house.

Star Schema:

erDiagram
    DIM_DATE ||--o{ FACT_CHECKOUTS : "date_key"
    DIM_ITEM ||--o{ FACT_CHECKOUTS : "item_key"
    DIM_CHECKOUT_CATEGORY ||--o{ FACT_CHECKOUTS : "category_key"

    FACT_CHECKOUTS {
        INT date_key FK
        INT item_key FK
        INT category_key FK
        INT checkouts
    }

    DIM_DATE {
        INT date_key PK
        INT checkoutyear
        INT checkoutmonth
    }

    DIM_ITEM {
        INT item_key PK
        STRING title
        STRING creator
        STRING isbn
        STRING publisher
        INT publicationyear
    }

    DIM_CHECKOUT_CATEGORY {
        INT category_key PK
        STRING materialtype
        STRING checkouttype
        STRING usageclass
    }

One attribute that could cause issues is `materialtype`. This notates whether the book that was checked out was a physical book, and ebook, an audiobook, etc. However, a publishing house doesn't care about the media type, just the title or genre. It may make sense to aggregate all titles month by month regardless of media type. There also might be different ways of notating the type. For example, ebook versus electronic book. This could be a Type 1 fix as it would just need to be standardized and edited, the version history in this case is not necessary and therefore wouldn't need a Type 2 handling. However, Type 2 handling could be helpful in preserving the documentation styles of the time period (with electronic media before 2022 and ebook after 2022, for a fake example). This is something I will have to deliberate further before P3 as it may or may not be something I use to answer my primary question. Although it could be useful information for secondary questions from a monetary standpoint (ebooks don't have the same printing costs). Additionally, while ISBN is included as an attribute in the item dimension, it is important to note that until the year 2022, the ISBN was not included in the dataset.

## Plan the Build and its Checks


## Diagnose and Revise the Plan



