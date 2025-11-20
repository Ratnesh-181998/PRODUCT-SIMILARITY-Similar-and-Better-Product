Product Similarity: 

Business Requirement:  

In GeM portal, buyer can shortlist products based on a specific set of parameters and get the best price 
for the given set of parameters. However, no options / directional recommendations are provided to the 
buyers on possible parameter combination, that is not only an equivalent of the selected set of 
parameters but might also give a better value to cost proposition. 

Presently, the GeM portal focuses solely on best price discovery rather than discovering the best value 
for the price. This means buyers are not informed about similar or better-featured products within the 
same price range as their selected product. As a result, buyers may not be making the most informed 
purchasing decisions and might be paying more than necessary. 

To address this gap, GeM wants to develop a product similarity module to assist buyers. This module will 
offer a list of product suggestions with the same features and quality at a lower price or highlight better 
products available at the same price. By providing these recommendations, buyers can make more 
informed decisions, ensuring they receive the best value for their money. 
With this solution, buyers will gain access to: 

• Similar Products at Lower Prices: Identifying products with equivalent features at more 
competitive prices. 
<img width="2047" height="1073" alt="image" src="https://github.com/user-attachments/assets/d61db2ff-8166-41c5-ad99-e34426365d85" />

• Better Products at Lower Price: Highlighting options that offer superior features or quality 
without exceeding the buyer's budget. 
<img width="2027" height="1055" alt="image" src="https://github.com/user-attachments/assets/20bfc29c-41b3-4b37-87e0-6b4c5957d607" />


By implementing the product similarity module, GeM aims to enhance the purchasing experience, 
ensuring buyers achieve optimal value for their expenditure. 
Proposed solution: 
The Product Similarity module is an intelligent, machine learning-powered system designed to identify 
products that are similar to a buyer's current selection. This module will recommend products either 
with a better price or superior value. 

The objective of the product similarity module is to identify similar products for a given product of 
interested based on two primary criteria: 
• The features of the shortlisted products are similar to the product of interest. 
• The price of the shortlisted products is either lower than the product of interest or offers better 
value with upgraded components. 

The Product Similarity module will transform the GeM portal from a platform focused solely on price 
discovery to one that also emphasizes value discovery. By offering intelligent, machine-learning-driven 
product comparisons, buyers will have access to a wider range of options that meet their needs both in 
terms of features and cost, ultimately enhancing their overall purchasing experience on the GeM portal.

Solution Consumption: 

Currently, this solution is integrated into the Product Display Page (PDP) of the GeM portal, providing 
buyers with actionable recommendations directly where they view product detail.
<img width="1343" height="1305" alt="image" src="https://github.com/user-attachments/assets/45693497-0f23-4717-a0f1-551827075015" />

<img width="1334" height="733" alt="image" src="https://github.com/user-attachments/assets/4b675dc4-3032-4bbd-ba97-c71c306c8876" />

Data: 

To build this solution, required data consist of all the product id’s in a category, along with its all-golden 
parameters, attributes within parameters and their respective order and offer price. The required data 
can be fetched from below tables and the list of golden parameters can be fetched from GeM API.  
• Table: inb_variants  
• Table: inb_browse_nodes  
• Table: inb_stock  
• Table: inb_catalog_attrs  
• Table: ffm_orderitems 
• API - https://mkp.gem.gov.in/pub/api/browse_nodes/show_schema.json?browse_node_id=home_info_comp_comp_de52008682

Model Methodology: 

To enable computation of feature similarity and importance, the product catalogue data - GeM data - 
will be one-hot encoded (better representation) and then we perform AI/ML model. This will help us 
understand the influence of each feature on price and also which attributes of a feature are similar. This 
analysis lays the foundation to understanding product value and product similarity.  

The order of attribute importance is also considered in the analysis. Using data from the GeM API, we 
extract the attributes within a parameter and their order of importance. For parameters without 
predefined attribute orders, we can determine their order through business insights, research, or AI/ML 
approaches. The determined order of importance of attributes will be validated with the respective 
Category Owner/Manager. 

To identify the impact of each parameter on the product price, we use AI/ML models. Every product has 
two types of parameters: golden and non-golden. In analyzing the relationship between price and parameters, 
we consider the golden parameters. The order of importance of these parameters is also 
validated with the respective Category Owner/Manager. The solution allows for flexibility in changing 
the order of importance of attributes and parameters as per the requirements of the category owner. 

