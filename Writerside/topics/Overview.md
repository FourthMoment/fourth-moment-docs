# Overview

Fourth Moment provides a risk model generator for quantitative investors to use for their optimization process (TODO: add more uses?).
Users supply their own data to the risk model generator with whatever factors they want, and the risk model generator will produce risk models for each date of data provided.
The risk model generator offers the following key benefits:

- _Fast_: The risk model generator can handle huge input datasets very quickly. For 5,000 days of data, with 15 style factors,
100 industry factors, and 5,000 asset rows per day, the risk model generator can finish producing risk models in under 20 minutes.
- _Flexible_: Users can adjust a wide array of parameters beyond just the data itself. From exposure shrinkage adjustment,
to covariance matrix halflifes, to custom GLS regression asset weights, there are a wide array of parameters one can adjust
for their own use case.
- _Trusted_: Fourth Moment includes high-level analytics results with each risk model, giving insights about the quality
of the risk model and highlighting any potential issues. There's also a wealth of knowledge and open-source analytics
tools to go even deeper.

With these benefits, users no longer have to rely on one-size-fits-all risk model providers. Instead, they can get a far better
risk model for their own needs in no time at all.

### Inputs
At a high-level, a risk model needs a few key inputs that the customer provides:

#### Factor dataframe
The factor dataframe contains the all the factor and universe data necessary to run the model. Specifically, it must contain the following columns:
  - Style factor columns
  - Industry factor columns
  - Estimation universe column
    - This consists of either booleans or 0/1 numerical values, each one indicating whether or not that asset should be included in the estimation universe
  - Asset ID column
  - Date column
  - (Optional) coverage universe column

This factor dataframe can come from any data source, can be an aggregate of multiple data sources, etc., but it just needs
to be given to the risk model as one single dataframe.

#### Return dataframe
The return dataframe contains all the return data necessary to build up a return covariance matrix that is used in tandem with the factor dataframe
to generate the final model. Specifically, it must contains the following columns:
  - Return column
  - Asset ID column (to use to join this dataframe with the factor dataframe)
  - Date column

The returns dataframe can contain data that precedes the start of the factor dataframe's data, allowing it to build up data
so that the return covariance matrix has sufficient data from the very start of the factor data.

### Outputs

For each day of data specified in the factor dataframe, a directory is created, labelled with the date, and containing the following items:
- Factor-mimicking portfolio weight dataframe
- Factor-mimicking portfolio covariance matrix dataframe
- Specific risk dataframe
- Specific returns dataframe
- Returns dataframe
- Adjusted exposures dataframe

For a more in-depth look at what exactly these inputs and outputs consist of, and how they're given to the risk model
generator, continue on to the [Tutorial](Tutorial.md).