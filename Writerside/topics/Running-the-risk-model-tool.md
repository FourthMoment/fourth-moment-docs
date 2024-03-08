# Running the Risk Model Tool

## Introduction

The risk model generation tool is an executable file with a number of configurable parameters. These parameters are
set in a configuration file whose path is passed to the executable file.

### Setting up the configuration file

The configuration file uses the [TOML](https://toml.io) markup language.

Example configuration file:
```Generic
data_file_path = "/path/to/dataframe.parquet"
output_dir = "/path/to/output_dir"
expo_shrink = 0.5
covariance_halflife = 20
...
```

If this example configuration file resided at `/path/to/config.toml`, we'd run the whole thing with `/path/to/fm --config_file /path/to/config.toml`.

### Overriding parameters via command-line arguments

All configuration file parameters can be overridden by corresponding command-line flags with the same names. For example,
if we wanted to override the `expo_shrink` value that we specified in the configuration data above to instead be 0.75, we could do that by
adding `--expo-shrink 0.75` to the invocation: `/path/to/fm --config_file /path/to/config.toml --expo_shrink 0.75`.
Note that array parameters, such as `factor_columns_industry`, are represented in comma-separated form, e.g.
`--factor_columns_industry some_factor,other_factor,yet_another_factor`.

For a complete list of the specific arguments that the tool takes as an input, see "API Documentation".
