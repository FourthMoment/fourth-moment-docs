# Overview

Fourth Moment provides a risk model generator for quantitative investors to use for their optimization process (TODO: add more uses?).
Users supply their own data to the risk model generator with whatever factors they want, and the risk model generator will produce risk models for each date of data provided.
The risk model generator offers the following key benefits:

- _Fast_: The risk model generator can handle huge input datasets very quickly. For 5,000 days of data, with 15 style factors,
100 industry factors, and 5,000 asset in the estimation universe per day, the risk model generator can finish producing
risk models in under 20 minutes on the user's own machine.
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

#### Factor exposure dataset
The factor dataset contains the all the factor and universe data necessary to run the model. Specifically, it must contain the following columns:
  - Style factor columns
  - Industry factor columns
  - Estimation universe column
    - This consists of either booleans or 0/1 numerical values, each one indicating whether or not that asset should be included in the estimation universe
  - Asset ID column
  - Date column
  - Coverage universe column (optional)

#### Return dataset
The return dataset contains all the return data necessary to build up a return covariance matrix that is used in tandem with the factor exposure dataset
to generate the final model. Specifically, it must contains the following columns:
  - Return column
  - Asset ID column (to use to join this dataset with the factor exposure dataset)
  - Date column

The return dataset can contain data that precedes the start of the factor exposure dataset's data, allowing it to build up data
so that the return covariance matrix has sufficient data from the very start of the factor data.

### Outputs

For each day of data specified in the factor exposure dataset, a directory is created, labelled with the date, and containing the following items:
- Factor-mimicking portfolio weight dataset
- Factor-mimicking portfolio covariance matrix dataset
- Specific risk dataset
- Specific returns dataset
- Returns dataset
- Adjusted exposures dataset

For a more in-depth look at what exactly these inputs and outputs consist of, and how they're given to the risk model
generator, continue on to the [Tutorial](Tutorial.md).