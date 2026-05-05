# 🎧 K-Pop Album Sales & Fan Engagement Analytics Dashboard

---

## 📊 Project Overview

This project is a comprehensive **data analytics solution** designed to evaluate Album Sales & Fan Engagement trends within the music industry. By transforming fragmented data from multiple platforms into a unified, **interactive dashboard**, this tool enables high-level executive decision-making and market analysis.

The dataset tracks **800+ albums** across **190 unique artists** and **37 record labels**, covering a total of **408.59 million physical sales** and **2.81 trillion digital streams**.

---

## 📦 Dataset Summary

| Attribute       | Details                                 |
| --------------- | --------------------------------------- |
| Dataset Scope   | Multi-source industry dataset           |
| Total Records   | 800 albums                              |
| Artists Covered | 190 artists                             |
| Labels Covered  | 37 labels                               |
| Time Span       | 2019 – 2026                             |
| Platforms       | Spotify, YouTube, Melon, Social Metrics |

---

## 🧾 DataDictionary - **MasterData** Sheet

| Column           | Formula                       |
| ---------------- | ----------------------------- |
| Index            | Table.AddIndexColumn          |
| Rank             | NA                            |
| Year             | NA                            |
| Album Name       | NA                            |
| Artist           | NA                            |
| ReleaseDate      | NA                            |
| Label            | NA                            |
| AlbumSales       | [KoreaSales] + [GlobalSales]  |
| KoreaSales       | NA                            |
| GlobalSales      | NA                            |
| SpotifyStreams   | NA                            |
| YoutubeStreams   | NA                            |
| MelonStreams     | NA                            |
| TwitterMentions  | NA                            |
| Weverse Posts    | NA                            |
| FanCafeMembers   | NA                            |
| Spotify_N        | [SpotifyStreams] / List.Max(finaldata[SpotifyStreams])  |
| Youtube_N        | [YoutubeStreams] / List.Max(finaldata[YoutubeStreams])  |
| Melon_N          | [MelonStreams] / List.Max(finaldata[MelonStreams])      |
| Twitter_N        | [TwitterMentions] / List.Max(finaldata[TwitterMentions])|
| Weverse_N        | [WeversePosts] / List.Max(finaldata[WeversePosts])      |
| FanCafe_N        | [FanCafeMembers] / List.Max(finaldata[FanCafeMembers])  |
| AlbumSales_N     | [AlbumSales] / List.Max(finaldata[AlbumSales])          |
| FES              | [Spotify_N]*0.18 + [Youtube_N]*0.17 + [Melon_N]*0.15 + [Twitter_N]*0.14 + [Weverse_N]*0.13 + [FanCafe_N]*0.12 + [AlbumSales_N]*0.11 |
| NormalizedFES    | [FES] * 100                   |
| Tier             | if [NormalizedFES] >= 75 then "LEGEND" else if [NormalizedFES] >= 60 then "ICONIC" else if [NormalizedFES] >= 40 then "RISING" else if [NormalizedFES] >= 20 then "SOLID" else "EMERGING" |
| Koreapct         | [KoreaSales] / [AlbumSales]   |
| Globalpct        | [GlobalSales] / [AlbumSales]  |
| Exportindex      | [GlobalSales] / [KoreaSales]  |
| Classification   | if [Globalpct] >= 0.6 then "🌍 Global-Led" else if [Koreapct] >= 0.6 then "🇰🇷 Korea-Led" else "⚖️ Balanced" |
| AlbumStreams     | [SpotifyStreams] + [YoutubeStreams] + [MelonStreams] |
| Month            | Date.MonthName([ReleaseDate])   |

---

## 🧾 DataDictionary - **Artistwise** Sheet 

