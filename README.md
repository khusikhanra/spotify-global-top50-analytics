<div align="center">

# 🎧 Spotify Global Top 50 — Analytics Command Center

**A 4-page Power BI report that turns 18 months of daily chart data into answers about who dominates, who lasts, and what a "hit" is actually made of.**

![Power BI](https://img.shields.io/badge/Power%20BI-Desktop-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Custom%20Measures-1DB954?style=for-the-badge)
![Rows](https://img.shields.io/badge/Chart--Day%20Records-27%2C800-1E2A3E?style=for-the-badge)
![Period](https://img.shields.io/badge/May%202023%20→%20Nov%202024-556%20days-1E2A3E?style=for-the-badge)

<br>

<img width="880" alt="Executive Overview page of the Spotify Global Top 50 Power BI dashboard" src="https://github.com/user-attachments/assets/6b8f935f-be77-4e33-bdd9-3adf2516df7f" />

</div>

---

## 📑 Table of Contents

- [Why this project](#-why-this-project)
- [Headline findings](#-headline-findings)
- [Dashboard tour](#-dashboard-tour)
- [Dataset](#-dataset)
- [Methodology & metric definitions](#-methodology--metric-definitions)
- [DAX highlights](#-dax-highlights)
- [Data validation & known limitations](#-data-validation--known-limitations)
- [Getting started](#-getting-started)
- [Repository structure](#-repository-structure)
- [Author](#-author)

---

## 🎯 Why this project

A chart position tells you where a song is *today*. It says nothing about how long it stays, how much of the chart its artist controls, or whether that success is repeatable. This report separates those ideas:

| Question | Where it's answered |
|---|---|
| How healthy is the chart overall, and how does popularity move month to month? | **Executive Overview** |
| Which artists *dominate* the chart, and which tracks combine a high peak with real staying power? | **Track & Artist Deep-Dive** |
| What does the catalog look like: duration, explicit content, collaborations, release type? | **Catalog & Content Insights** |

The goal is a dashboard an executive can scan in 30 seconds and an analyst can interrogate for an hour, backed by metrics that are defined explicitly and checked against the raw data.

---

## 🔎 Headline findings

All figures below were recomputed directly from the raw CSV.

| # | Finding | Evidence |
|---|---|---|
| 1 | **Taylor Swift is in a league of her own.** | 1,871 chart appearances (6.7% of every chart slot in the dataset) and a dominance score of **47,309**, roughly **1.7×** the runner-up, Sabrina Carpenter (27,103). Billie Eilish follows at 26,447. |
| 2 | **Longevity and dominance are different things.** | *I Wanna Be Yours* (Arctic Monkeys) charted for **548 days but peaked at #11 and never entered the Top 10.** *Cruel Summer* charted 517 days and spent **253 of them in the Top 10**. |
| 3 | **Her dominance is built on breadth.** | Taylor Swift placed **85 distinct tracks** on the chart, nearly 3× the next artist (Travis Scott, 30). |
| 4 | **Singles outlast album tracks per song.** | Singles are 33.1% of unique tracks but 37.8% of chart-days, averaging ~38 days on chart vs ~32 for album tracks. |
| 5 | **Explicit tracks churn faster.** | Explicit tracks are 44.7% of unique tracks but only 40.2% of chart-days, averaging ~30 days on chart vs ~36 for clean tracks. |
| 6 | **Collaborations carry a small popularity premium.** | Tracks with a featured/joint artist average **90.7** popularity vs **89.4** for solo tracks (+1.3 pts). |
| 7 | **Explicit content shows no meaningful popularity penalty.** | The gap is −0.2 pts with zero-popularity rows included and flips to +0.2 pts when they are excluded, i.e. statistical noise. |
| 8 | **Chart popularity is uniformly high.** | Mean popularity is **89.6** (90.2 after excluding 182 records where the API returned 0). Fewer than 6% of chart-days score below 80. |

> ⚠️ Findings 6 and 7 are descriptive differences in averages, not causal claims. No significance testing was performed.

---

## 🖥️ Dashboard tour

### 1 · Executive Overview
High-level KPIs, the monthly popularity trend, top artists, release-type mix, and explicit vs. clean tracks over time. Slicers: **Year**, **Album Type**, **Explicit Content**.

<img width="100%" alt="Executive Overview page with KPI cards, monthly popularity trend, top artists, content mix and explicit vs clean tracks" src="https://github.com/user-attachments/assets/6b8f935f-be77-4e33-bdd9-3adf2516df7f" />

### 2 · Track & Artist Deep-Dive
Artists ranked by a custom **Chart Dominance Score**, a peak-rank vs. longevity scatter, a popularity trend, and a track table with a rule-based **Performance Tier**.

<img width="100%" alt="Track and Artist Deep-Dive page with dominance ranking, peak rank vs longevity scatter and performance tier table" src="https://github.com/user-attachments/assets/48fd6e36-73c4-468f-a1f7-7d587959f4b7" />

### 3 · Catalog & Content Insights
Duration distribution, explicit vs. clean split, collaboration share, and chart performance categories across the full catalog.

<img width="100%" alt="Catalog and Content Insights page with duration profile, explicit split and chart performance categories" src="https://github.com/user-attachments/assets/1b33cd55-d0c3-409d-9a6a-ddfd0c540f82" />

### 4 · About
Project background, data source, page guide, and methodology, embedded in the report so the definitions travel with the file.

<img width="100%" alt="About page describing project background, data source, page guide and methodology" src="https://github.com/user-attachments/assets/2b421344-5023-4f11-bfa1-ca0e9e0f73ea" />

---

## 🗂️ Dataset

| Property | Value |
|---|---|
| **Source** | Spotify Global Top 50 Daily Charts (Kaggle) |
| **Grain** | One row per *chart position per day* |
| **Rows** | 27,800 (556 days × exactly 50 positions) |
| **Date range** | 18 May 2023 → 27 Nov 2024 |
| **Unique artists** | 343 |
| **Unique tracks** | 827 (`song` + `artist`); 794 unique song names |

<details>
<summary><b>Data dictionary</b> (click to expand)</summary>

| Column | Type | Description |
|---|---|---|
| `date` | text (`dd-mm-yyyy`) | Chart date |
| `position` | integer (1–50) | Rank on that day, 1 = top |
| `song` | text | Track title |
| `artist` | text | Artist(s); collaborations appear as one combined string |
| `popularity` | integer (0–100) | Spotify popularity index on that date, derived from play volume and recency |
| `duration_ms` | integer | Track length in milliseconds |
| `album_type` | text | `album`, `single`, or `compilation` |
| `total_tracks` | integer | Number of tracks on the parent release |
| `release_date` | text | Release date. Mostly `dd-mm-yyyy`, but 105 rows hold only a year or year-month |
| `is_explicit` | boolean | Explicit-content flag |
| `album_cover_url` | text | Cover art URL |

</details>

**Structural checks that passed:** no null values, no duplicate rows, no duplicate `(date, position)` pairs, exactly 50 positions on every one of the 556 days, and no track with a release date later than its chart date.

---

## 📐 Methodology & metric definitions

| Metric | Definition |
|---|---|
| **Chart Dominance Score** | Per artist, the sum of `(51 − position)` over all of that artist's chart appearances. A #1 placement earns 50 points, a #50 placement earns 1. It rewards both rank strength *and* frequency. |
| **Artist Share %** | An artist's dominance score as a share of the total in the current filter context. |
| **Peak Rank** | Best (lowest-numbered) position a track reached during its full run. |
| **Days in Top 10** | Count of distinct chart-days a track held position ≤ 10. |
| **Performance Tier** | Rule-based, applied in this order: 💎 *All-Time Mega Blockbuster* (peaked #1 and 30+ Top-10 days) → 🔥 *Superstar Track* (peaked Top 5 and 15+ Top-10 days) → ⭐ *Consistent Hit* (peaked Top 10) → ☑️ *Steady Performer* (all other charted tracks). |
| **Avg Popularity** | Mean of the daily popularity score across chart-day records. |
| **Collaboration** | Artist string contains `&`, `feat`, or `ft.` (see [limitations](#-data-validation--known-limitations)). |

---

## 🧮 DAX highlights

Collaboration detection is done with string search over the artist field, then solo tracks and collaboration share are derived from it:

```dax
Collaboration Songs Count =
CALCULATE (
    DISTINCTCOUNT ( 'Spotify-Top-50-World'[song] ),
    SEARCH ( "&",    'Spotify-Top-50-World'[artist], 1, 0 ) > 0
        || SEARCH ( "feat", 'Spotify-Top-50-World'[artist], 1, 0 ) > 0
        || SEARCH ( "ft.",  'Spotify-Top-50-World'[artist], 1, 0 ) > 0
)

Solo Songs Count = [Total Tracks] - [Collaboration Songs Count]

Collaboration Share % =
DIVIDE ( [Collaboration Songs Count], [Total Tracks], 0 )
```

The model also includes measures for peak rank, days in Top 10, chart dominance, performance tier classification, duration formatting (`mm:ss`), and content-label logic.

---

## ✅ Data validation & known limitations

Transparency about what the data can and cannot support:

- **No track ID.** Tracks are identified by `song` + `artist`, which gives 827 tracks. Counting by song name alone gives 794, because **31 song names are shared by different artists** (for example, two different tracks titled *Beautiful Things*). Any song-name-level aggregation can merge unrelated tracks, so the grain used by each visual matters.
- **182 records have `popularity = 0`** (0.65% of rows). These look like API gaps rather than real scores and pull the mean down by about 0.6 pts (89.6 vs 90.2).
- **Release-type drift.** 30 tracks change `album_type` over the period and 117 appear with more than one cover image, typically a single later re-released on an album.
- **Partial calendar coverage.** The data starts mid-May 2023 and ends in late November 2024, so months are not equally represented: January–April appear only for 2024, and December only for 2023. Row counts by calendar month reflect this coverage, not chart activity, because every day contains exactly 50 rows by construction.
- **Rank-share KPIs are structural.** With a fixed 50-slot chart, "#1 share" is always 2% and "Top 10 reach" is always 20%. They describe the chart's design, not performance.
- **Collaboration flag is heuristic.** It catches `&`, `feat`, and `ft.` but not other separators (commas, `x`, `with`), so collaboration share is a lower bound.
- **Dates are text.** `date` and `release_date` are stored as `dd-mm-yyyy` strings, so date parsing in Power BI depends on regional settings.
- **Scope.** This is one global chart. Results do not generalize to regional charts, genres, or streaming counts, and the dataset has no genre field.

---

## 🚀 Getting started

**Requirements:** [Power BI Desktop](https://powerbi.microsoft.com/desktop/) (free, Windows).

1. Clone the repository
   ```bash
   git clone https://github.com/khusikhanra/spotify-global-top50-analytics.git
   cd spotify-global-top50-analytics
   ```
2. Open `Spotify_Global_Top50_Analytics.pbix` in Power BI Desktop.
3. If prompted for the data source, point it to `data/spotify_global_top_50.csv`
   (**Home → Transform data → Data source settings**).
4. Use the **Year**, **Album Type**, and **Explicit Content** slicers to explore.

---

## 📁 Repository structure

```text
.
├── README.md
├── Spotify_Global_Top50_Analytics.pbix   # Power BI report (model, DAX, visuals)
└── data/
    └── spotify_global_top_50.csv         # 27,800 chart-day records
```

---

## 👤 Author

<table>
<tr>
<td>

**Khusi Khanra**
Dashboard design & analysis

[![GitHub](https://img.shields.io/badge/GitHub-khusikhanra-181717?style=flat-square&logo=github)](https://github.com/khusikhanra)

</td>
</tr>
</table>

---

<div align="center">

*Data © original dataset authors and Spotify. This project is for educational and portfolio purposes and is not affiliated with or endorsed by Spotify.*

</div>
