# Config.toml Reference

##### config_file_path
- **Description**: The path to the .toml configuration file. This is the one required argument that must always be passed as a command-line parameter (`--config_file_path ...`)

##### data_file_path
- **Description**: The path to the factor dataframe file

##### data_file_format
- **Description**: The format of the factor dataframe file.
- **Supported Values**:
  - `parquet`: A Parquet file
  - `arrow_ipc`: An Arrow IPC (A.K.A. Feather v2) file

##### return_data_file_path
- **Description**: The path to the return dataframe file

##### return_data_file_format
- **Description**: The format of the return dataframe file.
- **Supported Values**:
  - `parquet`: A Parquet file
  - `arrow_ipc`: An Arrow IPC (A.K.A. Feather v2) file

##### estimation_universe_column
- **Description**: The name of the factor dataframe column that contains the estimation universe

##### expo_shrink
- **Description**: The expo shrink for the adjusted style exposures
- **Type**: Float, e.g. `1.0`

##### return_column
- **Description**: The name of the return dataframe column that contains the returns

##### asset_id_column
- **Description**: The name of the column, both in the factor dataframe and in the return dataframe, that contains the asset IDs

##### added_market_factor_column (Optional)
- **Description**: A market factor column (all 1's) that gets added as a style factor. Clients should not supply their own
- **Default if not specified**: No market factor is added

##### date_column
- **Description**: The name of the column, both in the factor dataframe and in the return dataframe, that contains the dates. This column must be sorted in ascending order in both dataframes

##### weight_column
- **Description**: The name of the factor dataframe column that contains the weights for the factor-mimicking portfolio calculation

##### factor_columns_industry
- **Description**: The names of the factor dataframe's industry factor columns
- **Type**: String array, e.g. `["a", "b", "c"]`

##### factor_columns_style
- **Description**: The names of the factor dataframe's style factor columns
- **Type**: String array, e.g. `["a", "b", "c"]`

##### covariance_halflife
- **Description**: The covariance matrix's covariance half-life (must be >= 1)
- **Type**: Integer, e.g. `5`

##### correlation_halflife
- **Description**: The covariance matrix's correlation half-life, (must be >= 1)
- **Type**: Integer, e.g. `5`

##### window_size (Optional)
- **Description**: The covariance matrix's window size (must be >= 2). If not specified, defaults to not having any window
- **Type**: Integer, e.g. `5`
- **Default if not specified**: The window size is infinite, i.e. no data is cut off

##### min_overlap
- **Description**: The minimum overlap between two assets to include their covariance in the covariance matrix. If the minimum overlap isn't reached for a pair of assets, their covariance is set to NaN
- **Type**: Integer, e.g. `5`

##### min_obs
- **Description**: The minimum number of returns an asset must have for any of their covariances to be included in the covariance matrix. If the overlap for an asset, its covariances are set to NaN
- **Type**: Integer, e.g. `5`

##### start_date (Optional)
- **Description**: A threshold date before which all dates in the factor dataframe are ignored. Does not affect the covariance matrix calculation
- **Type**: A date represented as a string (TOML's date type is not supported). It can either be a RFC 3339 date with a UTC timestamp (e.g. `"2020-01-20T03:05:10Z"`) or a date in the format `YYYY-MM-DD` (e.g. `"2020-01-20"`, which is equivalent to `"2020-0120T00:00:00Z"`).
- **Default if not specified**: The earliest date in the factor dataframe, i.e. no start date cutoff

##### end_date (Optional)
- **Description**: A threshold date after which all dates in the factor dataframe are ignored. Does not affect the covariance matrix calculation
- **Type**: A date represented as a string (TOML's date type is not supported). It can either be a RFC 3339 date with a UTC timestamp (e.g. `"2020-01-20T03:05:10Z"`) or a date in the format `YYYY-MM-DD` (e.g. `"2020-01-20"`, which is equivalent to `"2020-0120T00:00:00Z"`).
- **Default if not specified**: The latest date in the factor dataframe, i.e. no end date cutoff

##### output_dir
- **Description**: The directory to which the generated models will be written. This directory must not exist beforehand, as it will be created by the tool

##### z_normalization_method
- **Description**: The Z-Normalization method to be used
- **Supported Values**:
  - `standard`: Apply standard normalization, z-scoring each piece of data
  - `rank`: Apply standard normalization but on the _rank_ of each piece of data (the lowest value gets assigned 1, the second lowest gets assigned 2, etc.)
  - `none`: Don't perform any normalization on the data, e.g. because it has already been normalized
