# Exploratory Data Analysis on Real Time Space Missions Dataset 


## Project Brief
Space Missions is a dataset which shows a Global rocket launches between 1957-2022. I will performing some initial ETL on the dataset using Power Query before creating a database within DB Browser for SQLite.

Using "" I created a new table which will serve as a look-up table for company/agencies behind the rocket launches. My reasoning behind this is because I wanted to understand trends over time based on volume and success of launches depending on which sector funded and performed the launch.

As a further objective, I am interested in seeing global trends in private spaceflight. While the USA has a very established private sector rocketary industry, I am interested to see which over countries globally are entering the private spaceflight fold and where potential investment in the industry would be well-placed.


## Table of Contents
1. [Project Objectives]
2. [ETL - Extract, Transform, Load]
3. [Querying to Understand Data & Identify Trends]


## Project Aims
In terms of the skills I am developing through-out this project, it would be best to explain my current level. I have previously completed similar projects using RStudio during my University studies, as well as performing a critical analysis on an alternative project of the same dataset. I am not interested in directly using these skills here, although I believe my experience on a previous project will allow me to achieve my goals with less-difficulty.

Instead, I will be using Excel to perform some ETL on the data, where I will make use of my experience using Power Query. To conduct my exploratory data analysis I will be tapping into my SQL skillset, this is something I have developed organically in my professional life where I often query customer and sales data sets. I want to use SQLite as it is different to the SQL I use via Snowflake and in order to build out a dataset from scratch. The reason for this is to A) create tables from scratch and B) to perform joins in order to recover further company and sector detail depending on a range of flights.

The skill that I am looking to develop most in this project is my ability to produce powerful and concise visualisations using Power BI. It is not a package I have used before, however I have used various DAX functions when using Power Pivot within the Excel framework. At work I have created various dashboards using our software "Salesforce" and used ggplot2 during my university projects so I am hopeful that this will prove quite intuitive.

## ETL

Performing my ETL within Excel, note the error in the date field with all years showing as 30/12/1899, as there is already a year field, I shall simply remove this data as it is unimportant to my analysis.

![image](https://github.com/user-attachments/assets/01295f51-1176-45b0-8682-b1303dae8082)

I am now happy with the data and suitably happy that it is ready to queried consistently and logically. I saved this table as space_missions.csv.

![image](https://github.com/user-attachments/assets/3b9e617c-ae39-4d62-8608-8f0000ffdd45)

Before moving onto the next stage of this project, I wanted to create a second dataset so that I could have a database with multiple tables in order to perform joins. Returning to my goal of this project, to evaluate trends in public vs private sector spaceflight, I needed to create a dataset which could tell me which flights were public or private sector. 

For exclusively this part of the project, I utilised generative AI to streamline a specific part of the data preparation process—creating a table of existing company names and classifying them as public or private sector. By automating this step, I was able to focus more on the deeper analysis and insights, ensuring a more efficient workflow. AI tools were used responsibly and did not influence the core data analysis, but instead acted as a means to enhance productivity and accuracy during the initial data collection phase. In order to cross to ensure the resultant table returned the exact index of companies as the initial data set, I used an EXACT function which confirmed consistency across the two datasets. I saved this table as space_agencies.csv.


## Exploratory Data Analysis with SQL

Quick query to find average missions a year.

![image](https://github.com/user-attachments/assets/b6069a78-0eac-4a3a-8683-1f919d72aa88)

Now I am just curious to see which years were above this average - I had to use a UNION function to further include the average entry for comparison purposes. Notice the 19 year gap between 1997 and 2016, likely due to a combination of Shuttle redundancy and economic factors.

![Screenshot 2024-10-17 223656](https://github.com/user-attachments/assets/c4f78402-398e-4100-8775-307a0b806d54)

What I am interested in now I want to see how the composition of launches based on sector changes over time, using a count of total missions. I will use joins to incorporate my generated sector and company information.

![Query2](https://github.com/user-attachments/assets/79ec5af8-6d84-479e-abb8-52a9c4923d55)

After running this query, I was mostly pleased with how the data was returned except for two things:
* In 1957-58 there were no Private sector space flights. Rather than having no entry, for the purpose of further analysis I would like to include them with 0 values to reflect the output highlighted in green.
* Also note the 'Unknown' entry in 1958. This is the only 'Unknown' entry in the dataset and given this occurred over 60 years ago, this is not easy to remediate. As such I want to include an Unknown field for 1958, but not for any other years as this would create more noise.

In order to address the first point, I decided to introduce a series of CTEs to my query to create lookup tables for all combinations of year and sector. I used the union function to append the Public and Private tables together, before joining to the orignal launch data to populate the launch counts. Using the coalesce function, I was able to draw down the launch count or map 0 if no launches occured.

For the second point, I created a further CTE for the Unknown sector, but filtered to only show 1958 to reduce noise as mentioned. Given the size of this dataset, it was quite easy to notice unknown as the only entry, however in a larger dataset, I would further explore how common an occurance unknown was before deciding to filter.

![Query3](https://github.com/user-attachments/assets/99b0c30b-507e-481a-a15d-85c8a7a3d7a9)

I am now very happy with the output and format of output for my breakdown of missions per year per sector.
