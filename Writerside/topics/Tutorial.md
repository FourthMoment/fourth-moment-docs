# Tutorial

## Introduction

In this section, we'll go through the end-to-end steps of how to use the risk model engine to generate risk
models for an example dataset.
The risk model engine is an executable file with a number of configurable parameters. These parameters are
set in a configuration file (TOML format) whose path is then passed to the executable file.

### Input datasets

For our factor exposure dataset, we'll use a Parquet file 
of data for 25 large-cap tech stocks and 25 large-cap industrial stocks for the year 2023. There are two
Z-scored style factors and two industry factors. We'll include everything in the
estimation universe by setting the `est_univ` column to all 1's. We'll use the inverse square root of the market cap
as the GLS regression weight column. Here's a sample of it:

```Generic
-        date asset_id  earnings_yield_12m  momentum_12m  INDUSTRIALS  TECHNOLOGY
0  2023-01-03     AAPL           -0.337086      5.532012          0.0         1.0
1  2023-01-03      ACN           -0.219205      0.265881          0.0         1.0
2  2023-01-03     ADBE           -0.448981     -0.846101          0.0         1.0
...
50 2023-01-04     AAPL           -0.204665      5.462584          0.0         1.0
51 2023-01-04      ACN           -0.253574      0.265670          0.0         1.0
52 2023-01-04     ADBE           -0.422477     -0.857809          0.0         1.0
...
```
[Download full dataframe](https://fm-public-files.s3.amazonaws.com/data.parquet)

For our return dataset, we'll have another Parquet file, this time with return data for all the assets in the factor exposure dataset, for the year 2023 as well as the preceding year 2022. Here's a sample of it:

```Generic
-         date asset_id    return
0   2022-01-03     AAPL  0.025004
1   2022-01-03      ACN -0.017706
2   2022-01-03     ADBE -0.004744
...
50  2022-01-04     AAPL -0.012692
51  2022-01-04      ACN -0.007146
52  2022-01-04     ADBE -0.018374
...
```

[Download full dataframe](https://fm-public-files.s3.amazonaws.com/return_data.parquet)

Note: although the datasets are small in this tutorial for the sake of simplicity, the risk model generator can handle
huge real-world datasets very efficiently.

### Setting up the configuration file

There are a number of parameters we need to specify. This covers things like which columns in the dataframe should
be used for the style factors, covariance matrix settings, and more.

Note: although there's a synopsis of each argument in the `# ...` comment preceding it, there's also a more in-depth explanation of all available
parameters [here](Api-documentation.md).

Configuration file:
```Generic
# Where to put the output risk models
output_dir = "/path/to/risk_models"
# Style factors in the dataframe
style_factor_column_names = ["earnings_yield_12m", "momentum_12m"]
# Industry factor columns in the dataframe
industry_factor_column_names = ["INDUSTRIALS", "TECHNOLOGY"]
# Name of the each asset's unique ID
asset_id_column_name = "asset_id"
# Date column in the factor dataframe. Data must be sorted by date
date_column_name = "date"
# Return column in the return dataframe.
return_column_name = "return"
# Weight column for values used for the FMP calculation. The lower this value for a stock, the larger its impact on the FMPs.
gls_regression_weight_column_name = "market_cap_inv_sqrt" 
# Estimation universe column in the factor
estimation_universe_column = "est_univ"
# Exposure shrinkage for adjusted style factors
exposure_shrinkage = 0.5
# Path to the factor exposure dataset, in the form of a dataframe file
data_file_path = "/path/to/data.parquet"
# Data file format
data_file_format = "parquet"
# Path to the return data file
return_data_file_path = "/path/to/return_data.parquet"
# Return data file format
return_data_file_format = "parquet" 
# (Optional) Cutoff date before which no risk models will be generated, even if data is present for them
start_date = "2022-01-01"
# (Optional) Cutoff date after which no risk models will be generated, even if data is present for them
end_date = "2022-12-31"
# Type of z-normalization to be applied to the style factors
z_normalization_method = "standard"

# Covariance matrix parameters

min_overlap = 20 # Minimum number of dates that two assets' returns must both have overlapping return data before the covariance for that pair is included in the covariance matrix
min_obs = 252 # Minimum number of dates that an asset must have return data before its included for as part of any pairs in the covariance matrix
window_size = 1260 # Maximum number of dates to include return data for when computing a date's covariance matrix
variance_halflife = 126 # Variance halflife for the covariance matrix
correlation_halflife = 252 # Variance halflife for the correlation matrix
```

With this configuration file stored at `/path/to/config.toml`, we'll run the whole thing with `/path/to/fm --config_file /path/to/config.toml`.

### Output

We'll then invoke the tool with `/path/to/fm --config_file_path /path/to/config.toml`, and end up with the following files:
```Generic
- /path/to/output_dir
  - /risk_model_2023-01-03_00-00-00
    - fmp_portfolio_weights.parquet
    - fmp_covariance_matrix.parquet
    - ...
  - /risk_model_2023-01-04_00-00-00
    - fmp_portfolio_weights.parquet
    - fmp_covariance_matrix.parquet
    - ...
```



We can see that each day gets its own directory with each of the outputs having its own file. Here, for example is the FMP covariance matrix
for 2023-01-03:

```Generic
-       date          __factor__  earnings_yield_12m  momentum_12m  INDUSTRIALS  TECHNOLOGY
0 2023-01-03         INDUSTRIALS           -0.000018     -0.000068     0.000242    0.000254
1 2023-01-03          TECHNOLOGY           -0.000018     -0.000081     0.000254    0.000343
2 2023-01-03  earnings_yield_12m            0.000014      0.000011    -0.000018   -0.000018
3 2023-01-03        momentum_12m            0.000011      0.000045    -0.000068   -0.000081
```

These output models are now ready to be plugged into an optimizer, analyzed on their own, or whatever else.