# Exploratory Data Analysis on Real Time Space Missions Dataset 


## Project Brief
Space Missions is a dataset which shows a Global rocket launches between 1957-2022. I will performing some initial ETL on the dataset using Power Query before creating a database within DB Browser for SQLite.

With the use of Generative AI, I created a new table which will serve as a look-up table for company/agencies behind the rocket launches. My reasoning behind this is because I wanted to understand trends over time based on volume and success of launches depending on which sector funded and performed the launch.

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

For this project I am performing my ETL within Excel. While I have experiencing doing so with direct commands in R, I opted to use Excel as it is far more efficient for a project like this, given the relatively small size of the datset. This being said, I am determined to learn Python for situations where Excel is not suitable, I shall explore this in a later project.

I imported the CSV directly into Power Query and scanned the data very quicky to identify any burning fires. Note the error in the date field with all years showing as 30/12/1899, as there is already a year field, I shall simply remove this data as it is unimportant to my analysis.

![image](https://github.com/user-attachments/assets/01295f51-1176-45b0-8682-b1303dae8082)

I am now happy with the data and suitably happy that it is ready to queried consistently and logically. I saved this table as space_missions.csv.

![image](https://github.com/user-attachments/assets/3b9e617c-ae39-4d62-8608-8f0000ffdd45)

Before moving onto the next stage of this project, I wanted to create a second dataset so that I could have a database with multiple tables in order to perform joins. Returning to my goal of this project, to evaluate trends in public vs private sector spaceflight, I needed to create a dataset which could tell me which flights were public or private sector. 

For exclusively this part of the project, I utilised generative AI to streamline a specific part of the data preparation process—creating a table of existing company names and classifying them as public or private sector. By automating this step, I was able to focus more on the deeper analysis and insights, ensuring a more efficient workflow. AI tools were used responsibly and did not influence the core data analysis, but instead acted as a means to enhance productivity and accuracy during the initial data collection phase. In order to cross to ensure the resultant table returned the exact index of companies as the initial data set, I used an EXACT function which confirmed consistency across the two datasets. I saved this table as space_agencies.csv.


## Exploratory Data Analysis with SQL

To get a better understanding of my data, as well as an insight into the scale of launches per year, I performed a quick query to find average missions a year.

![image](https://github.com/user-attachments/assets/b6069a78-0eac-4a3a-8683-1f919d72aa88)

Now I am just curious to see which years were above this average - I had to use a UNION function to further include the average entry for comparison purposes. Notice the 19 year gap between 1997 and 2016, likely due to a combination of Shuttle redundancy and economic factors. As an early hypothesis, it is interesting to note that on either side of this hiatus, there is a marked shift from public to private sector launches.

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

# Bonus EDA

I thought it might also be interesting to understand the distribution of launches globally and further, how this has trended over time. Within my visualisation stage, I shall hopefully create a heatmap to show location data, as well as slicer/sliders to show time trends on a map. For places like Kazakhstan, it might be interesting to map launch data in its own graph to show if the break-up of the USSR impacted launch frequency.

The data I will need for this stage will include Launch Country (parsed from Launch Site), Year and also mission count. Before extraction of Launch Country, the result is very noisy with long strings of launchpad designations, launch complexes and subregions. I need to tweak the Location row. As opposed to standardised text strings like EmployeeId etc., I am unable to use the substr function as there is no consistent start to Country name within each string. I must use the final delimeter.

![image](https://github.com/user-attachments/assets/f20cbbd0-0b02-42fe-9fef-734664c80198)

As I am using SQLite in this project, it is slightly more difficult to perform this function than in PostgreSQL where I could simply use a SPLIT_PART function to categorically split by delim.

In fact, after attempting a number of work arounds, my version of DB Browser for SQLite does not support REVERSE or negative indexing of INSTR functions. As a result, I made the decison to return to the ETL stage and delimit within Power Query and create a new missions table variant this gives a greater focus to location. I will add this new table to the database as space_locations.


* So upon doing this addition ETL I discovered some historical quirks within my dataset. Namely any Missions launched from 'France', amounting to 318 missions. Not a single one of these within the dataset are launched from mainland France (4 from Algeria and 314 from French Guiana). This presented a challenge for me because with my objective of mapping launches geographically I want the location tagged geographically but still want accredit France with the launch - I can do this via the space_agencies table which accredits the launching agency to the Country which operates it. This will acredit some launches instead to 'Joint' but as a summary statistic, I am okay with this.

As a first step, I would parse out the last component of the Location column as 'initial_country'

* A further complication was that the launch location was occassionally not affiliated with a country but instead another geographical region, such as 'Pacific Ocean', 'Yellow Sea' and in one case 'New Mexico'. I also noticed an error where an Iranian launch did not include Iran in the location, instead pulling the launch site in as initial country.

  ![image](https://github.com/user-attachments/assets/03153165-eefa-415b-a728-da995fde0cc3)
  
In order to address both complications, I opted to create a custom column that would transform the outputs for the missions subject to the above challenges. Using a series of IF statements within M Code, I created the launch_geo column which I would use as my country field for geo mapping.

As part of my ongoing improvement of best practice, I formatted the new columns in lower case and with _ instead of spaces for greater efficiency when querying.

![ETL3](https://github.com/user-attachments/assets/b2b1bad7-0d3c-42b9-a051-7a6b0c9f5ffc)

I now saved the new table as space_location.csv which I will now add to my database in DB Browser.
Note: There was a variety of locations within the USA, Russia and China such as Marshall Islands. Although this would have made for a more interesting graphic, I decided to group these as best as possible as it would make for a more powerful visualisations. If was solely interested in mapping the precise location of each launch, this is something I would explore, however this does not fall within this projects objectives. I consider a distinction between this extrapolating launch sites under 'France' due to broader context.

As a result of my extensive ETL on this location data, I am able to extract the information I want with a relatively simple query (note: 21 unique launch geos were returned within the data). 

![Query4](https://github.com/user-attachments/assets/63f8ba21-0b65-4c97-8467-8ede4b8c8836)

As expected, given common knowledge of space exploration, the USA and Russia feature heavily on this list, with a large number from former Soviet Republic Kazahkstan. With launches from French Guiana corresponding with the efforts of the European Space Agency (ESA); China and Europe represent the 'best of the rest'. Given my awareness of space flight trends, India's inclusion in the Top 7 reflects a remarkable and rapid development of their space program in recent years, something I expect to represented visually during my visualisation phase.

I'd now like to do further analysis on launch locations by also including the agency type, agency country of origin and year. The reason for this is to see how often the launch geo corresponds with the agency country of origin and if there is a difference in this trend between public and private sector.

![image](https://github.com/user-attachments/assets/4aa4777b-301f-4da1-83b7-4d9ec049c481)

Although this view offers no summary or analytical value, I think it is quite a nice way to present the full dataset in the context of this project. It also serves as a good base to perform further analysis on.
I would now like summarise the missions based on launch geo and also the sector behind the launch. This is similar to a previous query but splits out missions across sector.

![image](https://github.com/user-attachments/assets/9c998370-5192-4751-b7bc-72cf5e9eff95)

It might be surprising to see the scale of the disparity between public and private sector launches in the USA considering the prevalence of NASA in this industry. As a result of this, now I am curious to find launch geos that are favoured for private launches. I only want locations where both Public and Private launches have occurred and when COUNT(PUBLIC)<COUNT(PRIVATE).

![image](https://github.com/user-attachments/assets/6c21d0e7-9899-4405-a18d-d17303a6a258)

The USA, Japan and French Guiana all have launched an array of missions but tend to be primarily private launches. A naunce here is that many French Guiana launches are operated by Arianespace, a private French company that is the world's first commercial launch service provider and flights are fundamentally a joint venture between the Public and Private sectors. For the purpose of this project, I am happy that these launches are catergorised as Private as it reflects the wider growing involvement of the private sector in spaceflight.

As my finalpiece of exploratory data analysis, I want to identify which agency launched the most missions per year. This will be interesting to see how launch volumes have changed over time and also the change in environment of leading space agencies/companies.

![Query5](https://github.com/user-attachments/assets/a5bf0c0f-a081-4463-8a59-0e64879c36d7)

This was an incredibly insightful result, despite the slightly complex query. Notice the dominance of the Soviet agency (RSVN USSR) between 1963-1991, with the end of this dominance coincidicing directly with the dissolution of the Soviet Union that same year. Also note the 2000s when the highest launching agency reached double digits just twice, reflecting my hypothesis that the slow retirement of the space shuttle and wider economic factors contributed to a downturn in spaceflight investment, political ambition and concerns over safety.


## Project criticisms

* Data accuracy of AI-generated table was an issue. A few examples of errors included listing ABMA (Army Ballistic Missile Agency) as AMBA and subsequently being unable to identify it's sector, as well as RAE (Royal Aerospace Establishment) being incorrectly listed as being of Brazilian in origin instead of from Great Britain. Although I still believe using Generative AI was the most efficient approach to creating what has turned out to be an insightful further dataset, in the future I will take greater caution in this approach and exercise a greater degree of scrutiny when viewing the inital results. Below is an example of how I corrected the data within each table. I had similar issues with ISA (being mistakenly identified as the Indian Space Agency, rather than the Iranian Space Agency).

![Querycorrection2](https://github.com/user-attachments/assets/d762c83f-d629-4685-8ed4-f18cefa0f900)


* Another error I encountered was of my own doing. This error relates to how I saved and imported the space_locations table as an incorrect unicode. My knowledge of unicodes is limited alothough I tried to consistently use UTF-8 throughout, I must have not done so for this table. I realised this when joining the location and agency tables a receiving null results because the former table had incorrectly stored Armée de l''Air, producing an error instead of the special character. This created a headache and I had to manually UPDATE the table with the correct format. This is something I hope will not be repeated in the future - something I will mitigate by stating and understanding the unicode required for my data at the beginning.

![image](https://github.com/user-attachments/assets/d51b941b-186b-482d-a1e9-5e6e50d4ddf7)


