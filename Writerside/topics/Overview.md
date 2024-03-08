# Overview

### Inputs
At a high-level, a risk model needs a few key inputs that the customer provides:

#### Factor dataframe
The factor dataframe contains the all the factor and universe data necessary to run the model. Specifically, it contains the following columns:
  - Style factor columns
  - Industry factor columns
  - Estimation universe column
    - This consists of either booleans or 0/1 numerical values, each one indicating whether or not that asset should be included in the estimation universe
  - Asset ID column
  - Date column

This factor dataframe can come from any data source, can be an aggregate of multiple data sources, etc., it just needs
to be given to the risk model as one single dataframe.

#### Return dataframe
The return dataframe contains all the return data necessary to build up a return covariance matrix that is used in tandem with the factor dataframe
to generate the final model. Specifically, it contains the following columns:
  - Return column
  - Asset ID column (to use to join this dataframe with the factor dataframe)
  - Date column

The returns dataframe can contain data that precedes the start of the factor dataframe's data, allowing it to build up data
so that the return covariance matrix has sufficient data from the very start of the factor data.

### Outputs

For each day of data specified in the factor dataframe, a directory is created, labelled with the date, and containing the following items:
- Factor-mimicking portfolio weight dataframe
- Factor-mimicking portfolio covariance dataframe
- Specific risk dataframe
- Specific returns dataframe
- Returns dataframe
- Adjusted exposures dataframe

### Example

Suppose we want to generate risk models for the following factor dataframe:
```Generic
-     date asset_id  momentum_21_day    adv_21_day  TECH  INDUSTRIAL
2020-01-02     AAPL         0.099018  1.005026e+08     1           0
2020-01-02       GE         0.022469  1.216584e+08     0           1
2020-01-03     AAPL         0.038527  1.559063e+08     1           0
2020-01-03       GE         0.094212  1.351560e+08     0           1
```

And the following return dataframe:
```Generic
-     date asset_id    return
2020-01-02     AAPL  0.017174
2020-01-02       GE  0.038341
2020-01-03     AAPL  0.068160
2020-01-03       GE  0.080969
```

We then write the factor dataframe to `/path/to/factor_dataframe.parquet`, the return dataframe to `/path/to/return_dataframe.parquet`,
and a config file to `/path/to/config.toml` consisting of the following:
```Generic
data_file_path = "/path/to/factor_dataframe.parquet"
return_data_file_path = "/path/to/return_dataframe.parquet"
expo_shrink = 0.5
output_dir = "/path/to/output_dir"
...
```

We then invoke the tool with `/path/to/fm --config_file_path /path/to/config.toml`, and end up with the following files:
```Generic
- /path/to/output_dir
  - /2020-01-02T00:00:00Z
    - fmp_portfolio_weights.parquet
    - fmp_covariance_matrix.parquet
    - ...
  - /2021-01-03T00:00:00Z
    - fmp_portfolio_weights.parquet
    - fmp_covariance_matrix.parquet
    - ...
```

As one example, the `fmp_portfolio_weights.parquet` files above will look like this:

```Generic
-     date momentum_21_day  adv_21_day      TECH  INDUSTRIAL
2020-01-02        0.076388    0.046388  0.038853    0.008082
2020-01-03        0.056049    0.003916  0.098135    0.004257
```

We can see that each date for which we have data gets its own directory with its own dataframes. Although the amount data in this example is quite small,
the risk model tool can generate risk models for input data with hundreds of factors, thousands of days, and thousands of assets per day in
less than 20 minutes (if not less than 5) depending on the hardware used and the exact size of the data.

For more information on how to specify parameters for the tool and run it, see "Running the Risk Model Tool".
