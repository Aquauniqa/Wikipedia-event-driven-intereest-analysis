# Methodology

## 1. Objective

The project analyses how interest in Polish Wikipedia changed alongside selected social, cultural and media events. It also examines how the Polish Wikipedia community changed over time and how editing activity is distributed across editor types and namespaces.

The work was designed as an end-to-end analytical project rather than a single-dashboard exercise. It combines source assessment, validation, large-file processing, dimensional modelling, contextual research and interactive storytelling.

## 2. Source assessment

### Files 1–9

Each of the first nine datasets was profiled before use. The assessment included:

- file dimensions and column names;
- observation grain;
- data types;
- date coverage;
- potential keys;
- missing values;
- duplicate records;
- join risks and other data-quality issues.

This step determined which fields could safely be used in relationships and which transformations were required before modelling.

### File 10

File 10 represented a substantially larger page-view dataset. It was separated from the smaller-source workflow and processed in Python with pandas using chunked reads.

## 3. Layered data architecture

### RAW

Source data was retained without analytical transformations. This created a stable reference point for validation.

### Silver

The Silver layer contained cleaned and standardised tables. Tasks included type correction, structural validation, null and duplicate checks, key verification and preparation for joins.

### Gold

The Gold layer contained analysis-ready tables, measures, monthly rankings and compact exports designed for Power BI and Flourish.

## 4. Python processing

The Python workflow was designed to avoid loading all monthly records into memory at once.

For every compressed TSV.GZ file, the process:

1. opened the source in pandas using a defined chunk size;
2. validated expected columns and data types;
3. checked nulls, duplicates and `page_id`;
4. aggregated page views by month and page;
5. calculated control statistics: record count, sum, mean, minimum and maximum;
6. retained the top 1,000 `page_id` values by monthly views;
7. assigned monthly ranks;
8. combined the monthly results;
9. exported control and analytical tables to CSV.

The loop automated processing of 126 monthly files. Preserving `page_id` allowed the resulting tables to be joined back to article metadata without relying on mutable article titles.

## 5. Power BI model

The Power BI model used explicit fact-to-dimension relationships.

Key components included:

- `fact_monthly_activity`, combining files 2, 3 and 4;
- `dim_article` for article-level attributes;
- `dim_map` for mapping or classification fields;
- the `Is Polish only` attribute from file 6;
- monthly view and edit rankings;
- measures used to identify changes in attention and longer-term community trends.

The model was also used to reshape results into the compact, chart-specific tables required by Flourish.

## 6. Selection of the 11 moments

The selected moments were derived from visible changes in the data and then reviewed against external event context.

Selection was purposeful rather than purely mechanical. A moment had to:

- show a meaningful change in attention;
- have a plausible and documentable real-world context;
- contribute a different type of story to the final narrative;
- remain interpretable in an interactive presentation.

The 11 moments are therefore not claimed to be the 11 largest anomalies in the full dataset. High-volume evergreen pages without a clear event-driven change were excluded from this curated set.

## 7. Visual encoding

In the event chart:

- horizontal position represents time;
- vertical position represents the change in interest, expressed in percentage points;
- bubble size represents page views;
- colour distinguishes articles inside and outside the monthly top 100 edited pages.

The final section uses a more conventional dashboard layout to summarise current editing activity and to complement the earlier story-driven views.

## 8. Validation

Validation occurred at several points:

- structural checks before processing;
- null, duplicate and key checks;
- monthly control statistics after chunk aggregation;
- preservation of `page_id` across transformations;
- row-count and join checks when combining tables;
- visual review of Power BI exports after loading them into Flourish.

## 9. Limitations

- Event labels add researched context but do not establish causality.
- Page views measure attention, not sentiment or intent.
- Curated moment selection introduces analyst judgement.
- The top-1,000 monthly subset omits lower-ranked pages from detailed article-level analysis.
- Rolling averages trade short-term sensitivity for clearer long-term patterns.
- Reproduction requires the original source definitions and processing scripts in addition to this documentation.
