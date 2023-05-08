# EDA Sales Cohort Analysis

#### In the following notebook, the analysis of a set of data related to sales in a year is carried out with python libraries for data analysis such as pandas and plotly for data visualization. 
#### The tasks are concentrated in two stages:
* #### The first where exploratory data analysis and data cleaning is performed, and relevant information is generated for data analysis. 
* #### The second contemplates a cohort analysis where customer retention is analyzed in a certain period, which is very important for companies and their marketing strategies.

#### Exploring the data types is found that most of the data is of type object.This will make it difficult for us to group data analysis, so the data type must be changed in the following stages.

![image](https://user-images.githubusercontent.com/84011018/166722627-864d7b47-873d-4992-8c86-4538d912837d.png)

#### In the dataframe there are negative values associated with columns units and amounts, also null values. These values are related with alphanumeric invoices in invoice column, so these data correspond to returns or rejected transactions. These data make analysis difficult, so they are removed from the current dataframe.

![Screenshot 2022-05-04 175548](https://user-images.githubusercontent.com/84011018/166721930-7671c6b8-b4d3-4201-b07b-fea6e3949d2a.png)

#### For eliminated this values form the dataframe a index is created in units column for values < 0. 

```python
# Create index from 'units' columns with values < 0
index = df[df['units'] < 0 ].index
# drop the index with negative values 
df.drop(index , inplace=True)
df
```

#### After that the index is drop from the data frame and have the clean dataframe.

![image](https://user-images.githubusercontent.com/84011018/166724686-a693b50a-ff9a-471a-89e4-9626a31028ae.png)

#### to be able to group data and create visualizations, the format of the date is changed to datetime.

```python
# Format date time and create new time serie for visualizations
sales['date'] = pd.to_datetime(sales['date']).dt.date
sales['date'] = pd.to_datetime(sales.date, format='%Y-%m-%d %H:%M:%S')
sales['order_month'] = sales['date'].dt.to_period('M')
sales['order_month'] = sales['order_month'].dt.to_timestamp('s').dt.strftime('%Y-%m')
```

#### Invoices by month

#### Grouping the invoice data by month, it can be seen in the line graph that there is a large number of invoices in the month of November. This may correspond to the seasonality of purchases for the Christmas party, since afterwards a pronounced decrease in the number of invoices is observed.

![image](https://user-images.githubusercontent.com/84011018/166726310-345afa3a-7196-47eb-a1e4-756b17e42ee2.png)

Uniques clients by month

#### Grouping the unique customers per month chart, a trend similar to the invoices/month graph is observed. It can be determined that there was a large number of customers making purchases in a given month by increasing the volume of invoices.

![image](https://user-images.githubusercontent.com/84011018/166726581-63c50066-d6fe-49b1-a023-d69bf11fadbc.png)

Top countries by invoice

#### Grouping the invoice number data by country, it is observed that the countries that generate the most invoices are the UK, Germany, France and Eire, with the UK being the one that generates the largest number of invoices.

![image](https://user-images.githubusercontent.com/84011018/166726721-e1469464-f595-496d-b74f-895e545700f7.png)

Percentage of invoices by countries

#### Observing the same data and converting it to a percentage, it can be seen that the UK has 89% of the total invoices generated in 2021.

![image](https://user-images.githubusercontent.com/84011018/166726969-9c29e8cd-8bd1-4a13-98fe-6f41d0c89ff6.png)

Total amount by countries

#### As expected, when graphing the total amounts accumulated during 2021, the same trend as in the previous graphs is observed, showing that the UK generated the highest amount during the 2021 period.

![image](https://user-images.githubusercontent.com/84011018/166727239-18b79a1f-a646-48c4-a06b-a619050f01bb.png)

### Cohort analyst

#### In this stage is used the cohort analyst. This method analyze how many customers remain in business during the period, which can indicate the impact of the business strategies implemented on customers to keep them over time or period.

![image](https://user-images.githubusercontent.com/84011018/166727694-e546fa4a-29bf-4e5c-a995-2a62f5fa04ee.png)

#### Analyzing the data, it is observed that there are no negative values in the units column, and the data count of the `invoice` column and the `id_client` column are also observed, so there are no null values. Finally, it was observed that the maximum number of `invoices` is greater than the number of clients because it already gives us an idea of retention data.

#### Grouping the data by `client_id` and `invoice` numbers and filtering by the `id_clients` that have generated more than one `invoice`,
#### it is obtained that 67% of the clients ordered more than once.

### Histogram

#### The histogram shows that the majority of customers 3900 aprox. presents an invoice frequency of 0-9, then decreases to clients with invoice frequencies of 10 -19, and only some exceptions, clients have 50 to 200 invoices.

![image](https://user-images.githubusercontent.com/84011018/166728662-1b626657-64f7-4508-a192-827c2d2a5e9f.png)

### Cohort variables

#### The next step is to create the variables for the cohort analysis. To do this, two columns are generated, one for each month of the year and the other the date on which the customer made the first purchase. 

```python
df['order_m'] = df['date'].dt.to_period('M')
df['cohort'] = df.groupby('id_Client')['date'] \.transform('min') \.dt.to_period('M') 
```

#### Then they are grouped, a new dataframe is generated with the number of clients generated per month.

![image](https://user-images.githubusercontent.com/84011018/166729335-4a871490-fa1c-4bcf-80d9-48d8601fd7f9.png)

After this step, a pivot table is generated to relate the cohort to a given period.

![image](https://user-images.githubusercontent.com/84011018/166729640-0ce49e71-034f-4438-80c3-4d7ae5d36e22.png)

### Retention matrix analyst

#### Obtaining the pivot table, the retention matrix can be generated by dividing each row by the cohort size, which corresponds to the first column that contains the total number of customers for their first purchase.

![image](https://user-images.githubusercontent.com/84011018/166730344-69de8896-0468-43e5-a933-7ca2c23230c2.png)

* The retention table show us that the 2020-12 cohort has higher user retention than subsequent cohorts. 
* It is observed that in cohort 2220-12 in period 11 there is an increase of 50% retention that may be associated with some strategy, promotion or be associated with Christmas purchases seen in previous visualizations.
* In the cohorts after 2020-12, a drop in retention of approximately 80% is observed. and is maintained during the following periods.


