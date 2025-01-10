# Background
The study here involves a company called CosmicCart, which is a mobile-first digital marketplace. It has experienced explosive growth over the past two years. However, the recent drop in customer satisfaction ratings has become an issue due to the customer service agents struggling to keep up with the exponential increase in their customer base. 

## Objectives
* Reduce the average handling time for customer service agents.
* Provide customer service agents with a comprehensive view of each customer, including their history and interactions.
* Explore the potential benefits of using generative AI to enhance the productivity and efficiency of customer service agents in managing the increasing customer base.

# Tools utilized
1. Salesforce CRM (Cloud SaaS)
2. Custom developed e-Commerce platform with REST APIs available to 
access product information. (Hosted on AWS cloud)
3. Order information stored on AWS RDS service
4. Zendesk (Cloud SaaS) for customer support
5. Stripe.com for payments
6. Shipping information provided by DHL, SingPost and NinjaVan REST 
APIs
7. Customer data lake using Snowflake

# Proposed solutions
Reducing Average Handling Time for Customer Service Agents:
•	Integrate Salesforce CRM with Zendesk to provide agents with a unified view of customer information and interactions.
•	Use Workato to automate the retrieval of customer data from Salesforce CRM and Zendesk, reducing the time agents spend manually searching for information.
•	Implement automated responses for common customer queries using Zendesk macros or automated workflows triggered by specific events in Salesforce CRM.
Providing Comprehensive Customer View:
•	Use Workato to synchronize customer data across the platforms involved (Salesforce, Zendesk, Stripe, AWS RDS, Shipping APIs) in ensuring agents have access to the latest information.
•	Utilize Workato's data mapping and transformation capabilities to standardize customer data formats across systems.
•	Implement real-time updates for customer interactions, ensuring agents have access to the most recent communications and transactions.
Automation of Data Loading to Customer Data Lake:
•	Set up scheduled data extraction workflows to pull data from source systems and load it into Snowflake data lake.
•	Utilize Workato's data transformation capabilities to standardize data formats and ensure consistency across sources.
•	Implement error handling and data validation processes to maintain data integrity and accuracy.
•	Monitor data loading processes and performance metrics using Workato's analytics dashboard.

## Project scope and deliverables 
Project Scope:
•	Integration of Salesforce CRM, AWS RDS, Zendesk, Stripe.com, Shipping APIs, and Snowflake data lake.
•	Automation of data loading from various source systems to the Snowflake data lake.
•	Implementation of workflows to reduce average handling time for customer service agents.
•	Provision of a comprehensive view of each customer, including their history and interactions, for the benefit of customer service agents when handling customer ticket queries.
Deliverables:
•	Integration workflows built using Workato to connect Salesforce CRM, AWS RDS, Zendesk, Stripe.com, Shipping APIs, and Snowflake data lake.
•	Automated data loading processes from source systems to Snowflake data lake using Workato.
•	Integration of generative AI platforms with Zendesk to provide AI-powered customer service capabilities.
•	Workflows and automation scripts to reduce average handling time for customer service agents.
•	Unified customer view dashboard in Zendesk, providing agents with comprehensive customer information.
•	Documentation of integration strategy, workflows, and automation processes for future reference and scalability.

## Integration architecture design 
Below is the proposed technology stack for the automation of the data loading into the customer data lake as well as for the efficient extraction of customer information from the essential applications for supporting customer service agents when attending to customer support tickets. Workato's enterprise automation platform is used in this project for the integration process. <br>

<img src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/e7c0b305-8c0b-4f5e-8240-edf7bd7a48e2" width="60%">

#### ZENDESK <br>
The function of ZENDESK is to fetch customer support tickets, agent activities, and customer interactions. It would also integrate with Salesforce CRM to provide agents with a comprehensive view of customer history and interactions. When a customer support ticket is triggered in ZENDESK, it can be in the form of a dispute case such as charges dispute or general support enquiries. <br>

<img src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/16d43230-432f-48c0-8198-bfab16ff2b6e" width="70%">

