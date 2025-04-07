# Background
CosmicCart, a mobile-first digital marketplace, has experienced explosive growth over the past two years. However, the recent drop in customer satisfaction ratings has become an issue due to the customer service agents struggling to keep up with the exponential increase in their customer base. 

## Objectives
* Reduce the average handling time for customer service agents.
* Provide customer service agents with a comprehensive view of each customer, including their history and interactions.
* Explore the potential benefits of using generative AI to enhance the productivity and efficiency of customer service agents in managing the increasing customer base.

# Tools utilized
1. Salesforce CRM (Cloud SaaS) - Customer details 
2. Zendesk (Cloud SaaS) - Tracking/handling customer queries
3. Stripe - Customer payments/charges information
4. DHL, SingPost and NinjaVan REST APIs - Providing shipping information
5. Snowflake - Customer data lake
6. Amazon RDS service to consolidate data from the various source systems

# Proposed solution stack
Reducing Average Handling Time for Customer Service Agents:
*	Salesforce CRM with Zendesk - Provide agents with a unified view of customer information and interactions.
*	Workato - Automate retrieval of customer data from Salesforce CRM and Zendesk, reducing the time agents spend manually searching for information.
*	Zendesk - Implement automated responses for common customer queries using Zendesk macros or automated workflows triggered by specific events in Salesforce CRM. <br>

Providing Comprehensive Customer View:
*	Workato
  - Synchronize customer data across the platforms involved (Salesforce, Zendesk, Stripe, AWS RDS, Shipping APIs) in ensuring agents have access to the latest information.
  - Utilize data mapping and transformation capabilities to standardize formatting of customer data across systems.
  - Implement real-time updates for customer interactions, ensuring agents have access to the most recent communications and transactions. <br>
  
Automation of Data Loading to Customer Data Lake:
*	Set up scheduled data extraction workflows to pull data from source systems and load it into Snowflake data lake.
*	Utilize Workato's data transformation capabilities to standardize data formats and ensure consistency across sources.
*	Implement error handling and data validation processes to maintain data integrity and accuracy.
*	Monitor data loading processes and performance metrics using Workato's analytics dashboard.

## Project scope and deliverables 
Project Scope:
•	Integration of Salesforce CRM, AWS RDS, Zendesk, Stripe.com, Shipping APIs, and Snowflake data lake.
•	Automation of data loading from various source systems to the Snowflake data lake.
•	Implementation of workflows to reduce average handling time for customer service agents.
•	Provision of a comprehensive view of each customer, including their history and interactions, for the benefit of customer service agents when handling customer ticket queries. <br>

Deliverables:
•	Integrate workflow using Workato to connect Salesforce CRM, AWS RDS, Zendesk, Stripe.com, Shipping APIs, and Snowflake data lake.
•	Automate data loading processes from source systems to Snowflake data lake using Workato.
•	Integration of generative AI platforms with Zendesk to provide AI-powered customer service capabilities.
•	Workflows and automation scripts to reduce average handling time for customer service agents.
•	Unified customer view dashboard in Zendesk, providing agents with comprehensive customer information.
•	Documentation of integration strategy, workflows, and automation processes for future reference and scalability.

## Integration architecture design 
Below is the proposed technology stack for automating data loading into customer data lake as well as for efficient extraction of customer information from the essential applications. This aids customer service agents better by having customer information readily at hand when attending to a customer ticket request. <br>

<img src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/e7c0b305-8c0b-4f5e-8240-edf7bd7a48e2" width="60%">

### ZENDESK <br>
The function of ZENDESK is to fetch customer support tickets, agent activities, and customer interactions. It is also integrated with Salesforce CRM to provide agents with a comprehensive view of customer history and interactions. When a customer support ticket is triggered in ZENDESK, it could be dispute cases such as charges dispute or general support enquiry. <br>

<img src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/16d43230-432f-48c0-8198-bfab16ff2b6e" width="70%">

Formulas are employed in the connector to introduce logic for differentiating between general customer support tickets and tickets raised for charge dispute settlements. 

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/5b4e679c-4d21-4539-929b-7dec171719e7"> <br>
<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/98d5e227-a3a2-475d-8623-e0f99187d9e3">

