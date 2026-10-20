# P0 - Need, Audience, Dataset & Feasibility

## Working Title

Guiding Book Acquisition Decisions for an Independent Publishing House

## Audience and Need

### Specific audience, role, or user group

The acquisition board or committee which is often composed of the Aquisitions Editor, the Editor-in-Chief, Sales & Marketing Directors, the Publisher. The acquisition editor is the one who initially reads the manuscript and pitches the proposal and advocates for the book's potential. The Editor-in-Chief, also known as the Editorial Director, gives the inital green-light to move the proposal to the committee after checking to see if the book would align with the publisher's image or brand. The Sales & Marketing Directors look at the proposed book as compared to other already published titles to determine how well the book would actually sell and who the target audience is; if they don't think it will sell, it will likely be rejected. The Publisher, often the business leader of a publishing house holds the ultimate veto power and does the final financial sign-off. They look closely at profit-and-loss (P&L) statements prepared by the editor to ensure the company is making the right business decision and to determine the advance, printing costs, and projected sales. This project will be mostly focused on the Publisher's role, however every position on this committee holds significant power in the book acquisition process and gives us a more imformed understanding of the best business approaches the proposed analytical product will need to take.

### Decision, recurring task, or problem that audience faces

The recurring task that this audience faces is whether or not to "acquire", or agree to publish, a proposed book or series of books based on various financial factors such as projected sales of that book/series, the advance the author would be given, the printing costs for this book/series, and the P&L statements. In addition to the financial costs, the manuscript will have to show enough potential that it will be a competitive book/series in bookstores, that it can capture the attention of an agent willing to advocate for the book, and that it aligns with the publishing house's branding.

### Why a data product could help

A data product could help the acquisition board to stay informed on the trends in book sales and reader activity to help them make an informed decision on whether the book would align well with current trends or, on the other hand, if it wouldn't balance aligning with the trends and still being able to stand out on its own as a unique and appealing new read.

### What the audience might do differently if the product is useful

If the product is useful, the audience would be able to more quickly understand whether or not a manuscript has the potential to be successful so that they can then take on more potential manuscripts to keep up with the increasing numbers of authors wanting to publish. This can also benefit more than this specific audience, as it can inform authors how their manuscript might fare in the current market.

## Proposed Analytical Product and Questions

The proposed analytical product is a dashboard that will capture trends in book sales based on genre, tropes, subject matter, target reader audience, etc. Additionally, it will show how these trends ebb and flow over the years to help the acquisition committee since often a book that is presented to this committee is still a year or more out from actually being on the shelves. It would be highly informative for this committee to see the project direction of trends to help with their decision process.

Possible Questions the proposed model should answer:
- What are the current trends in booksales across reader audience, genre, trope, or subject matter?
- What have the trends been like historically? Are some more consistent or do some spike and fall rapidly? How risky would a certain book be?
- What are the directions of the projected futures for these categories?
- What reader audiences are more likely to actually purchase a book? – This one may not be answerable with the given dataset.

## Source Identification and Access

- Source dataset name and provider: Checkouts by Title, Seattle Public Library and David Christensen
- Source URL: https://catalog.data.gov/dataset/checkouts-by-title 
- The BigQuery project and dataset identifiers visibile: Identifier given from the source page: https://cos-data.seattle.gov/api/views/tmmm-ytt6
- Candidate tables or views and their location/region: Location is the US, currently they exist outside of BigQuery in a folder containing CSV files for each of the years included in the dataset.
- Confirmation that the source is approved and currently accessible from the course environment: Access level is set to public, published by the city of Seattle
- Any visible use, attribution or redistribution constraints relevant to the project: Unsure at the moment.

## Quick Source Profile