Formulas are employed in the connector to introduce logics for differentiating between general customer support tickets and tickets raised for charge dispute settlement. 

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/5b4e679c-4d21-4539-929b-7dec171719e7"> <br>
<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/98d5e227-a3a2-475d-8623-e0f99187d9e3">

#### Snowflake <br>
Snowflake connector here loads customer data from Salesforce, Zendesk and Stripe into Snowflake’s data lake. Besides serving as a centralized storage point for aggregating customer data from Salesforce, Zendesk, Stripe and the shipping APIs, it also provides analytical and reporting capabilities allowing CosmicCart to gain insights into customer trends, preferences, and behavior.  Snowflake also integrates with Workato to automate the extraction, transformation, and loading (ETL) of customer data from source systems into the data lake. Once a new ticket is raised in ZENDESK, this inserts a new record in the Snowflake database after consolidating the information from the various sources. The ‘Table’ field refers to the database table where the new record will be inserted. The columns refer to the selected columns for the new data to be inserted into.

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/cb5d7b79-28a7-440d-b3b5-b86365c43508">

#### Salesforce <br>
The setup of the above Salesforce connector is designed such that when a customer raises a support ticket in Zendesk, customer service agents can retrieve detailed information about the associated case record in Salesforce. By using the requester's ID from Zendesk as the object ID, you can quickly retrieve the corresponding case details from Salesforce, providing agents with comprehensive information to address customer inquiries or issues effectively.

This integration enables seamless data exchange between Zendesk and Salesforce, allowing for a unified view of customer interactions and support history. Customer service agents can access the details of salesforce cases directly from within Zendesk, streamlining their workflow and enhancing productivity.

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/b604b388-c059-4907-a3c7-811488fd2702">

#### Stripe <br>
The selection of 'Requester ID' from Zendesk and 'Account ID' from Salesforce implies integration between Zendesk and Salesforce within the CosmicCart ecosystem. It suggests that customer support interactions and data from Zendesk are being correlated with customer account information from Salesforce to facilitate charge searches in Stripe. By using the requester's ID from Zendesk and the account ID from Salesforce, customer service agents can quickly search for relevant charge transactions in Stripe based on the customer's support ticket context and associated account information.

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/c2b948e6-c840-46c5-b75f-863ec688e50a">

#### Retrieving charges history in Stripe <br>
Following the search operation, the 'List charges in Stripe' connector is used to retrieve a list of charge transactions from Stripe that match the criteria specified in the previous step. These charge transactions represent payments made by customers on the CosmicCart platform. 
This action enables access to detailed information about individual charges processed through the Stripe payment gateway. The selected fields such as Charge ID, Amount captured, Invoice ID, and Payment method specify the data attributes to be retrieved for each charge listed in Stripe. This setup can prove useful for example in the case of a charges dispute as well in the event of a charges dispute. 

<img width="70%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/c3e20b81-ce9d-486b-add8-9e98b07365fe">


1.  Access to Transaction Details: By retrieving detailed information about charges, including Charge ID, Amount captured, Invoice ID, and Payment method, the setup provides access to essential transaction details. These details can be crucial when investigating and resolving charges disputes.

2. Invoice Reference: The presence of the Invoice ID allows for easy reference to the billing details associated with the charge transaction. In the event of a dispute, having the invoice reference enables customer service agents or finance teams to quickly locate and review relevant billing information.

3. Payment Method Identification: The Payment method field provides insight into the payment method used by the customer for the charge transaction. This information can be valuable when verifying payment authenticity and addressing discrepancies related to payment methods.

4. Integration with CosmicCart Systems: The retrieved charge information can be integrated with other systems within the CosmicCart architecture, such as Salesforce CRM. This integration facilitates a holistic view of the customer's transaction history and interactions, aiding in dispute resolution by providing context and supporting evidence.

