# Command-Line Help for `fm`

This document contains the help content for the `fm` command-line program.

**Command Overview:**

* [`fm`↴](#fm)
* [`fm bootstrap`↴](#fm-bootstrap)
* [`fm generate`↴](#fm-generate)

## `fm`

**Usage:** `fm <COMMAND>`

###### **Subcommands:**

* `bootstrap` — Bootstrap a new workspace at the specified path with an example config.toml in place containing defaults and pointing at the data paths provided to this bootstrap command
* `generate` — Generate a risk model using the specified `config.toml` file as well as any overrides supplied at the command-line



## `fm bootstrap`

Bootstrap a new workspace at the specified path with an example config.toml in place containing defaults and pointing at the data paths provided to this bootstrap command

**Usage:** `fm bootstrap --workspace_path <WORKSPACE_PATH> --sample_data_file_path <SAMPLE_DATA_FILE_PATH> --sample_return_data_file_path <SAMPLE_RETURN_DATA_FILE_PATH>`

###### **Bootstrap Options:**

* `--workspace_path <WORKSPACE_PATH>` — The desired path and name in which to generate the new workspace. Type: `string`
* `--sample_data_file_path <SAMPLE_DATA_FILE_PATH>` — The path to the sample factor dataframe file distributed by FourthMoment alongside this binary. This path is used in the generated config.toml. Type: `string`
* `--sample_return_data_file_path <SAMPLE_RETURN_DATA_FILE_PATH>` — The path to the sample return dataframe file distributed by FourthMoment alongside this binary. This path is used in the generated config.toml. Type: `string`



## `fm generate`

Generate a risk model using the specified `config.toml` file as well as any overrides supplied at the command-line

**Usage:** `fm generate [OPTIONS] --config_file_path <CONFIG_FILE_PATH>`

###### **Generate Options:**

* `--config_file_path <CONFIG_FILE_PATH>` — The path to the .toml configuration file. This is the one required argument that must always be passed to the executable
* `--data_file_path <DATA_FILE_PATH>` — The path to the factor dataframe file. Type: string
* `--data_file_format <DATA_FILE_FORMAT>` — The format of the factor dataframe file. It can either be a Parquet file or an Arrow IPC (Feather v2) file. Type: string

  Possible values: `parquet`, `arrow-ipc`

* `--return_data_file_path <RETURN_DATA_FILE_PATH>` — The path to the return dataframe file
* `--return_data_file_format <RETURN_DATA_FILE_FORMAT>` — The format of the return dataframe file. It can either be a Parquet file or an Arrow IPC (Feather v2) file. Type: `string`

  Possible values: `parquet`, `arrow-ipc`

* `--estimation_universe_column <ESTIMATION_UNIVERSE_COLUMN>` — The name of the factor dataframe column that contains the estimation universe. Type: `string`
* `--expo_shrink <EXPO_SHRINK>` — The expo shrink for the adjusted style exposures. Type: `float`
* `--return_column <RETURN_COLUMN>` — The name of the return dataframe column that contains the returns. Type: `string`
* `--asset_id_column <ASSET_ID_COLUMN>` — The name of the column, both in the factor dataframe and in the return dataframe, that contains the asset IDs. Type: `string`
* `--added_market_factor_column <ADDED_MARKET_FACTOR_COLUMN>`
* `--date_column <DATE_COLUMN>` — The name of the column, both in the factor dataframe and in the return dataframe, that contains the dates. This column must be sorted in ascending order in both dataframes. Type: `string`
* `--weight_column <WEIGHT_COLUMN>` — The name of the factor dataframe column that contains the weights for the factor-mimicking portfolio calculation. Type: `string`
* `--factor_columns_industry <FACTOR_COLUMNS_INDUSTRY>` — The names of the factor dataframe's industry factor columns. Type: `string-array`
* `--factor_columns_style <FACTOR_COLUMNS_STYLE>` — The names of the factor dataframe's style factor columns. Type: `string-array`
* `--covariance_halflife <COVARIANCE_HALFLIFE>` — The covariance matrix's covariance half-life (must be >= 1). Type: `int`
* `--correlation_halflife <CORRELATION_HALFLIFE>` — The covariance matrix's correlation half-life, (must be >= 1). Type: `int`
* `--window_size <WINDOW_SIZE>` — (Optional) The covariance matrix's window size (must be >= 2). If not specified, defaults to not having any window. Type: `int`
* `--min_overlap <MIN_OVERLAP>` — The minimum overlap between two assets to include their covariance in the covariance matrix. If the minimum overlap isn't reached for a pair of assets, their covariance is set to NaN. Type: `int`
* `--min_obs <MIN_OBS>` — (Optional) The minimum number of returns an asset must have for any of their covariances to be included in the covariance matrix. If the overlap for an asset, its covariances are set to NaN. Type: `int`
* `--start_date <START_DATE>` — (Optional) A threshold date before which all dates in the factor dataframe are ignored. Does not affect the covariance matrix calculations. Type: A date represented as a string (TOML's date type is not supported). It can either be a RFC 3339 date with a UTC timestamp (e.g. `"2020-01-20T03:05:10Z"`) or a date in the format `YYYY-MM-DD` (e.g. `"2020-01-20"`, which is equivalent to `"2020-0120T00:00:00Z"`)
* `--end_date <END_DATE>` — (Optional) A threshold date after which all dates in the factor dataframe are ignored. Does not affect the covariance matrix calculations. Type: A date represented as a string (TOML's date type is not supported). It can either be a RFC 3339 date with a UTC timestamp (e.g. `"2020-01-20T03:05:10Z"`) or a date in the format `YYYY-MM-DD` (e.g. `"2020-01-20"`, which is equivalent to `"2020-0120T00:00:00Z"`)
* `--output_dir <OUTPUT_DIR>` — The directory to which the generated models will be written. This directory must not exist beforehand, as it will be created by the tool. Type: `string`
* `--z_normalization_method <Z_NORMALIZATION_METHOD>` — The Z-Normalization method to be used. Type: `string`

  Possible values: `rank`, `standard`, `none`

* `--warn_only <WARN_ONLY>` — Treat errors as warnings for the specified issues detected during model generation. Type: `string-array`

  Possible values: `covmat_asset_dropped`, `factor_scalar_nan`, `factor_scalar_inf`, `matrix_singular`




<hr/>


