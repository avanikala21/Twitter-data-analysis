#Topic: Twitter: Summing up the Pulse of the Internet

Data Model: Document (NoSQL)

Target Platform: Azure SQL Multi-Model

Objective/Scope:

● Scrap real-time tweets, Twitter user details, URLs and images off of Twitter

● Compare statistics across tweets and find the most trending type of tweets for a
particular period of time

● Find the top 10 users who have the most tweets and look for any correlation between
the number of tweets and their followers and following

● Perform location-based analysis on trending hashtags across regions (group by location)

● Perform analysis on the top 10 most followed Twitter users to understand what gender,
age group and nationalities they most appeal to Visualizations Tool: Tableau/ Power BI

Implementation

1) A brief description of the implementation process

The aim of our project is to use ETL tools to extract data from twitter, transform the data and load it onto our Azure database. 

1.	First, we wrote a Python script that includes API calls to Twitter to extract tweet data and some tweet details like twitter user, location, hashtags, tweet text and follower count. The script also performs some transformation on the data like data cleaning.

2.	Next we created our SQL server, source blob, destination database, and batch pools.
For the pools, we used a standard A2 windows VM instance with two nodes and two CPUs.

3.	We then incorporated the python script in a batch job (“runpythonscript”) which is essentially the start of our data pipeline. This batch job runs the python script, extracts data from Twitter into data frames, puts the data into CSV files which are then automatically uploaded into Azure blobs (our data source).

4.	Upon successful loading of data into the source, the second step in our pipeline is multiple jobs to copy the data from source (blob storage) to destination (database) using column mapping in Azure.

5.	For the document data model, we linked a Cosmos DB resource instance to the data factory as a destination for the hashtag data.

6.	For the graph data model, we created Nodes and Edges files to be pushed to our SQL destination database.

7.	As a third step in our pipeline, we also added a job to delete all CSV files from the blob once the data is copied into the destination.
We then plan to perform analysis and visualizations on this data using PowerBI.


2) Screenshots which capture key steps in implementing the database and other architecture components

Step 1: We created our own SQL server with the name sqlserversindhu.database.windows.net  

Step 2: We created the source azure storage blob container as tweetblob in the SQL server.

Step 3: We created the destination azure SQL database as db_destination_sindhu in the SQL server.  

Step 4: Next we created a Data Factory to build a pipeline to move the data from source to destination using ETL operations. We set the source of the data factory as tweetblob and destination as db_destination_sindhu and sindhucosmosdb.

Step 5: We then created a batch account and a batch job runpythonbatch to run the python script that extracts twitter data via API.