#### Connecting to REST API of a shipping application (e.g Ninjavan) <br>
We connect to the REST API of Ninjavan in order to interact with it. In Workato, the action to be used here is the ‘Send request via HTTP" action’. The purpose of integrating with the shipping platform API is to retrieve shipping information for the order. This action queries the platform's API to obtain details such as shipping method, delivery address, and shipping cost. The ‘GET’ method is used for retrieval of data from the web APIs. Shipping-related information can prove useful in the event of providing support to queries that require this information.

Based on Ninjavan’s API endpoint call for integration process function, the API URL for data retrieval is https://api-sandbox.ninjavan.co/sg. The data returned by the Ninjavan API here is of a JSON data type as well. 

<img width="55%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/ad24209d-6620-4d14-885d-c86dc4aa2fd9">

#### Connecting to REST API of Amazon RDS (Database management system)
MySQL connector is used for connecting to the Amazon RDS via Amazon account credentials. A new row (record) is inserted into the Amazon database management system whenever a new ticket is raised.

<img width="55%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/28e3830f-8b51-4396-aecd-c9d1bbda26b2">

‘Host’ refers to the location of the MySQL server to connect to. The host specifies the network address or hostname of the server where the MySQL database is hosted.

<img width="55%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/6509d406-7e7d-4031-a70a-09d04c302421">

Username and password refers to the account details of the Amazon RDS database management platform. Database refers to the name of the MySQL database to connect to.

<img width="55%" src="https://github.com/bayyangjie/Foundation-to-Python-for-AI/assets/153354426/22adf8e4-c169-4e0a-a9c5-961b90b7dcd4">

#### Function of AMAZON RDS:
1.	Data Integration: Workato helps to set up data pipelines or integration workflows to extract data from Salesforce, Stripe, Zendesk, and other sources, perform the necessary data transformations, and load it into Amazon RDS database. This allows consolidation of data from multiple sources into a single database for analysis, reporting, or other purposes.

2.	Data Storage: Once data from Salesforce, Stripe, Zendesk, and other applications is loaded into Amazon RDS, it is stored within the database according to the required schema. It provides features such as data encryption, automatic backups, and scalability to ensure the security and availability of the loaded data.

3.	Data Analysis and Reporting: Amazon RDS can then be used in conjunction with analytical tools or business intelligence platforms to analyze and report on data from Salesforce, Stripe, Zendesk, and other sources. By querying the data stored in Amazon RDS database, it allows users to gain insights into customer behaviour, transaction patterns, support ticket trends, and more.

### Future enhancements
1.	Real-Time Data Integration:
•	Implement real-time data integration capabilities to ensure that the customer data lake is continuously updated with the latest information from source systems.
•	Utilize change data capture (CDC) techniques to capture and replicate only the changed data from source systems to minimize latency and improve data freshness.
2.	Automated Data Transformation and Enrichment:
•	Enhance data loading workflows with automated data transformation and enrichment capabilities to standardize data formats and enrich it with additional contextual information.
•	Utilize machine learning algorithms to automatically categorize and tag data, derive insights, and identify relationships between different datasets for more meaningful analysis.
3.	Predictive Analytics for Customer Service Optimization:
•	Implement predictive analytics algorithms to forecast customer service trends and identify potential issues before they arise.
•	Use historical data to predict customer behavior, such as peak support hours or common support issues during specific periods.
•	Enable proactive measures to address customer concerns and optimize resource allocation within the customer service team.
4.	Natural Language Processing (NLP) for Ticket Analysis:
•	Integrate NLP capabilities to analyze the content of Zendesk tickets and extract key insights.
•	Use NLP to categorize tickets based on the nature of the inquiry or sentiment analysis to prioritize urgent cases.
•	Implement automated tagging and routing of tickets to the most appropriate agent or department based on ticket content.
5.	Chatbot Integration for Self-Service Support:
•	Integrate chatbot platforms with Zendesk to provide self-service support options to customers.
•	Develop AI-powered chatbots capable of answering common customer queries and guiding users through troubleshooting steps.
•	Enable seamless escalation to human agents when chatbots are unable to resolve issues, ensuring a smooth transition between automated and human support.