- Apparent meaning of one row in each candidate object: Each row represents the number of checkouts of a particular title for a particular month.
- Current row count: Original dataset was over 50 million rows, I used an API to pull 100,000 random rows from the years 2006-2025. 2 million total rows.
- Relevant date or time coverage: 2006-2025
- Possible IDs or keys, what each identifies, and whether values are missing or duplicated: Had lots of missing ISBN values which caused discrepencies in number of columns for each table (year) which needed to be fixed. Because of this, ISBN would not be a good ID or key, instead it might make sense to add a unique key column that would then have a unique key for each instance.
- Fields likely to support the proposed questions: `subjects`, `checkouts`, `checkoutyear`, `checkouttype`
- Important null, duplicate, type, or semi-structured characteristics: Currently lots of nulls with the 'ISBN' column.
- Known refresh behavior: Dataset was first uploaded on June 3, 2022 and most recently updated on September 6, 2026.
- Any obvious sensitivity, aggregation, or interpretation concern: Currently each year is a separate table. Not sure if I should make a master table with everything and then load that into BigQuery or if there is a different way I can load in a dataset with 20 tables into BigQuery.

**Note:** Because of the many issues surrounding the API and the ISBN column issues, I have not yet loaded into BigQuery to run preliminary queries. I have, however, done some EDA in a couple of jupyter notebooks that I will attach to my submission. As soon as I resolve this issue, I will begin the profiling queries.

## Need-to-Source Fit

| Proposed question | Needed fields or concepts | Candidate source object(s) | Why the source appears sufficient| Gap or assumption to test in P1 |
|---|---|---|---|---|
|What are the current trends in booksales across reader audience, genre, trope, or subject matter?|`subjects`, `checkouts`, `checkoutyear`, `checkouttype`|Unnamed Table|Allows me to look at various aspects of a book and the frequency at which it was checkout from the library. This can then be used to inform me on favored books which will help with acquisition decisions.|Will need to look more closely at what `subject` refers to and whether or not that can be used as an indicator for genre.|
|What have the trends been like historically? Are some more consistent or do some spike and fall rapidly? How risky would a certain book be?|`subjects`, `checkouts`, `checkoutyear`, `title`, `checkoutmonth`|Unnamed Table|I have data across 20 years up until the most completed year at the time of this project which will allow me to look both at how trends have behaved as well as let me make more accurate predictions for future trend directions.|Will need to look more closely at what `subject` refers to and whether or not that can be used as an indicator for genre. Will also need to somehow combine the tables without losing their delicate structure.|
|What are the directions of the projected futures for these categories?|`subjects`, `checkouts`, `checkoutyear`, `title`, `checkoutmonth`, `publicationyear`|Unnamed Table|I have data across 20 years up until the most completed year at the time of this project which will allow me to look both at how trends have behaved as well as let me make more accurate predictions for future trend directions.|Will need to look more closely at what `subject` refers to and whether or not that can be used as an indicator for genre. Will also need to test to make sure 20 years is enough time to see how historical trends might inform us on future trends.|

**Note:** The unnamed table refers to the future name of the table that will include all years 2006-2025. Will likely be something along the lines of `yearly_checkouts`.

## Feasibility Risks and Response

A couple potential risks that could stop this project are the high volumn of null/missing ISBN values, the lack of an official genre feature, the possibility that my API caused some mismatching of columns and values due to the differences in column numbers for years when ISBN was not included, and maybe the source size/query costs. Because I haven't loaded into BigQuery yet, the last concern is more general. I do have several jupyter notebooks with python code repairing some of these issues as well as with the API code, however, despite my checks, it is good to err on the side of caution.

## First P1 Decision

Will need to first either make a master table or learn how to work across multiple tables in BigQuery to create my candidate object. Will also need to look into the `subject` column to see how that was interpreted by the library and whether or not that can be used either as a substitute for genre or as a way to make an inference on what the genre would be.

## AI/Tool Accountability

Used AI to find a free, public dataset. Used AI to code an API to pull 100,000 random rows from each of the years between 2006 and 2026 for a smaller, more workable dataset. Following this, some tables (years) had missing ISBN values which meant that those tables (years) only had 11 columns, where the tables that did have ISBN values had 12. Used AI to help guide a safe cleaning and repairing of this issue.

