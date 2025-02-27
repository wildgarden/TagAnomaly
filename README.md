# taganomaly
Anomaly detection labeling tool, specifically for multiple time series (one time series per category).

## Note: 
**This tool was built as a part of an [engagement](https://www.microsoft.com/developerblog/2019/01/02/real-time-time-series-analysis-at-scale-for-trending-topics-detection/), and is not maintained on a regular basis.**

Taganomaly is a tool for creating labeled data for anomaly detection models. It allows the labeler to select points on a time series, further inspect them by looking at the behavior of other times series at the same time range, or by looking at the raw data that created this time series (assuming that the time series is an aggregated metric, counting events per time range)

#### Click here to deploy on Azure using [Azure Container Instances](https://azure.microsoft.com/en-us/services/container-instances/):
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/omri374/taganomaly)

The app has four main windows:
### 1. The labeling window
![UI](https://github.com/Microsoft/taganomaly/raw/master/assets/ui.png)
###### Time series labeling
![Time series](https://github.com/Microsoft/taganomaly/raw/master/assets/ts.png)

###### Selected points table view
![Selected points](https://github.com/Microsoft/taganomaly/raw/master/assets/selected.png)

###### View raw data for window (if exists)
![Detailed data](https://github.com/Microsoft/taganomaly/raw/master/assets/detailed.png)


### 2. Compare this category with others over time
![Compare](https://github.com/Microsoft/taganomaly/raw/master/assets/compare.png)

### 3. Find proposed anomalies using the Twitter AnomalyDetection package
![Reference results](https://github.com/Microsoft/taganomaly/raw/master/assets/twitter.png)

### 4. Observe the changes in distribution between categories
This could be useful to understand whether an anomaly was univariate or multivariate
![Distribution comparison](https://github.com/Microsoft/taganomaly/raw/master/assets/dist.png)

## How to run locally:
This tool uses the [shiny framework](https://shiny.rstudio.com/) for visualizing events.
In order to run it, you need to have [R](https://mran.microsoft.com/download) and preferably [Rstudio](https://www.rstudio.com/products/rstudio/download/).
Once you have everything installed, open the project on R studio and click `Run App`, or call runApp() from the console. You might need to manually install the required packages

#### Requirements
- R (3.4.0 or above)
#### Used packages: 
- shiny
- dplyr
- gridExtra
- shinydashboard
- DT
- ggplot2
- shinythemes
- AnomalyDetection

## How to deploy using docker:
Option 1: [Deploy to Azure Web App for Containers or Azure Container Instances](https://azuredeploy.net/). More details [here (webapp)](https://azure.microsoft.com/en-us/services/app-service/containers/) and [here (container instances)](https://azure.microsoft.com/en-us/services/container-instances/)

Option 2: Deploy [this image](https://hub.docker.com/r/omri374/taganomaly/) to your own environment.

### Dockerize the shiny app:
In order to build a new Docker image, run the following commands from the root folder of the project:

```sh
sudo docker build -t taganomaly .
```

If you added new packages to your modified TagAnomaly version, make sure to specify these in the Dockerfile.

Once the docker image is built, run it by calling

```sh
docker run -p 3838:3838 taganomaly
```

Which would result in the shiny server app running on port 3838.


## Instructions of use
1. Import time series CSV file. Assumed structure:
- date (`"%Y-%m-%d %H:%M:%S"`). TagAnomaly will attempt to infer the date from other patterns as well, using the *parsedate* package
- category (optional)
- value

2. (Optional) Import raw data time series CSV file.

If the original time series is an aggreation over time windows, this time series is the raw values themselves. This way we could dive deeper into an anomalous value and see what it is comprised of.
Assumed structure:
- date (`"%Y-%m-%d %H:%M:%S"`). TagAnomaly will attempt to infer the date from other patterns as well, using the *parsedate* package
- category (optional)
- content

2. Select category (optional, if exists)

3. Select time range on slider

4. Select points on plot that look anomalous.
Optional (1): click on one time range on the table below the plot to see raw data on this time range
Optional (2): Open the `All Categories` tab to see how other time series behave on the same time range.
5. Once you decide that these are actual anomalies, save the resulting table to csv by clicking on `Download labels set` and continue to the next category.

#### Current limitations/issues
It is currently impossible to have multiple selections on one plot. A workaround is to select one area, download the csv and select the next area. Each downloaded CSV has a random string so files won't override each other. Once labeling is finished, one option is to run the provided [prep_labels.py](https://github.com/Microsoft/TagAnomaly/blob/master/prep_labels.py) file in order to concatenate all of TagAnomaly's output file to one CSV.
# Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
