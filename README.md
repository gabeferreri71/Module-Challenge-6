# Module-Challenge-6: Housing Rental Analysis fo San Francisco

## Import the Data

1. Our included imports are pandas as pd, hvplot.pandas for plotting, and Path from patlib for reading .csv files. We first want to import the information from sfo_neighborhoods_census_data into a data frame sfo_data_df using the pandas read_csv function. Within the function, we call the path to the file, index columns with 'year', parse dates, and infer the datetime index with the following:

pd.read_csv(
   
   Path("./Resources/sfo_neighborhoods_census_data.csv"),
   
   index_col = 'year',
   
   parse_dates = True,
   
   infer_datetime_format = True) 
   
   We then review the data with the .head() and .tail() function. Note, we are given years in this case, so when we create the dataframe, the dates and months were automatically set to 201*-01-01. 
   
## Calculate and Plot the Housing Units per Year

1. With our created dataframe, we will now group the data by year and average out the price-per-sq.-foot by year. To do this, we create variable housing_units_by_year and assign it to sfo_data_df.groupby('year')['sale_price_sqr_foot'].mean() where from our sfo_data_df, we use the groupby() function to 'group' by years, then apply the mean() function to average out the sale_price_sqr_foot by year. We then preview the data with the head() function. 