| Column           | Formula                       |
| ---------------- | ----------------------------- |
| Artist           | NA                            |
| AlbumSales       | NA                            |
| KoreaSales       | NA                            |
| GlobalSales      | NA                            |
| SpotifyStreams   | NA                            |
| YoutubeStreams   | NA                            |
| MelonStreams     | NA                            |
| Koreapct         | [KoreaSales] / [AlbumSales]   |
| Globalpct        | [GlobalSales] / [AlbumSales]  |
| Exportindex      | [GlobalSales] / [KoreaSales]  |
| Classification   | if [Globalpct] >= 0.5 then "🌍 Global-Led" else if [Koreapct] >= 0.5 then "🇰🇷 Korea-Led" else "⚖️ Balanced" |
| BestGlobalAlbum  | let artist = [Artist],filtered = Table.SelectRows(tblmaster, (r) => r[Artist] = artist),maxVal = List.Max(filtered[GlobalSales]),result = Table.SelectRows(filtered, (r) => r[GlobalSales] = maxVal) in result{0}[AlbumName] |
| BestKoreaAlbum   | let artist = [Artist],filtered = Table.SelectRows(tblmaster, (r) => r[Artist] = artist),maxVal = List.Max(filtered[KoreaSales]),result = Table.SelectRows(filtered, (r) => r[KoreaSales] = maxVal) in result{0}[AlbumName]   |
| AlbumStreams     | [SpotifyStreams] + [YoutubeStreams] + [MelonStreams]  |
| StreamSaleRatio  | [AlbumStreams] / [AlbumSales] |
| LoyaltyClass     | if [StreamSaleRatio] >= 10000 then "🎧 Stream-Heavy" else if [StreamSaleRatio] >= 5000 then "⚖️ Balanced" else "🏆 Physically Loyal" |
| PlatformIndex    | [SpotifyStreams] / [MelonStreams] |
| PrimaryPlatform  | if [Platformindex] >= 5 then "🌍 Spotify Global" else if [Platformindex] >= 2 then "⚖️ Mixed" else "🇰🇷 Melon Korea" |
| Albums           | let artist = [Artist],filtered = Table.SelectRows(tblmaster, each [Artist] = artist) in Table.RowCount(filtered))) |
| UniqueAlbums     | let artist = [Artist],filtered = Table.SelectRows(tblmaster, each [Artist] = artist),uniqueAlbums = List.Distinct(filtered[AlbumName]) in List.Count(uniqueAlbums) |
| BestSales        | let artist = [Artist],filtered = Table.SelectRows(tblmaster, each [Artist] = artist),grouped = Table.Group(filtered, {"AlbumName"}, {{"AlbumSales", each List.Sum([AlbumSales]), type number}}),maxVal = List.Max(grouped[AlbumSales]) in maxVal|
| BestAlbum        | let artist = [Artist],filtered = Table.SelectRows(tblmaster, each [Artist] = artist),grouped = Table.Group(filtered, {"AlbumName"}, {{"AlbumSales", each List.Sum([AlbumSales]), type number}}),maxVal = List.Max(grouped[AlbumSales]),result = Table.SelectRows(grouped, each [AlbumSales] = maxVal){0}[AlbumName] in result|
| Label            | let artist = [Artist],filtered = Table.SelectRows(tblmaster, each [Artist] = artist),grouped = Table.Group(filtered, {"Label"}, {{"Count", each Table.RowCount(_), Int64.Type}}), maxVal = List.Max(grouped[Count]),result = Table.SelectRows(grouped, each [Count] = maxVal){0}[Label] in result |
| FirstAlbum       | let sorted = Table.Sort([AllRows], {{"ReleaseDate", Order.Ascending}}) in sorted{0}[AlbumName]|
| FirstSales       | let sorted = Table.Sort([AllRows], {{"ReleaseDate", Order.Ascending}}),firstAlbum = sorted{0}[AlbumName],filtered = Table.SelectRows([AllRows], each [AlbumName] = firstAlbum) in List.Sum(filtered[AlbumSales]) |
| FirstStreams     | let sorted = Table.Sort([AllRows], {{"ReleaseDate", Order.Ascending}}),firstAlbum = sorted{0}[AlbumName],filtered = Table.SelectRows([AllRows], each [AlbumName] = firstAlbum) in List.Sum(filtered[SpotifyStreams]) + List.Sum(filtered[YoutubeStreams]) + List.Sum(filtered[MelonStreams])|
| LatestAlbum      | let sorted = Table.Sort([AllRows], {{"ReleaseDate", Order.Descending}}) in sorted{0}[AlbumName]|
| LatestSales      | let sorted = Table.Sort([AllRows], {{"ReleaseDate", Order.Descending}}),latestAlbum = sorted{0}[AlbumName],filtered = Table.SelectRows([AllRows], each [AlbumName] = latestAlbum) in List.Sum(filtered[AlbumSales])|
| LatestStreams    | let sorted = Table.Sort([AllRows], {{"ReleaseDate", Order.Descending}}),latestAlbum = sorted{0}[AlbumName],filtered = Table.SelectRows([AllRows], each [AlbumName] = latestAlbum) in List.Sum(filtered[SpotifyStreams]) + List.Sum(filtered[YoutubeStreams]) + List.Sum(filtered[MelonStreams]) |
| Sales Δ          | [LatestSales] - [FirstSales]  |
| SalesGrowth      | if [FirstSales] = 0 then null else ([LatestSales] - [FirstSales]) / [FirstSales] |
| Stream Δ         | [LatestStreams] - [FirstStreams] |
| StreamGrowth     | if [FirstStreams] = 0 then null else ([LatestStreams] - [FirstStreams]) / [FirstStreams] |
| Momentum         | if [SalesGrowth] >= 0.5 then "🚀 Accelerating" else if [SalesGrowth] >= 0.1 then "📈 Growing" else if [SalesGrowth] >= -0.2 then "⚖️ Softening" else "⬇️ Declining")) |

