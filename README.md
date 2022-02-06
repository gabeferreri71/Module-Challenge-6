# Module-Challenge-6: Housing Rental Analysis fo San Francisco

## Import the Data

Note: I completed this in my dev enviroment, where all required installations for Module 6 activites in the 'Getting Started' were installed.

1. Our included imports are pandas as pd, hvplot.pandas for plotting, and Path from patlib for reading .csv files. We first want to import the information from sfo_neighborhoods_census_data into a data frame sfo_data_df using the pandas read_csv function. Within the function, we call the path to the file, and index columns with 'year' with the following:

pd.read_csv(
   
   Path("./Resources/sfo_neighborhoods_census_data.csv"),
   
   index_col = 'year') 
   
   We then review the data with the .head() and .tail() function. Note, we are given years in this case, so we do not need to parse_dates and infer_datetime_index. 
   
## Calculate and Plot the Housing Units per Year

1. With our created dataframe, we will now group the data by year. To do this, we create variable housing_units_by_year and assign it to sfo_data_df.groupby('year').mean() where from our sfo_data_df, we use the groupby() function to 'group' by years, then apply the mean() function to average out the data within the dataframe. We then preview the data with the head(7) function, to see that all the years' data was grouped properly. 

2 - 4. To plot a bar chart, we first make a variable, in this case huby_plot, and assign it to our grouped data using the .hvplot() function. Within the function contains x, y, title, height, kind, and ylim variables for formatting parameters. After getting exponential values in my y-axis, I added .opts(yformatter = '%.0f') to the end to correct the format, with the code line as:

huby_plot = housing_units_by_year.hvplot(x="year", y="housing_units", title= "Housing Units in San Francisco from 2010 - 2016", height = 300, kind = 'bar', ylim =(365000,385000)).opts(yformatter= '%.0f')

Note that the x and y variables are from the grouped housing_units_per_year dataframe and are what we need for this bar chart.

5. Please refer to the answer in the provided notebook answer section. 

## Calculate and Plot the Average Sales Prices per Square Foot

1. Similar to before, we created a prices_per_square_foot_by_year variable, and assigned it to sfo_data_df.groupby('year').mean() to group the data by year and average the results. We can review the output with .head(7) like before or just entering prices_square_foot_by_year.

For answer portion, please refer to the provided notebook answer section.

2. We will edit our grouped data and drop the housing units column by taking prices_square_foot_by_year and assigning it to itself with the .drop() function with the following code: 

prices_square_foot_by_year = prices_square_foot_by_year.drop('housing_units', axis =1)

Note, 'housing_units' is the column we want to drop, and '1' is the axis position/location to drop said column. We then can review prices_square_foot_by_year to make sure the change occured. 

3 - 5. What's left in prices_square_foot_by_year is sale_price_sqr_foot and gross_rent, which we want to plot with a line chart. To do this, we take prices_square_foot_by_year and apply .hvplot.line(), with the internal variables for formatting and presentation as shown:

prices_square_foot_by_year.hvplot.line(x = 'year', ylabel = 'Gross Rent / Sale Price per Square Foot ($)', ylim = (0,4750), title = 'Average Gross Rent and Sale Price Per Square Foot: San Francisco 2010 - 2016')

Note the only column of data that is a variable in the plot code is 'year' on the x-axis, the rest of the variables are for titles and visual purposes. 

6. Please refer to the answer in the provided notebook answer section.

## Compare the Average Sale Prices by Neighborhood

1. Similar to before, we will use the groupby() function on sfo_data_df with mean() applied at the end, except this time, we will also include 'neighborhood' as shown:

prices_by_year_by_neighborhood = sfo_data_df.groupby(['year', 'neighborhood']).mean()

And then review prices_by_year_by_neighborhood. Note, 'year' and 'neighborhood' should have no indentation above the values if formatted correctly. 

2. Similar to before, we will assign prices_by_year_by_neighborhood to itself with .drop('housing_units', axis = 1) added to remove the housing units column. We use .head() and .tail() to review both ends of the dataframe.

3 - 5. We will enter the exact same thing to plot prices_by_year_by_neighborhood as prices_square_foot_by_year, except this time within .line(), we will add a groupby variable and assign it to 'neighborhood' as shown as:

.hvplot.line(x = 'year', ylabel = 'Gross Rent / Sale Price per Square Foot ($)', ylim = (0,4750), title = 'Average Gross Rent and Sale Price Per Square Foot: San Francisco 2010 - 2016', groupby = 'neighborhood')

This creates the drop-down widget to analyze individual neighborhoods. 

6. Please refer to the answer in the provided notebook answer section.

## Build an Interactive Neighborhood Map

1. We need to read the neighborhoods coordinates from the provided .csv file and review the data, which is done through the following:

neighborhood_locations_df = pd.read_csv(
   
   Path("./Resources/neighborhoods_coordinates.csv"),
   
   index_col = 'Neighborhood') 

neighborhood_locations_df.head()

2. Taking the original sfo_data_df, we want to only group by neighborhood this time and review the data, which can be done with:

all_neighborhood_info_df = sfo_data_df.groupby(['neighborhood']).mean()

all_neighborhood_info_df.head()

all_neighborhood_info_df.tail()

3. Note: the following code sections were provided and not edited. To combine both dataframes (location and price data), we use the .concat() function with Pandas to combine each neighborhood's location with its corresponding price data. To do this and review the results, the following code was provided:

all_neighborhoods_df = pd.concat(

    [neighborhood_locations_df, all_neighborhood_info_df], 
    
    axis="columns",
    
    sort=False
)


display(all_neighborhoods_df.head())

display(all_neighborhoods_df.tail())

Now we want to clean up the data. We first use the combination of .reset_index().dropna() to remove neighborhoods lacking data. Our data now is almost ready, except the index column with the neighborhoods has no name. With the following code, the index column is renamed to 'Neighborhood' and then reviewed:

all_neighborhoods_df = all_neighborhoods_df.reset_index().dropna()

all_neighborhoods_df = all_neighborhoods_df.rename(columns={"index": "Neighborhood"})

display(all_neighborhoods_df.head())

display(all_neighborhoods_df.tail())

4. Now our all_neighborhoods_df is ready for plotting. Using hvplot.points(), we include the locations through 'Lon' and 'Lat', set geo=True, set the size to sale_price_sqr_foot, set the color to gross_rent, set titles='OSM', set alpha=0.6 for transparency, long with the frame_width and frame_height set to 700 and 500, respectively. A plot similar to the example provided should be generated. 

5. Please refer to the answer in the provided notebook answer section.

## Compose Your Data Story

Please refer to the answer in the provided notebook answer section.
