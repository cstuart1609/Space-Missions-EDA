# Exploratory Data Analysis on Real Time Space Missions Dataset 


## Project Brief

The Space Missions dataset encompasses a comprehensive record of global rocket launches spanning from 1957 to 2022. This dataset serves as a rich resource for analysing the evolution of space missions over time, providing insights into various factors influencing launch trends. My project aims to conduct an Exploratory Data Analysis (EDA) on this dataset, employing a systematic approach to data processing and visualisation.

## Table of Contents
1. [Project Objectives]
2. [ETL - Extract, Transform, Load]
3. [Querying to Understand Data & Identify Trends]


## Project Aims
I will perform initial ETL (Extract, Transform, Load) operations on the dataset using Power Query to prepare it for further analysis. This preparation is crucial for ensuring data integrity and consistency, allowing for meaningful insights to be derived. This dataset is not my own and may not present the data in way I deem efficient or intuitive, so being able to manipulate how the data is stored is a vital step.

Leveraging Generative AI, I created a new table that serves as a lookup table for the companies and agencies behind the rocket launches. This additional layer of data is vital for understanding trends over time based on the volume and success of launches, specifically examining how these factors vary according to whether the launch was funded and performed by public or private sector entities.

I aim to explore global trends in private spaceflight, as well as the industry in general. While the USA boasts a robust and established private sector in the space industry, it is essential to identify which other countries are emerging in this field. Understanding these trends will provide insights into potential investment opportunities and the global landscape of the space industry.

I am also curious to see how investment, public interest and broader geopolitcal events influence the development of the space industry. It is important to identify such patterns to ensure that short-term events and political cycles do not inhabit the ability of our species to explore the unknown.

## Skill Development and Enrichment

Throughout this project, I will focus on developing several key skills. My previous experience includes completing similar projects using RStudio during my university studies, where I conducted critical analyses of the same dataset. Although I am not directly applying these skills here, I believe my background will enable me to achieve my goals more efficiently.

For the ETL process, I will utilise Excel and Power Query, leveraging my familiarity with these tools. To conduct the exploratory data analysis, I will apply my SQL skill set, which I have developed organically in my professional life by querying customer and sales datasets. I have chosen to use SQLite as it differs from the SQL I typically work with via Snowflake, allowing me to build a dataset from scratch. This choice will enable me to:

* Create tables from scratch.
* Perform joins to recover additional company and sector details based on various flights.

The primary skill I am keen to develop in this project is the ability to produce powerful and concise visualisations using Power BI. While I have not previously used this package, I have experience with various DAX functions in Power Pivot within Excel. My work experience includes creating dashboards using Salesforce, and I have also utilised ggplot2 via RStudio during my university projects. I am hopeful that this familiarity will make the transition to Power BI intuitive.


## Data Dictionary

Find an explanation of each column within the tables of the dataset (where columns appear more than once, they are not repeated)

* __space_missions__:
    - _company_: Short name code of launching agency
    - _location_: Full launch location commonly composed of launch pad, launch complex, region and country
    - _year_: Year of launch
    - _LaunchTime_: 24hr formatted time of launch
    - _Rocket_: Name of rocket used in launch
    - _mission_status_: Numeric value representing mission status as 0: Failure and 1: Success
    - _rocket_status_: Text string indicating whether the rocket used in the launch is still in active use or has been retired
    - _price_: Cost of the launch (redundant field)
    - _mission_: Full name of the mission, used as primary key
 
* __space_agencies__:
    - _full_name_summary_: Full name of launching agency to explain and identify more information around the agency itself
    - _country_: The country which operates the agency or company - when multiple countries are behind an agency, such as European Space Agency, this will be filled as 'Joint'
    - _sector_: This column indicates whether the agency is privately or publically operated.
 
* __space_locations__:
    - _launch_geo_: This field indicates which geographic region the mission occured - the default is the Country of launch, although some launches took place in neutral zones such as Oceans/Seas
    - _initial_country_: Similar to the launch_geo field, this field is an intermediate field which captures the highest order in the location field. As explored later in my analysis, this was not suitable for directly contributing launches to geographical regions
 

## Entity Relationship Diagram

