# Measures and calculated columns

The DAX from Power BI into each block.

## Measures

### Participant Count

**Purpose:** number of survey participants in the current filter context. Used in the KPI card, the column series of the mental health combo chart, the treemap tooltip and the platform and country bars.

```dax
Participant Count = DISTINCTCOUNT(
    'Students Social Media Addiction'[Student_ID]
)
```

### Self Reported Impact

**Purpose:** share of participants who answered Yes to `Affects_Academic_Performance`. Shown as the 64% KPI card. Formerly named `Affected`.

```dax
Self Reported Impact = Affected = DIVIDE(COUNTROWS(FILTER('Students Social Media Addiction','Students Social Media Addiction'[Affects_Academic_Performance]="Yes")),COUNTROWS('Students Social Media Addiction'))
```

## Calculated columns

### Usage Band

**Purpose:** groups `Avg_Daily_Usage_Hours` into under 3, 3-4, 4-5, 5-6 and 6+ hours. Used on the Reported academic effect chart.

```dax
Usage Band = 
VAR Hours = 'Students Social Media Addiction'[Avg_Daily_Usage_Hours]
RETURN
    SWITCH(
        TRUE(),
        ISBLANK(Hours), BLANK(),
        Hours < 3, "Under 3 hours",
        Hours < 4, "3–4 hours",
        Hours < 5, "4–5 hours",
        Hours <= 6, "5–6 hours",
        "6+ hours"
    )]
```

### Usage Band Order

**Purpose:** numeric sort key so the bands display from lowest to highest usage.

```dax
Usage Band Order = 
VAR Hours = 'Students Social Media Addiction'[Avg_Daily_Usage_Hours]
RETURN
    SWITCH(
        TRUE(),
        ISBLANK(Hours), BLANK(),
        Hours < 3, 1,
        Hours < 5, 2,
        Hours <= 6, 3,
        Hours > 6, 4
    )
```

## Aggregations set on visuals

| Visual | Field | Aggregation |
|---|---|---|
| KPI card, average daily usage | `Avg_Daily_Usage_Hours` | Average |
| KPI card, average mental health score | `Mental_Health_Score` | Average |
| KPI card, average sleep per night | `Sleep_Hours_Per_Night` | Average |
| KPI card, average addiction score | `Addicted_Score` | Average |
| Average addiction score by platform | `Addicted_Score` | Average |
| Average daily usage by academic level and gender | `Avg_Daily_Usage_Hours` | Average |
