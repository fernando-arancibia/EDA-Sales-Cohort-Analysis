# EDA Sales Cohort Analysis

#### This repository contains a Jupyter Notebook that performs an analysis of sales data for a year, using Python libraries such as pandas for data analysis and plotly for data visualization.

The analysis is divided into two main stages:

## Exploratory Data Analysis and Data Cleaning

During this stage, the data types are explored, and it is found that most of the data is of type object. To facilitate data analysis, the data types are changed in subsequent stages.

The dataset contains negative values in the 'units' and 'amounts' columns, as well as null values. These values are associated with alphanumeric invoices in the 'invoice' column and correspond to returns or rejected transactions. To ensure data accuracy, these entries are removed from the dataset.

To remove the values from the dataframe, an index is created for rows where the 'units' column has values less than 0. The corresponding rows are then dropped from the dataframe.

![Screenshot 2022-05-04 175548](https://user-images.githubusercontent.com/84011018/166721930-7671c6b8-b4d3-4201-b07b-fea6e3949d2a.png)

#### 

```python
# Create index from 'units' columns with values < 0
index = df[df['units'] < 0 ].index
# drop the index with negative values 
df.drop(index , inplace=True)
df
```

#### After that the index is drop from the data frame and have the clean dataframe.

![image](https://user-images.githubusercontent.com/84011018/166724686-a693b50a-ff9a-471a-89e4-9626a31028ae.png)

#### To further prepare the data for grouping and visualizations, the format of the date column is changed to datetime.

```python
# Format date time and create new time serie for visualizations
sales['date'] = pd.to_datetime(sales['date']).dt.date
sales['date'] = pd.to_datetime(sales.date, format='%Y-%m-%d %H:%M:%S')
sales['order_month'] = sales['date'].dt.to_period('M')
sales['order_month'] = sales['order_month'].dt.to_timestamp('s').dt.strftime('%Y-%m')
```

##Cohort Analysis

#### The second stage focuses on cohort analysis, specifically customer retention within a specific period. This analysis is crucial for companies and their marketing strategies.

### Invoices by month

#### The analysis begins by grouping the data by month and visualizing the number of invoices per month. The line graph indicates a significant number of invoices in November, which may correspond to the seasonality of purchases for the Christmas party. Following that, a pronounced decrease in the number of invoices is observed.

![image](https://user-images.githubusercontent.com/84011018/166726310-345afa3a-7196-47eb-a1e4-756b17e42ee2.png)

### Uniques clients by month

#### Similarly, the unique number of customers per month is plotted, showing a similar trend to the invoices per month graph. A higher volume of invoices corresponds to a larger number of customers making purchases in a given month.

![image](https://user-images.githubusercontent.com/84011018/166726581-63c50066-d6fe-49b1-a023-d69bf11fadbc.png)

### Top countries by invoice

#### The top countries generating invoices are identified, with the UK, Germany, France, and Eire leading the list. The UK stands out as the country with the highest number of invoices.

![image](https://user-images.githubusercontent.com/84011018/166726721-e1469464-f595-496d-b74f-895e545700f7.png)

### Percentage of invoices by countries

#### The top countries generating invoices are identified, with the UK, Germany, France, and Eire leading the list. The UK stands out as the country with the highest number of invoices.

Converting the invoice data to percentages reveals that the UK accounts for 89% of the total invoices generated in 2021.

![image](https://user-images.githubusercontent.com/84011018/166726969-9c29e8cd-8bd1-4a13-98fe-6f41d0c89ff6.png)

### Total amount by countries

#### The top countries generating invoices are identified, with the UK, Germany, France, and Eire leading the list. The UK stands out as the country with the highest number of invoices.

#### Converting the invoice data to percentages reveals that the UK accounts for 89% of the total invoices generated in 2021.

#### The total amount of sales is analyzed by country, confirming the UK as the top performer in terms of generated revenue during the 2021 period.

![image](https://user-images.githubusercontent.com/84011018/166727239-18b79a1f-a646-48c4-a06b-a619050f01bb.png)

### Cohort analyst

#### Finally, the cohort analysis is conducted. This method examines how many customers remain active during a specified period, providing insights into the effectiveness of business strategies in customer retention.

![image](https://user-images.githubusercontent.com/84011018/166727694-e546fa4a-29bf-4e5c-a995-2a62f5fa04ee.png)

#### Analyzing the data, it is observed that there are no negative values in the 'units' column, and both the 'invoice' and 'id_client' columns have no null values. Additionally, the maximum number of invoices is greater than the number of clients, indicating potential customer retention.

#### Grouping the data by 'client_id' and 'invoice' numbers and filtering for clients with more than one invoice, it is determined that 67% of clients placed multiple orders.

#### A histogram is plotted to visualize the distribution of invoice frequencies among customers. The majority of customers (approximately 3900) have invoice frequencies ranging from 0 to 9. The number of clients decreases for higher invoice frequencies, with only a few exceptions in the 50 to 200 invoice range.

![image](https://user-images.githubusercontent.com/84011018/166728662-1b626657-64f7-4508-a192-827c2d2a5e9f.png)

### Cohort variables

#### The next step involves creating variables for the cohort analysis. Two columns are generated: one for each month of the year and the other for the date on which the customer made their first purchase.

```python
df['order_m'] = df['date'].dt.to_period('M')
df['cohort'] = df.groupby('id_Client')['date'] \.transform('min') \.dt.to_period('M') 
```

#### The data is then grouped, and a new dataframe is created to display the number of clients generated per month.

![image](https://user-images.githubusercontent.com/84011018/166729335-4a871490-fa1c-4bcf-80d9-48d8601fd7f9.png)

After this step, a pivot table is generated to relate the cohort to a given period.

![image](https://user-images.githubusercontent.com/84011018/166729640-0ce49e71-034f-4438-80c3-4d7ae5d36e22.png)

### Retention matrix analyst

#### Using the generated pivot table, a retention matrix is constructed by dividing each row by the cohort size (i.e., the total number of customers for their first purchase). The retention matrix reveals that the 2020-12 cohort has higher user retention compared to subsequent cohorts. 

Specifically, cohort 2020-12 exhibits a 50% increase in retention during period 11, which may be associated with some strategy, promotion or be associated with Christmas sales. 

Respect to cohorts after 2020-12, a drop in retention of approximately 80% is observed. and is maintained during the following periods.

![image](https://user-images.githubusercontent.com/84011018/166730344-69de8896-0468-43e5-a933-7ca2c23230c2.png)




