# About the Dataset

[Kaggle Link](https://www.kaggle.com/datasets/jeanmidev/smart-meters-in-london/data)

## Context

To better follow the energy consumption, the government wants energy suppliers to install smart meters in every home in England, Wales and Scotland. There are more than 26 million homes for the energy suppliers to get to, with the goal of every home having a smart meter by 2020.

This roll out of meter is lead by the European Union who asked all member governments to look at smart meters as part of measures to upgrade our energy supply and tackle climate change. After an initial study, the British government decided to adopt smart meters as part of their plan to update our ageing energy system.

In this dataset, you will find a refactorised version of the data from the London data store, that contains the energy consumption readings for a sample of 5,567 London Households that took part in the UK Power Networks led Low Carbon London project between November 2011 and February 2014. The data from the smart meters seems associated only to the electrical consumption.

There is infomations on the ACORN classification details that you can find in this report or the website of CACI.

I added weather data for London area, I used the darksky api to collect this data.

## Content

There are 19 files in this dataset:

- `informations_households.csv`: This file contains all the information on the households in the panel, including their ACORN group, tariff, and the `block.csv.gz` file where their data are stored.

- `halfhourly_dataset.zip`: A ZIP file containing the block files with half-hourly smart meter measurements.

- `daily_dataset.zip`: A ZIP file containing the block files with daily information, such as the number of measurements, minimum, maximum, mean, median, sum, and standard deviation.

- `acorn_details.csv`: Details about the ACORN groups and the profiles of people in each group. This information comes from an [XLSX spreadsheet](https://acorn.caci.co.uk/what-is-acorn/). The first three columns contain the attributes studied, and `ACORN-X` is the index for the attribute. At a national scale, the index is 100. If the value for a column is 150, it means that there are 1.5 times more people with this attribute in the ACORN group than at the national scale. More information is available on the [CACI website](https://www.caci.co.uk/).

- `weather_daily_darksky.csv`: Daily weather data collected from the [Dark Sky API](https://support.apple.com/en-us/102594). More information about the parameters is available in the [API documentation](https://support.apple.com/en-us/102594#response-format).

- `weather_hourly_darksky.csv`: Hourly weather data collected from the [Dark Sky API](https://support.apple.com/en-us/102594). More information about the parameters is available in the [API documentation](https://support.apple.com/en-us/102594#response-format).

## Acknowledgements

All the big work of data collection has been done by the UK power networks for the smart meter data.

The details related at the acorn group are provided by the CACI.

The weather data are from darksky.