### Snowflake <br>
The Snowflake connector loads customer data from Salesforce, Zendesk and Stripe into Snowflake’s data lake. Besides serving as a centralized storage point for aggregating customer data, it also provides analytical and reporting capabilities allowing CosmicCart to gain insights into customer trends, preferences, and behavior. <br>
Snowflake also integrates with Workato to automate the ETL process of customer data from source systems into the data lake. Once a new ticket is raised in ZENDESK, a new record is created in the Snowflake database after consolidating the information from the various sources. The ‘Table’ field refers to the database table where the new record will be inserted. The columns refer to the selected columns for the new data to be inserted into.

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/cb5d7b79-28a7-440d-b3b5-b86365c43508">

### Salesforce <br>
The setup of the above Salesforce connector is designed for customer service agents to promptly retrieve detailed information about the associated case record in Salesforce when a customer ticket is raised. By referencing the requester's ID from Zendesk as the object ID, the corresponding case details from Salesforce can be retrieved, providing agents with comprehensive information to address customer inquiries effectively.

This integration enables seamless data exchange between Zendesk and Salesforce, allowing for a unified view of customer interactions and support history. Customer service agents can access the details of salesforce cases directly from within Zendesk, streamlining their workflow and enhancing productivity.

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/b604b388-c059-4907-a3c7-811488fd2702">

### Stripe <br>
Retrieving the requester's ID from Zendesk and the account ID from Salesforce, customer agents can quickly search for the customer's charges information from Stripe based on the customer's support ticket context and associated account information.

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/c2b948e6-c840-46c5-b75f-863ec688e50a">

#### Retrieving charges history in Stripe <br>
Retrieve the list of charges made to the customer on Stripe as well as charges information such as Charge ID, Amount captured, Invoice ID, and Payment method.

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/c3e20b81-ce9d-486b-add8-9e98b07365fe">

### Connecting to REST API of a shipping application (e.g Ninjavan) <br>
The purpose of integrating with the shipping platform API is to retrieve shipping information for the order. This action queries the platform's API to obtain details such as shipping method, delivery address, and shipping cost. The ‘GET’ method is used for retrieval of data from the web APIs.

In this case, the data returned by the Ninjavan API here is a JSON data type. 

Ninjavan’s API endpoint call for data retrieval: https://api-sandbox.ninjavan.co/sg. 

<img width="55%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/ad24209d-6620-4d14-885d-c86dc4aa2fd9">

#### Connecting to REST API of Amazon RDS (Database management system)
MySQL connector is used to connect to the Amazon RDS via Amazon account credentials. A new row (record) is inserted into the Amazon database management system whenever a new ticket is raised.

<img width="55%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/28e3830f-8b51-4396-aecd-c9d1bbda26b2">

‘Host’ refers to the location of the MySQL server to connect to. The host specifies the network address or hostname of the server where the MySQL database is hosted.

<img width="55%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/6509d406-7e7d-4031-a70a-09d04c302421">

Username and password refers to the account details of the Amazon RDS database management platform. Database refers to the name of the MySQL database to connect to.

<img width="55%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/22adf8e4-c169-4e0a-a9c5-961b90b7dcd4">

### Function of AMAZON RDS:
1.	Data Integration: Allows consolidation of data from multiple sources into a single database for analysis, reporting, or other purposes.

2.	Data Storage: Once data from Salesforce, Stripe, Zendesk, and other applications is loaded into Amazon RDS, it is stored within the database according to the required schema.

3.	Data Analysis and Reporting: Amazon RDS can then be used in conjunction with analytical tools or business intelligence platforms to analyze and report on data from Salesforce, Stripe, Zendesk, and other sources. By querying the data stored in Amazon RDS database, it allows users to gain insights into customer behaviour, transaction patterns, support ticket trends, and more.

### Future enhancements
1.	Automated Data Transformation and Enrichment:
*	Enhance data loading workflows with automated data transformation and enrichment capabilities to standardize data formats and enrich it with additional contextual information.
*	Utilize machine learning algorithms to automatically categorize and tag data, derive insights, and identify relationships between different datasets for more meaningful analysis.
2.	Predictive Analytics for Customer Service Optimization:
*	Implement predictive analytics algorithms to forecast customer service trends and anticipate potential issues that customers may encounter.
*	Use historical data to predict customer behavior, such as peak support hours or common support issues during specific periods.