---

## 🧾 DataDictionary - **Labelwise** Sheet

| Column           | Formula                       |
| ---------------- | ----------------------------- |
| Label            | NA                            |
| Albums           | NA                            |
| TotalSales       | [KoreaSales] + [GlobalSales]  |
| KoreaSales       | NA                            |
| GlobalSales      | NA                            |
| SpotifyStreams   | NA                            |
| YoutubeStreams   | NA                            |
| MelonStreams     | NA                            |
| Artists          | List.Count(List.Distinct([Artisttable][Artist])) |
| Market Share     | [TotalSales] / List.Sum(tblmaster[AlbumSales]) |
| ExportIndex      | if [KoreaSales] = 0 then null else [GlobalSales] / [KoreaSales] |

---

## 🚀 Key Highlights

* 📦 **800 Albums analyzed**
* 🎤 **190 Artists tracked**
* 🏷 **37 Record Labels evaluated**
* 🌍 **Global vs Korea market segmentation**
* 📈 **2.81 Trillion total digital streams analyzed**
* 💿 **408.59 Million physical album sales tracked**

---

## 🧠 Core Innovation: Fan Engagement Score (FES)

The **Fan Engagement Score (FES)** is a composite KPI designed to evaluate artist performance across multiple dimensions.

### 🔹 Components

* Physical Sales
* Streaming Metrics
* Social Engagement

### 🔹 Methodology

* Normalization (0–1 scaling)
* Weighted aggregation
* Final score scaled to **0–100**

### 🔹 Tier Classification

* 🏆 LEGEND
* 🌟 ICONIC
* 🚀 RISING
* 📊 SOLID
* 🌱 EMERGING

---

## ⚙️ Data Engineering & Tools

### 🛠 Tools Used

* Microsoft Excel
* Power Query (M-Code)
* Pivot Tables
* Conditional Formatting

### 🔄 ETL Process

* Extract → Multi-source data ingestion
* Transform → Cleaning, normalization, feature engineering
* Load → Master Table (single source of truth)

---

## 📊 Dashboard Structure

### 🏠 Overview

* KPI Summary
* Sales & Streaming trends

### 🔥 Fan Engagement

* FES Analysis
* Tier Distribution

### 🎧 Streaming

* Platform comparison
* Stream-to-sale ratio

### 🌍 Global vs Korea

* Export Index
* Market segmentation

### 🏷 Label Analysis

* Market share
* Portfolio strength

---

## 📁 File Structure

```
Project_File.xlsx
│
├── MasterData        → Cleaned dataset
├── Artistwise        → Cleaned dataset(using masterdata reference)
├── Labelwise         → Cleaned dataset(using masterdata reference)
├── Dashboard         → Main overview dashboard
├── FanEngagement     → FES analysis dashboard
├── Streaming         → Platform insights dashboard
├── GlobalVsKorea     → Market segmentation dashboard
├── LabelAnalysis     → Label performance dashboard
├── Cals              → Pivot tables & calculations
```

## 🚀 Getting Started

To explore this project locally:

1. Download the Excel file from this repository
2. Open using Microsoft Excel (recommended version: 2019 or later)
3. Enable editing and data connections if prompted
4. Navigate through dashboard sheets using slicers and filters

---

## ⚙️ Requirements

* Microsoft Excel (2019 / 365 recommended)
* Basic understanding of Pivot Tables and slicers
* No additional installations required

---

## 📝 Notes

* All dashboards are fully interactive and connected to a central Master Table
* Updating the dataset automatically refreshes all visuals and KPIs
* FES calculation is based on normalized and weighted metrics
* Designed for analytical exploration, not real-time production use

---

## 📈 Key Insights

* 🌍 Global markets drive majority of growth
* 🎧 Streaming ≠ direct sales conversion
* 🏆 Performance concentrated among top artists
* 📊 Fan engagement is a stronger indicator than sales alone
* 🚀 Rising artists show highest future potential

---

## 🔍 Advanced Analytics Features

* Fan Engagement Score (FES)
* Export Index
* Stream-to-Sale Ratio
* Momentum Classification

---

## 🙌 Acknowledgements

* Dataset compiled for analytical purposes
* Inspired by real-world music industry trends
* Special recognition to global fan communities

---

## 👩‍💻 Author

**Mounika Beera**

---

## ⭐ Support

If you like this project, consider giving it a ⭐ on GitHub!
