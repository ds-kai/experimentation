Geo Test in R 


install.packages('tidyverse',dependency=T)
install.packages(c('quantmod','ff','foreign','R.matlab'),dependency=T)
suppressPackageStartupMessages(library(tidyverse))
library(GeoLift)


raw_city_data <- read_csv('/Users/kai.cui/Documents/02_Work/021_Project/2025/acq-project/202503_snapchat/raw_city_data.csv', show_col_types=FALSE)

raw_city_data <- GeoDataRead(data = raw_city_data, date_id = 'act_date', location_id = 'city', Y_id='nts_gbp', X=c(), format = 'yyyy-mm-dd', summary = TRUE)


head(raw_city_data) 

GeoPlot(raw_city_data, Y_id = 'Y', time_id = 'time', location_id = 'location')

 MarketSelections <- GeoLiftMarketSelection(data = city_data1,
                                               treatment_periods = c(30,60),
                                               N = c(5,8),
                                               Y_id = "Y",
                                               location_id = "location",
                                               time_id = "time",
                                               effect_size = seq(0.2,0.5),
                                               lookback_window = 5,
                                               include_markets = c("london"),
                                               exclude_markets = c("liverpool"),
                                               holdout = c(0.5, 1),
                                               cpic = 7.50,
                                               alpha = 0.1,
                                               Correlations = TRUE,
                                               fixed_effects = TRUE,
                                               side_of_test = "two_sided")
                                               
                                               
                                               
                    
plot(MarketSelections, market_ID=1, print_summary = FALSE)
> market_id = 2
> market_row <- MarketSelections$BestMarkets %>% dplyr::filter(ID == market_id)
> treatment_locations <- stringr::str_split(market_row$location, ", ")[[1]]
> treatment_duration <- market_row$duration
> lookback_window <- 7
> power_data <- GeoLiftPower(
+   data = GeoTestData_PreTest,
+   locations = treatment_locations,
+   effect_size = seq(-0.25, 0.25, 0.01),
+   lookback_window = lookback_window,
+   treatment_periods = treatment_duration,
+   cpic = 7.5,
+   side_of_test = "two_sided"
+ )
Setting up cluster.
Importing functions into cluster.
Calculating Power for the following treatment group: newcastle.
> power_data <- GeoLiftPower(
+   data = raw_weekly_data,
+   locations = treatment_locations,
+   effect_size = seq(-0.25, 0.25, 0.01),
+   lookback_window = lookback_window,
+   treatment_periods = treatment_duration,
+   cpic = 7.5,
+   side_of_test = "two_sided"
+ )
Setting up cluster.
Importing functions into cluster.
Calculating Power for the following treatment group: newcastle.
> plot(power_data, show_mde = TRUE, smoothed_values = FALSE, breaks_x_axis = 5) +
+     labs(caption = unique(power_data$location))