![image](https://user-images.githubusercontent.com/113712334/233248354-dd5e85a3-ef42-4503-b7d0-7c3d537cbf93.png)

Step 6: We configured a pool runpythonpool with two nodes and two CPUs each of which are small virtual machines that execute the jobs depending on which machine has the bandwidth.
  
Step 7: We created a NoSQL destination database in Azure Cosmos DB as sindhucosmosdb.

Step 8: The services linked to the Data Factory are as shown below:
  
●	Azurebatch1 refers to the batch job runpythonbatch

●	AzureBlobStorage refers to the source storage blob tweetblob

●	AzureSqlDatabase1 refers to the destination database db_destination_sindhu

●	CosmosdbNosql1 refers to the NoSQL database sindhucosmosdb

Step 9: After running the batch job in the data pipeline, the tweetblob looks like this: 

Step 10: We created a data pipeline CopyPipeline_tweettest with 3 major steps to extract, copy data to destination and delete CSV files from source all in sequence.
 
![image](https://user-images.githubusercontent.com/113712334/233248545-04c82883-a2e4-4d11-89d1-b308441c7433.png)

Step 11: We performed destination data validation to ensure data flow was smooth and we can now perform analysis.

RDBMS and Graph Tables:

Document Data on Cosmos:
![image](https://user-images.githubusercontent.com/113712334/233248652-81b2f625-a9d4-4974-92e5-f6210febfb30.png)


Visualizations

1.	Location-based analysis 

We have performed a location-based analysis using Power BI, and have added the location to the map visual, and the number of users as the legend.It provides a visual representation of where your users are located and how many users are in each location.

The outcome we got from this analysis is a better understanding of where users are located and how many users are in each location. We are also able to identify patterns or trends in user behavior across different locations, which can help inform marketing and sales strategies. However, the depth of insights we can gain will depend on the complexity of the analysis and the additional layers of data we incorporate into the visualizations.

![image](https://user-images.githubusercontent.com/113712334/233258004-ef5376fc-7a5f-4516-89be-8a2d4d0cd977.png)

2.	User base on Twitter for each year
We have performed a time-series analysis on the user base of Twitter using Power BI. We have plotted the test date on the x-axis and the percentage growth of users on the y-axis. This type of analysis is useful for understanding how the user base of Twitter has grown or changed over time.

The outcome we can expect from this analysis is a better understanding of the trends and patterns in Twitter's user growth. By analyzing the percentage growth of users over time, we can identify periods of rapid growth or decline and the factors that contributed to these changes. This information can be used to inform business decisions, such as marketing strategies, product development, or investment opportunities. This type of analysis can be a valuable tool for understanding the trajectory of a company or product over time, and making data-driven decisions based on that insight.

![image](https://user-images.githubusercontent.com/113712334/233258042-6e5f19d6-d298-4d90-830a-6053d726b746.png)

3.	Analyze the number of retweets
We  have performed an analysis of the most retweeted tweets using Power BI. We have used the parent tweet ID as the x-axis and the count of tweet IDs as the y-axis. This type of analysis can be useful for identifying the most popular or viral tweets on a specific topic or theme.

The outcome you can expect from this analysis is a better understanding of the tweets that have generated the most engagement and reach on Twitter. By analyzing the parent tweet ID and the count of tweet IDs, we can identify the tweets that have been retweeted the most and understand the content or messaging that resonated with users.

This information can be used to inform social media marketing strategies, as it provides insight into the types of content that are most likely to generate engagement and reach on Twitter. Additionally, by visualizing the data in Power BI,we can create informative and visually appealing presentations to share with stakeholders.

![image](https://user-images.githubusercontent.com/113712334/233258079-b057a688-005a-40f0-b3f1-82a45505d3cf.png)
 
4.	Graph data model for most popular hashtag
We have performed an analysis on the correlation matrix for the most popular hashtag using Power BI. We have used the network navigator visual and have added "_to" as the Source Node and "_from" as the Target Node. This type of analysis can be useful for identifying patterns and relationships between the most popular hashtag in a region.

The outcome you can expect from this analysis is a better understanding of the network of relationships between the most popular hashtag on the platform. By visualizing the correlation matrix in Power BI, we can identify which hashtags are most closely connected to each other, which users are most influential.

This information can be used to inform social media marketing strategies, as it provides insight into the influencers and key players in the platform's ecosystem. By gaining insight into the hashtags that have the most impact on the platform, we can refine our marketing strategies and better engage with our target audience.

![image](https://user-images.githubusercontent.com/113712334/233258114-8bcbd278-84f6-4455-b13d-7d7a3507ad22.png)

 
5.	Top 10 users with highest following
We have analyzed the top 10 most followed users on Twitter using Power BI. The outcome we can expect from this analysis is a better understanding of the distribution of followers among the top 10 most followed users on Twitter. By visualizing the data in a Treemap, we can easily identify which users have the most followers and compare their popularity to other users.

This information can be used to inform social media marketing strategies, as it provides insight into the most popular users on the platform and the types of content that are resonating with Twitter users. By gaining insight into which users have the most followers, companies can refine their social media marketing strategies and better engage with the targeted audience.

![image](https://user-images.githubusercontent.com/113712334/233258149-3e55dafa-51af-459f-a356-68be12a9bc15.png)