The order of importance of attributes is then scaled between 0 and 1. Using the scaled values of 
parameters and their importance, we calculate the score of product IDs by summing all the parameter 
scores. This process generates a score table for each product category. 

With the product IDs scored based on features and attributes, we can compare them with other 
products. We can filter the results based on the offer price of the selected product, showing the most 
similar product with a lower price than the selected product. Additionally, since the scores are directly 
proportional to the features of the product, higher scores indicate better products. Thus, we can suggest 
better products within the price range of the selected product.

<img width="2006" height="1074" alt="image" src="https://github.com/user-attachments/assets/4bb469d0-0b2d-43e8-ad61-c2382139fca2" />

<img width="1994" height="1091" alt="image" src="https://github.com/user-attachments/assets/2996fa97-67f4-4fbb-97c5-48e8e8bea1d7" />

See the picture below to know the flow of processes in this solution:
<img width="1364" height="623" alt="image" src="https://github.com/user-attachments/assets/0af28b36-95b6-4eea-ab42-dd8175bf5144" />

Steps In Model:  

1. Extract the required data - All product/variant id’s and associated its golden parameters of a 
category along with its offer price.  
• From ‘stock’ and ‘Orderitem’ table, we get the price of listed product and sold products 
respectively  
• For rest of the products we get the offer price from ‘stock’ table  
2. Clean data (pre-processing)  https://mkp.gem.gov.in/pub/api/browse_nodes/show_schema.json?browse_node_id=home_info_comp_comp_de52008682
• All parameters stored in YAML format, covert to python data-frame  
• Scrape the technical parameter names from API of GeM website and attributes within 
parameters along with their order/rank 
• Rename the parameters  
• Select only the golden parameters for further analysis  
• Missing value treatment
3. Attributes which don’t have order provided, use business knowledge; if no business knowledge 
available, need to research and if both these ways don’t work then we use AI/ML model to get 
order. Parameters which have data type ‘group ordinal’, ‘ordinal’, ‘Boolean’, and ‘numeric and 
measurable’ order has provided. Only for ‘enumerable’ data type don’t have the order provided in 
API.  
• Categorize parameters into 4 categories  
• One hot encoding of categorical parameters  
• Create Linear regression model. Price as target variable and parameters as independent 
variables.  
4. Scaling the parameter attribute’s values within 0 and 1.  
5. Importance of parameter calculation using ML algorithm. 
6. Final Score table by summing parameter’s attribute value.
   <img width="1338" height="396" alt="image" src="https://github.com/user-attachments/assets/5760ca10-46b3-423a-ae22-61a51e817285" />

Business Value:  

1. Enhanced Value Discovery: The solution enables buyers to discover the best value for their price 
range by providing product recommendations with better specifications. Buyers can make more 
informed decisions and get the most value out of their budget. 
2. Market Price Sensitivity: By offering insights into similar or better specification products within the 
GeM platform, the solution keeps buyers informed about current market trends. This market price 
sensitivity ensures buyers are aware of their options and can make better purchasing decisions 
based on comprehensive market knowledge. 
3. Improved Transparency and Efficiency in Public Procurement: The solution aligns with GeM's 
objective of enhancing transparency and efficiency in public procurement. It improves various 
processes such as: Bid Process (seller evaluation and technical rejection), Incident Management and 
Catalogue Management System (CMS) which detects price abnormalities and ensures sellers quote 
realistic prices, maintaining the integrity of the product listings
![Uploading image.png…]()

Integration & API: Developed and integrated RESTful APIs using FlaskAPI :

SIMILAR PRODUCT 
Input: Solution requires product Id as input. 

<img width="1306" height="603" alt="image" src="https://github.com/user-attachments/assets/9cfba3c1-5174-4d87-808b-aff109f0d73a" />

<img width="1374" height="1214" alt="image" src="https://github.com/user-attachments/assets/1721bc78-79c5-4205-bcf2-b435df4f8893" />

BETTER PRODUCT 
Input: Solution requires product Id as input. 

<img width="1347" height="714" alt="image" src="https://github.com/user-attachments/assets/a0cec101-2965-494e-b90c-1262aa322b5f" />

<img width="1087" height="1113" alt="image" src="https://github.com/user-attachments/assets/8f079f4f-49d3-4d3a-a0ae-81d98790b2bd" />


