# Project: SQL Customer Engagement Analysis

<br>

## **Business Understanding**

This is a car insurance policy companythat have records of all clients details and their policies crossed with customer lifetime, complaints and other attributes.

<br>

## **Goal**

Discover what affects customer engagement (mostly longer lifetime that results in insurance renewals) and provide recommendations for the business. 

<br>

## **Walk through:**

1. [Data cleansing / Data Preprocessing to make it SQL-ready](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/WA_Fn-UseC_-Marketing-Customer-Value-Analysis.csv),
   
2. [Create SQL DB and tables](#anchor2),
   
3. [Data Understanding](#anchor3),

4. [Explore engagement attributes in the dataset and rank the data based on engagement](#anchor4).
   
5. [Conclusions and Recommendations](#anchor5).

<br>

## **Source:**

[Kaggle](https://www.kaggle.com/code/chebotinaa/customer-engagement-analysis-with-r/)

<br>

## **Tools:**

SQL, PgAdmin4, Google Sheets, IBM Watson, Kaggle.
   
<br>

## **Author:**

Luiz Furtado <br>
[LinkedIn](https://www.linkedin.com/in/luiz-furtado-dev/) | [GitHub](https://github.com/TSLSouth)

<br>

---

<br>

## **1. Data cleansing / Data Preprocessing to make it SQL-ready:**

<br>

Made on Google Sheets. [External link](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/WA_Fn-UseC_-Marketing-Customer-Value-Analysis.csv)

<br>

---

<br>

<a id="anchor2"></a>

## **2. Create SQL DB and tables:**

<br>

Create Table customer_engagement:

```sql
CREATE TABLE public.customer_engagement (
    Customer varchar(50),
    State varchar(50),
    Customer_Lifetime_Value double precision,
	Response varchar(50),
	Coverage varchar(50),
	Education varchar(50),
	Effective_To_Date date,
	EmploymentStatus varchar(50),
	Gender varchar(2),
	Income int,
	Location_Code varchar(50),
	Marital_Status varchar(50),
	Monthly_Premium_Auto int,
	Months_Since_Last_Claim int,
	Months_Since_Policy_Inception int,
	Number_of_Open_Complaints int,
	Number_of_Policies int,
	Policy_Type varchar(50),
	Policy varchar(50),
	Renew_Offer_Type varchar(50),
	Sales_Channel varchar(50),
	Total_Claim_Amount double precision,
	Vehicle_Class varchar(50),
	Vehicle_Size varchar(50)
);
```

<br>

Confirming creation viewing all rows:

```sql
SELECT * FROM public.customer_engagement
```

<br>

Importing and confirming import:

![imported table customer engagement](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/table%20created%20and%20imported.png?raw=true)

<br>

---

<br>

<a id="anchor3"></a>

## **3. Data Understanding:**

<br>

The data has 9134 customer records with 24 variables. All customers in the data have policies expiring between Jan 1 to Feb 28, 2011.


Data description:

  - Customer: customer ID,
  - State,
  - Customer_Lifetime_Value: in days,
- Response: ?,
- Coverage: 3 types (Basic, Premium, Extended),
- Education,
- Effective_To_Date,
- EmploymentStatus: 5 types (Employed, Unemployed, Medical Leave, Disable, Retired),
- Gender: F = female and M = male,
- Income: integer,
- Location_Code: 3 types (Urban, Suburban, Rural),
- Marital_Status: 3 types (Single, Married, Divorced),
- Monthly_Premium_Auto: monthly amount you pay your insurance company,
- Months_Since_Last_Claim,
- Months_Since_Policy_Inception,
- Number_of_Open_Complaints,
- Number_of_Policies,
- Policy_Type: 3 types (Special Auto, Personal Auto, Corporate Auto),
- Policy: types (Special L1, L2 or L3, Personal idem, Corporate idem),
- Renew_Offer_Type: 4 types (1, 2, 3, 4),
- Sales_Channel: Branch, Call Center, Agent, Web,
- Total_Claim_Amount,
- Vehicle_Class: Luxury, Sports, SUV, Two-Door, Four-Door car,
- Vehicle_Size.
<br>

---

<br>

<a id="anchor4"></a>

## **4. Exploring engagement attributes in the dataset:**

<br>

How many unique customers do we have:

```sql
SELECT COUNT(DISTINCT customer) 
FROM customer_engagement;

9134
```

<br>

Customers with longer lifetime by state:

```sql
SELECT AVG(customer_lifetime_value) AS avg_customer_lifetime_value, state
FROM customer_engagement
GROUP BY state
ORDER BY AVG(customer_lifetime_value) DESC;
```
![avg lifetime by state](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/avg%20c%20life%20time%20by%20state%201.png?raw=true)

<br>

States with more customers:

```sql
SELECT COUNT(customer), state
FROM customer_engagement
GROUP BY state
ORDER BY COUNT(customer) DESC;
```
![States with more customers](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/customers%20by%20state.png?raw=true)

<br>

Customers with longer lifetime by income doesn't show obvious correlation. There are customers with less income and more lifetime and vice-versa:

```sql
SELECT AVG(customer_lifetime_value) AS avg_customer_lifetime_value, AVG(income) AS avg_income
FROM customer_engagement
GROUP BY income, state
ORDER BY AVG(customer_lifetime_value) DESC
LIMIT 50
```
![avg lifetime by income](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/avg%20lifetime%20by%20income.png?raw=true)

<br>

By gender:

```sql
SELECT AVG(customer_lifetime_value) AS avg_customer_lifetime_value, gender
FROM customer_engagement
GROUP BY gender
ORDER BY AVG(customer_lifetime_value) DESC
```
![avg lifetime by gender](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/by%20gender.png?raw=true)

<br>

By coverage shows big differences:

```sql
SELECT AVG(customer_lifetime_value) AS avg_customer_lifetime_value, coverage
FROM customer_engagement
GROUP BY coverage
ORDER BY AVG(customer_lifetime_value) DESC
```
![avg lifetime by coverage](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/by%20coverage.png?raw=true)

<br>

By education is slightly different. Less Education looks to performbetter:

```sql
SELECT AVG(customer_lifetime_value) AS avg_customer_lifetime_value, education
FROM customer_engagement
GROUP BY education
ORDER BY AVG(customer_lifetime_value) DESC
```
![avg lifetime by education](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/by%20education.png?raw=true)

<br>

% of difference between max and min lifetime of employment status is something:

```sql
SELECT MAX(avg_lifetime_value) / MIN(avg_lifetime_value)
FROM (SELECT AVG(customer_lifetime_value) AS avg_lifetime_value, employmentstatus
      FROM customer_engagement
      GROUP BY employmentstatus) AS perc_avg_lifetime;

9.8%
```

<br>

Divorced seems to have 6.8% longer lifetime than singles:

```sql
SELECT MAX(avg_lifetime_value) / MIN(avg_lifetime_value)
FROM (SELECT AVG(customer_lifetime_value) AS avg_lifetime_value, marital_status
      FROM customer_engagement
      GROUP BY marital_status) AS perc_avg_lifetime;

6.8%
```

<br>

Number of open complaints seems to play a important role on drop lifetime:

```sql
SELECT AVG(customer_lifetime_value) AS avg_customer_lifetime_value, number_of_open_complaints
FROM customer_engagement
GROUP BY number_of_open_complaints
ORDER BY AVG(customer_lifetime_value) DESC
```
![avg lifetime by complaints](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/open%20complaints.png?raw=true)

<br>

Customers with more than one policy has longer lifetime:

```sql
SELECT AVG(customer_lifetime_value) AS avg_customer_lifetime_value, number_of_policies
FROM customer_engagement
GROUP BY number_of_policies
ORDER BY AVG(customer_lifetime_value) DESC
```
![avg lifetime by number of policies](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/number_of_policies.png?raw=true)

<br>

Special Auto type policy seems to have aprox 10% longer lifetime than other types:

```sql
SELECT MAX(avg_lifetime_value) / MIN(avg_lifetime_value)
FROM (SELECT AVG(customer_lifetime_value) AS avg_lifetime_value, policy_type
      FROM customer_engagement
      GROUP BY policy_type) AS perc_avg_lifetime;

10%
```

<br>

Offers type 1 and 3 seems to have greater appeal than other types:

![avg lifetime by offers](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/offers.png?raw=true)

<br>

Quantity of sales by channel:

![sales by channel](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/sales%20by%20channel.png?raw=true)

<br>

Luxury, Sports and SUVs class have much more engagement than common cars:

![vehicle class](https://github.com/TSLSouth/SQL-Customer-Engagement-Analysis/blob/main/img/vehicle%20class.png?raw=true)

<br>

No significant differences in lifetime because of location_code, policy, sales_channel, total_claim_amount and vehicle_size.

<br>

---

<br>

<a id="anchor5"></a>

## **5. Conclusions and Recommendations:**

<br>

We can divide the attributes in 2 types: those that damage engagement, those that improve it and are more convenient to sale.

<br>

<details>
<summary>a) Those that undermine engagement</summary>
<br>

- Complaints: open compliants are directly connected with low rates of customer lifetime.

  **Recommendation:** resolve more and more quickly open complaints. A closer look at this is required.

</details>

<br>

<details>
<summary>b) Those that improve engagement</summary>
<br>

- Customers with 2 or more active policies tend to engage more,
- Customers with low education level, contrary what could be intuitive, are more likely to be loyal,
- Premium and Extended policies leverage to increase engagement,
- Offer type 1 and 3 results in longer lifetime,
- Luxury, Sports and Suvs car classes have much more engagement than common cars,
- Employees and divorced are more likely to stay,

  <br>

  **Recommendation:** the ideal target audience is employed, divorced, with low education, but with luxury, sport or a SUV that buy more than one insurance and prefer Premium and Extended servicies.

</details>

<br>

<details>
<summary>c) There is no significant influence on engagement:</summary>
<br>

- If the customer lives in a Rural or Urban environment, 
- If the policy was contracted with an Agent or through Web,
- Or because of the Claim Amount.

  <br>

  **Recommendation:** Observe the sales channel with less costs to the company and invest in it to raise sales through that channel (probably web).

</details>

<br>

---

<br>

## **References/Acknowledgments**

References: 

<br>

Kaggle, W3 Schools, IBM Data Science Certificate Course.

<br>

Thanks to: 

<br>

- I2A2 - Institute of Applied Artificial Intelligence, Canada to have accepted me in their amazing courses.  <br>
- Pierian Data Training, USA to their great online courses.
- And to all friends that welcome me in the data science world and help me so much with their kindness, knowledge, experience and orientation.

<br>

'*******************************'
<br>
