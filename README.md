# Sales Insights

### Dashboard Link : [Sales Insights Dashboard.pdf](https://github.com/user-attachments/files/17546980/Sales.Insights.Dashboard.pdf)

## Problem Statement

TechCore, a company specializing in computer hardware and peripherals sales across India, is experiencing challenges as its business grows. The Sales Director is struggling to track and analyze sales performance across various regions, including North, South, West, and East India. His current method of relying solely on reports from regional sales managers is proving insufficient, as he believes the data provided may not fully reflect the true sales figures. 

The sales managers deliver numerous Excel reports, but the overwhelming amount of data makes it difficult for the director to derive meaningful insights. As a result, decision-making has become cumbersome. To address this issue, the Sales Director wants to hire a Business Analyst to develop a dashboard that will consolidate the sales data and present it in a clear, actionable format, enabling him to quickly identify trends and make informed decisions.

### Methodology Followed 
We will utilize the AIMS grid, a project management tool, to systematically address the problem. The AIMS grid consists of four key components:

We will utilize the AIMS grid, a project management tool, to systematically address the problem. The AIMS grid consists of four key components:

- **Purpose**: To uncover valuable sales insights that are currently hidden, hindering effective decision-making.
- **Stakeholders**: Sales Team, Analytics Team, Marketing Team, IT Team.
- **End Result**: An automated dashboard that delivers real-time, actionable insights, empowering informed and timely decision-making.
- **Success Criteria**: The dashboard should reveal key sales insights, enabling the Sales Director to make more informed and effective decisions based on these insights.

### Data Analysis using SQL
TechCore stores all its data in SQL and hence SQL will be used as a data repository. 

- **Step 1**: Load the data in SQL. The database consists of five tables: customers, date, markets, products and transactions.
- **Step 2**: Count the number of rows in each table to get an idea of the quantity of data available.
     
       SELECT count(*) FROM sales.transactions;
       SELECT count(*) FROM sales.customers;
       SELECT count(*) FROM sales.markets;

  Counting the number of transactions in various markets:
     
       SELECT * FROM sales.transactions WHERE market_code="Mark002";
       SELECT * FROM sales.transactions WHERE market_code="Mark005";
       SELECT * FROM sales.transactions WHERE market_code="Mark007";

  Some transactions have currency in USD to perform analysis  all transactions in USD must be converted to INR. This is handled in PowerBI
  The transaction which has currency in USD:
    
       SELECT * FROM sales.transactions WHERE currency="USD";

<img width="439" alt="Currency USD" src="https://github.com/user-attachments/assets/3e94cb48-5968-402a-a521-88aee3ab7709">



  Performing an inner join between the date table and the transaction table to analyze the transaction in each year.
    
        sales.date.* FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date WHERE sales.date.year=2020;
        sales.date.* FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date WHERE sales.date.year=2019;

  Now to check the revenue generated in a particular year (2020 and 2019):
  
       SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date WHERE sales.date.year=2020;
       SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date WHERE sales.date.year=2019;

  Now to check the revenue of a particular market in a particular year 

       SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date  WHERE sales.date.year=2020 and sales.transactions.market_code="Mark002";
       SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date  WHERE sales.date.year=2019 and sales.transactions.market_code="Mark003";

<img width="751" alt="Screenshot 2024-10-05 181513" src="https://github.com/user-attachments/assets/93bcd334-8513-4311-a32b-1cee00fe4f3e">

### Data Cleaning and ETL
- Establish a connection between MySQL and PowerBI and create a STAR schema data model to establish relationships between different tables.
<img width="596" alt="Screenshot 2024-10-06 151022" src="https://github.com/user-attachments/assets/25f7d634-86b3-402a-8398-0c1c9fb4331a">

- The markets table has two market names New York and Paris which need to be removed since TechCore has all its clients in India and these two transactions seem to be a one off transaction. 

- In the transaction table there are few transactions where the order quantity is zero or less than zero. These transactions need to be removed.

- In the transaction table, some trssactions are in USD, these need to be converted to INR to have a normalised currency. This is done in power query by creating a custom coloumn (norm_sales_amt) and multiplying the USD currency by 84 which is the exchange rate.

### Building the Dashboard
- Measures created for the analysis: Sales Quantity, Total Profit Margin, Profit Margin Contribution, Profit Margin %, Revenue, Revenue Contribution %, Revenue Last Year. 

#### Charts
##### Profit Analysis
- Revenue Contribution % chart: shows the percentage of the total revenue contribution made by each market. This chart also has customer type added under legends to distinguish between Brick and Mortar and E-commerce customers.
<img width="139" alt="revenue contri %" src="https://github.com/user-attachments/assets/c0b86f84-dae0-4d80-a0dc-1a7110361d83">

- Profit Contribution %: shows the percentage of the total profit contribution made by each market.
<img width="125" alt="profit contri %" src="https://github.com/user-attachments/assets/7a44ab74-36fd-4913-b498-2449f3b02b2d">

- Profit % by Market: shows the percentage of profit of each market when compared to the others.
<img width="122" alt="Profit % by mkt" src="https://github.com/user-attachments/assets/9d6471d5-bea6-47c8-8628-19df565f8262">


- Revenue Trend: Shows the revenue over time.
<img width="325" alt="revenue trend" src="https://github.com/user-attachments/assets/7c3df387-bfbd-47a9-9f38-07469bcc35e4">

- Customer Table: A table view of all the customers sorted according to Revenue, Profit or % contribution.
<img width="329" alt="Customer table" src="https://github.com/user-attachments/assets/61ae4abd-0ab1-40d4-8378-e38144f4b229">


##### Performance Insights
- Profit Margin % charts: Shows the profit margin percentage which can be drilled down -  market zone > market name > customer name.
<img width="303" alt="profit margin %" src="https://github.com/user-attachments/assets/6afcaaba-d106-4cb4-b846-a95a2173376c">

- Revenue Trend: Shows the current revenue against the last year's revenue. Also shows a trend line for the profit margin %.
<img width="419" alt="revenue trend line" src="https://github.com/user-attachments/assets/0ce22bdc-7469-4244-a61d-bbb898a9a5f1">

- Customer Table: A table view of all the customers sorted according to Revenue, Profit or % contribution.
<img width="329" alt="Customer table" src="https://github.com/user-attachments/assets/61ae4abd-0ab1-40d4-8378-e38144f4b229">

- Profit Target: A dynamic slicer in which the user can alter the profit target. The profit margin % chart responds to these changes with changes in the bar colour. Red signifies below target and blue signifies on or above target.
<img width="134" alt="profit target" src="https://github.com/user-attachments/assets/260ff598-4402-444d-b685-1d8716d51129">


### Dashboard

<img width="815" alt="dash1" src="https://github.com/user-attachments/assets/2e3e1b7b-640f-499c-a83b-bf164e79be8a">

<img width="814" alt="dash2" src="https://github.com/user-attachments/assets/2b80296d-4e3d-4756-8170-99268206d7e7">