![ERD](https://github.com/user-attachments/assets/6caa00a5-9412-441f-b480-40817ef55f39)

The database behind this project consists of 3 tables - space_missions, space_agencies, space_locations - the first table being the original dataset downloaded from Kaggle. The latter two tables were of my own design with the second presenting deeper analysis into each agency and the location table presenting a format that aids the geoegraphical analysis I will perform. 

'Mission' is the primary key in both missions and locations linking the tables in a one-to-one relationship. Meanwhile, the agencies table primary key is company with a one-to-many relationship with both the missions and location table. This dataset has a relatively simple structure with the agencies table acting as a look-up table, with both missions and locations as indexed tables - albeit a different variants of the same data.


## Data ETL - Extract, Transform, Load

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

## Data Visualisations

The next and final practical stage of this project is visualisation the data that I have created, cleaned, modelled and briefly reviewing. This will be carried out within Power BI Desktop where I used a ODBC driver to import data from space_missions database on SQLite. Rather than including links to the visuals themselves, I will instead include screenshots of the graphic and then include a brief explanation - this is due to the license I have for Power BI, but also as it better reflects previous work in this project.

Prior to this project I had little experience with this software, although was accustomed with complex visualisations via Excel, the power query function, as well as writing DAX expressions to model data further within already imported data. It is a very intuitive piece of software in my opinion with a wide array of informative pieces online and within the product itself, however I do think it is important to have a good understanding of whatever data you are dealing with in this case. 

At this stage, I should explain a late hurdle that I ran into, as well as an underlying issue within the way I modelled my data based on an incorrect assumption. During my initial planning of the data, I assumed that the mission name would be a unique value for each row and as such used it as my index for both the space_missions and space_locations tables. I made this assumption because I felt there would be a standardised naming convention adopted by each agency and while this was the case for the majority of launches, there were a number of exceptions particularly when the launch was an early stage test for a new rocket - for example, there were four identical mission_names for early stage launches for the SpaceX Starship (see below).

![image](https://github.com/user-attachments/assets/4b4159dd-8696-4c6e-aaf5-09a84628cf9c)

I had a choice of solutions to create a unique index/ID field, each with various implications; the first, creating an ID from combining tables - for example combining mission_name with year - was impossible due to some identically named launches within the same year. My only alternative now was index the data with a brand new mission_id field, indexed by number. This is not something I was able to do within Power BI because I needed to ensure that the unique ID was assigned to the same launch and it was impossible for me to ensure that both the space_missions and space_locations were ordered identically. Instead I had to create a new database with the mission_id field added to my initial downloaded data from Kaggle, retreading my steps of creating the space_locations table for this. Although this felt like an annoying backward step, it was an important one to ensure data integrity and accuracy when visualising the data. 

I then ensured that this changed had worked, by ensuring Power BI recognised a one-to-one relationship between mission_id.space_missions and mission_id.space_locations.

As outlined in my project brief and reflecting the work done within SQL, were a number of key visuaulisations I needed to create:
* Trend of overall spaceflights over-time as well as a breakdown of flights by sector
    - in order to draw conclusions on growth of private sector and impact of real-world conditions on public/Government-funded spaceflight.
* Insight into composition of overall flights by Sector, as well as insight into the Country of origin for the agency faciliating the launch
    - also shows which sectors have contributed to the most flights and also which countries are chief proponents of private spaceflight.
* Mapping of flight data and mission counts by launch geography and also agency country of origin
* Bonus is to see if I can show how flights by location have changed over time

## Project criticisms

### Data Accuracy
Data accuracy of AI-generated table was an issue. A few examples of errors included listing ABMA (Army Ballistic Missile Agency) as AMBA and subsequently being unable to identify it's sector, as well as RAE (Royal Aerospace Establishment) being incorrectly listed as being of Brazilian in origin instead of from Great Britain. Although I still believe using Generative AI was the most efficient approach to creating what has turned out to be an insightful further dataset, in the future I will take greater caution in this approach and exercise a greater degree of scrutiny when viewing the inital results. Below is an example of how I corrected the data within each table. I had similar issues with ISA (being mistakenly identified as the Indian Space Agency, rather than the Iranian Space Agency).

![Querycorrection2](https://github.com/user-attachments/assets/d762c83f-d629-4685-8ed4-f18cefa0f900)


### Format/Data type issues
Another error I encountered was of my own doing. This error relates to how I saved and imported the space_locations table as an incorrect unicode. My knowledge of unicodes is limited alothough I tried to consistently use UTF-8 throughout, I must have not done so for this table. I realised this when joining the location and agency tables a receiving null results because the former table had incorrectly stored Armée de l''Air, producing an error instead of the special character. This created a headache and I had to manually UPDATE the table with the correct format. This is something I hope will not be repeated in the future - something I will mitigate by stating and understanding the unicode required for my data at the beginning.

![image](https://github.com/user-attachments/assets/d51b941b-186b-482d-a1e9-5e6e50d4ddf7)

### Naming Conventions
Less important but still an area to improve is the constant use of best practice naming conventions. There are capitalised fields and fields not separated with _ that appear in the data which I would like to improve in later projects.
