# Online_retail_analysis
Customer segmentation is the technique of dividing customers into groups based on their purchase patterns to identify who are the most profitable groups. In segmenting customers, various criteria can also be used depending on the market such as geographic, demographic characteristics or behavior bases, etc. There are so many techniques out there but I will use RFM analysis for my analysis.


## Business requirements

The Sales Manager wants me to segment customers into 4 levels: VIP, Normal, Low, and Extremely Low. The purpose of the Sales Manager when requesting me to do that because not only does he want to better understand customers but he also wants to give some attractive endowments to retain the customers based on their level.

Data sources: https://www.kaggle.com/datasets/jihyeseo/online-retail-data-set-from-uci-ml-repo

**Note:** The last date in the dataset is 2011-12-09. However, the day I do this project is 2022-07-22. Therefore, I modify the year in the dataset from 2010-2011 to 2020-2021 and I assume that the director wants to conduct RFM analysis in 2021-12-10.

## Data cleaning

First, I check data distribution in Kaggle and find that there are some missing values in the customerID column. Then, I write an easy query to check if there are any returned products. You can see that there are a lot of returned products in this dataset – the negative numbers are returned quantities.


In addition, the InvoiceDate is in Date-time format, in this analysis, I only want to focus on the date of the customer, not the time so I convert the date-time format of the InvoiceDate column into Date format. Finally, I also remove all duplicates in my dataset.

Here are all the queries that I use to clean the data
![image](https://user-images.githubusercontent.com/101198685/215956972-863e90ef-28d8-49b3-a979-8663c897ec28.png)

## RFM analysis

**Recency:** To calculate the recency (the last time that Customers are active), I just only find the latest date that customers buy stuff by using MAX() function and subtracting it by 2021-12-10.

**Frequency:** When calculating frequency, at first I don’t realize that there are differences between the number of orders and the number of products that customers buy. After encountering errors, I gradually understand that to calculate the frequency we have to count the number of Orders not the number of Products because one order may have lots of products. We want to find out how many times customers buy stuff not how many products they have bought.

![image](https://user-images.githubusercontent.com/101198685/215957047-dcf66476-2bbe-4675-aeef-238df521adc5.png)

Look at the picture above, the customer who has CustomerID 17850 bought 2 times. One is in 8:26 and the last one is in 8:28 with the Invoice being respectively 536365 and 536366. One order may have a lot of products like there are lots of products in InvoiceNo 536365 but actually, they just buy stuff only 2 times.

Therefore when I count the orders I have to combine the COUNT and DISTINCT functions.

**Monetary:** It is so easy to find out how much customers have spent by multiplying the quantity of each product by its price then we sum it up by each customer.

![image](https://user-images.githubusercontent.com/101198685/215957087-a8e7adb7-165e-43fe-a253-ebaebe9e1843.png)
![image](https://user-images.githubusercontent.com/101198685/215957106-0577d09b-f721-4819-be6e-7bc04a7bfa5d.png)

After having recency, frequency, and monetary, I apply the Quantile concept in statistics and respectively divide the values of each of them into 4 levels of Quantile ( 0-25th, 25th-50th, 50th-75th, and 75th-100th)

Before doing that, I have to calculate the percentile of each value by using the PERCENT_RANK function
![image](https://user-images.githubusercontent.com/101198685/215957155-13bc83d6-3818-4861-8aad-0e88a225dc4f.png)
![image](https://user-images.githubusercontent.com/101198685/215957178-46314818-8465-4e4f-8f01-6b8e0a1d18a5.png)

Now I will respectively divide the values of each recency, frequency, and monetary into 4 levels of Quantile ( 0-25th, 25th-50th, 50th-75th, and 75th-100th)

**Note:** I label 1, 2, 3, and 4 respectively for 0-25th, 25th-50th, 50th-75th, and 75th-100th for frequency and monetary. However, I only label 4, 3, 2, and 1 respectively for 0-25th, 25th-50th, 50th-75th, and 75th-100th for recency, because recency means how much time has elapsed since a customer’s last order. Therefore the smaller the value is, the more engaged a customer is with that brand.
![image](https://user-images.githubusercontent.com/101198685/215957232-5ad490cf-0077-4cf0-bc21-28cc81ff03e6.png)

![image](https://user-images.githubusercontent.com/101198685/215957997-f87b57ba-97bb-45b6-bc4a-e93b40eeacc9.png)

When I have done all of the steps above, it is time to actually segment the customer into 4 levels like the request of the Sales Manager: VIP, Normal, Low, and Extremely Low.

First, I will sum up the values of Recency_rank, Frequency_rank, and Monetary_rank altogether and I attach those values to the RFM_score column. Then, I continue applying the Quantile concepts.
![image](https://user-images.githubusercontent.com/101198685/215957329-66c83655-fb6a-4a2b-8204-c0f66d296506.png)
![image](https://user-images.githubusercontent.com/101198685/215957346-412527ed-9d64-469a-8979-3b2c61694f46.png)
Now, instead of labeling 1, 2, 3, and 4 respectively for 0-25th, 25th-50th, 50th-75th, and 75th-100th, I label VIP, Normal, Low, and Extremely Low respectively for 0-25th, 25th-50th, 50th-75th, and 75th-100th.
![image](https://user-images.githubusercontent.com/101198685/215957387-902472ba-80dd-4f2a-b21e-57607500b103.png)
![image](https://user-images.githubusercontent.com/101198685/215957409-aea63927-d36f-4f43-b1bf-c4fb45cfd2b9.png)
This is the final result of the RFM analysis, customers are finally divided into 4 levels. However, I found that the result is so tedious for my Sales Manager to follow, so I decided to create a visualization of the customer segmentation. Here is the visualization that I will show to my Manager.
![image](https://user-images.githubusercontent.com/101198685/215957451-0b889e73-bfc1-4f7b-b229-af5c87bf4908.png)
As you can see, the Extremely Low group accounts for the majority of the Customer Segmentation levels. Therefore, we need to create more attractive discounts or events to increase the conversion rate of those customers from the Extremely Low group to higher levels like Normal or VIP. In addition, the VIP group has the least members so the Sales Manager has to construct some strategies to retain those customers because those customers in the VIP group can contribute huge money to our company.







