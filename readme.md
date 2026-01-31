## Overview

The notebook in this repository (pandas_vs_duckdb.ipynb) compares performance across several real-world data operations, measuring execution time for each approach.

## Dataset

The dataset (artists.parquet) contains Spotify artist information with the following columns:

| Column | Description |
|--------|-------------|
| `id` | Spotify artist ID |
| `fetched_at` | Timestamp (milliseconds since epoch) |
| `name` | Artist name |
| `followers_total` | Total follower count |
| `popularity` | Popularity score (0-100) |

## Benchmarks Performed

### 1. Reading Data

- Parquet file loading with different engines
- DuckDB lazy evaluation vs Pandas eager loading

### 2. Filtering

- Applying WHERE conditions on timestamps and numeric columns
- Query-based filtering vs boolean indexing

### 3. Deduplication (Latest Record)

- Finding the most recent record per artist using window functions
- ROW_NUMBER() in DuckDB vs sort + drop_duplicates in Pandas

### 4. Daily Aggregations

- Grouping by date with multiple aggregate functions
- Computing averages, medians (p50), and p90 percentiles

### 5. Top N Query

- Retrieving the top 100 most followed artists
- Combining window functions with sorting and limits

### 6. Data Quality Checks

- Counting rows and distinct IDs
- Detecting missing names, negative followers, and out-of-range popularity values

### 7. Writing Parquet Files

- Comparing different compression codecs (ZSTD, LZ4, Brotli, GZIP)
- File size comparison across formats

|                                | Size (MB)  |
|------------------------------: |----------: |
|          artists.csv           | 925.5315   |
| artists.parquet (Source File)  | 615.9072   |
|      artists_out.parquet       | 410.0665   |
|   artists_out_brotli.parquet   | 364.7962   |
|   artists_out_duckdb.parquet   | 410.0665   |
|    artists_out_gzip.parquet    | 421.6951   |
|    artists_out_lz4.parquet     | 635.1817   |
|   artists_out_pandas.parquet   | 435.7401   |
| artists_out_pandas_fp.parquet  | 409.5974   |

## Requirements

```
pandas==2.3.3
duckdb==1.4.3
pyarrow==21.0.0
fastparquet==2025.12.0
```

## Key Findings

DuckDB generally outperforms Pandas for:

- Large file reads with filtering (pushdown optimization)
- Complex window function operations
- Aggregations with multiple statistics

Pandas works well for:

- Simple transformations and quick prototyping
