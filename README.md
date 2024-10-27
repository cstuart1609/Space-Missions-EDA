# Exploratory Data Analysis on Real Time Space Missions Dataset 


## Project Brief

The Space Missions dataset encompasses a comprehensive record of global rocket launches spanning from 1957 to 2022. This dataset serves as a rich resource for analysing the evolution of space missions over time, providing insights into various factors influencing launch trends. My project aims to conduct an Exploratory Data Analysis (EDA) on this dataset, employing a systematic approach to data processing and visualisation.

## Table of Contents
1. [Project Aims](#project-aims)
2. [Skill Development & Enrichment](#skills-development-&-enrichment)
3. [Data Dictionary](#data-dictionary)
4. [Entity Relationship Diagram](#entity-relationship-diagram)
5. [Data ETL in Power Query](#data-etl-in-power-query)
6. [Exploratory Data Analysis and Database Creation with SQL](#exploratory-data-analysis-and-database-creation-with-sql)
7. [Bonus EDA and Data Commentary](#bonus-eda-and-data-commentary)
8. [Data Visualisations in Power BI](#data-visualisations-in-power-bi)
9. [Project Criticisms](#project-criticisms)
10. [Conclusions and Review of Outcomes](#conclusions-and-review-of-outcomes)


## Project Aims

I will perform initial ETL (Extract, Transform, Load) operations on the dataset using Power Query to prepare it for further analysis. This preparation is crucial for ensuring data integrity and consistency, allowing for meaningful insights to be derived. This dataset is not my own and may not present the data in way I deem efficient or intuitive, so being able to manipulate how the data is stored is a vital step.

Leveraging Generative AI, I created a new table that serves as a lookup table for the companies and agencies behind the rocket launches. This additional layer of data is vital for understanding trends over time based on the volume and success of launches, specifically examining how these factors vary according to whether the launch was funded and performed by public or private sector entities.

I aim to explore global trends in private spaceflight, as well as the industry in general. While the USA boasts a robust and established private sector in the space industry, it is essential to identify which other countries are emerging in this field. Understanding these trends will provide insights into potential investment opportunities and the global landscape of the space industry.

I am also curious to see how investment, public interest and broader geopolitcal events influence the development of the space industry. It is important to identify such patterns to ensure that short-term events and political cycles do not inhabit the ability of our species to explore the unknown.


## Skill Development & Enrichment

Throughout this project,  I am focusing on developing essential skills in data analytics and visualisation. My prior experience with RStudio during university, where I analysed similar datasets, will serve as a solid foundation as I embark on this project. Although I am not directly applying these skills here, I believe my background will enable me to achieve my goals more efficiently.

For the ETL process, I chose to utilize Excel and Power Query due to their user-friendly interface and my familiarity with these tools. This will facilitate effective data cleaning and transformation. I plan to conduct the EDA using SQL, specifically with SQLite, enabling me to build datasets from scratch, perform complex joins, and recover additional details effectively.

I am particularly eager to enhance my visualisation skills using Power BI. While I have previously used Excel and ggplot2 in RStudio, I believe that Power BI's capabilities will allow me to create more sophisticated and interactive visualizations. I anticipate that my existing knowledge will make the transition to Power BI more intuitive.


## Data Dictionary

Find an explanation of each column within the tables of the dataset (where columns appear more than once, they are not repeated)

* __space_missions__:
    - _company_: Short name code of launching agency (e.g., NASA, SpaceX)
    - _location_: omprehensive launch location, detailing the launch pad, launch complex, region, and country
    - _year_: Year of launch
    - _LaunchTime_: 24hr formatted time of launch
    - _Rocket_: The name of the rocket employed for the launch (e.g., Falcon 9, Atlas V)
    - _mission_status_: Numeric representation of the mission outcome (0 for Failure, 1 for Success)
    - _rocket_status_: Text string indicating whether the rocket used in the launch is still in active use or has been retired
    - _price_: price: The cost associated with the launch (this field is deemed redundant for the analysis)
    - _mission_: The full name of the mission, initially serving as the primary key for identifying each unique launch
 
* __space_agencies__:
    - _full_name_summary_: The full official name of the launching agency (e.g., National Aeronautics and Space Administration)
    - _country_: The country where the agency or company operates (e.g., 'Joint' for agencies operating across multiple countries)
    - _sector_: Classification of the agency as either publicly or privately operated
 
* __space_locations__:
    - _launch_geo_: This field indicates which geographic region the mission occured - the default is the Country of launch, although some launches took place in neutral zones such as Oceans/Seas
    - _initial_country_: Similar to the launch_geo field, this field is an intermediate field which captures the highest order in the location field. As explored later in my analysis, this was not suitable for directly contributing launches to geographical regions
 

## Entity Relationship Diagram

![ERD](https://github.com/user-attachments/assets/6caa00a5-9412-441f-b480-40817ef55f39)

The database behind this project consists of 3 tables - space_missions, space_agencies, space_locations - the first table being the original dataset downloaded from Kaggle. The latter two tables were of my own design with the second presenting deeper analysis into each agency and the location table presenting a format that aids the geoegraphical analysis I will perform. 

'Mission' is the primary key in both missions and locations linking the tables in a one-to-one relationship. Meanwhile, the agencies table primary key is company with a one-to-many relationship with both the missions and location table. This dataset has a relatively simple structure with the agencies table acting as a look-up table, with both missions and locations as indexed tables - albeit a different variants of the same data.


## Data ETL in Power Query

The ETL process is conducted within Excel, a choice made due to the dataset's relatively small size, which allows for effective management without overwhelming computational demands. After importing the CSV file into Power Query, I meticulously examined the dataset for errors. A notable issue identified was the datetime field erroneously displaying all years as 30/12/1899. This anomaly was resolved, removing the date aspect, as it bore no significance for the subsequent analysis.

I imported the CSV directly into Power Query and scanned the data very quicky to identify any burning fires. Note the error in the date field with all years showing as 30/12/1899, as there is already a year field, I shall simply remove this data as it is unimportant to my analysis.

![image](https://github.com/user-attachments/assets/01295f51-1176-45b0-8682-b1303dae8082)

I am now happy with the data and suitably happy that it is ready to queried consistently and logically. I saved this table as space_missions.csv.

![image](https://github.com/user-attachments/assets/3b9e617c-ae39-4d62-8608-8f0000ffdd45)

Before moving onto the next stage of this project, I wanted to create a second dataset so that I could have a database with multiple tables in order to perform joins. Returning to my goal of this project, to evaluate trends in public vs private sector spaceflight, I needed to create a dataset which could classify missions as either public or private sector. 

    For exclusively this part of the project, I utilised generative AI to streamline a specific part of the data preparation process—creating a table of existing company names and classifying them as public or private sector. By automating this step, I was able to focus more on the deeper analysis and insights, ensuring a more efficient workflow. AI tools were used responsibly and did not influence the core data analysis, but instead acted as a means to enhance productivity and accuracy during the initial data collection phase. To ensure the resultant table returned the exact index of companies as the initial data set, I used an EXACT function which confirmed consistency across the two datasets. I saved this table as space_agencies.csv.


## Exploratory Data Analysis and Database Creation with SQL

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


# Bonus EDA and Data Commentary

I thought it might also be interesting to understand the distribution of launches globally and further, how this has trended over time. Within my visualisation stage, I shall hopefully create a heatmap to show location data, as well as slicer/sliders to show time trends on a map. For places like Kazakhstan, I could map mission data in its own graph to show if the break-up of the USSR impacted launch frequency for example.

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

As my final piece of exploratory data analysis, I want to identify which agency launched the most missions per year. This will be interesting to see how launch volumes have changed over time and also the change in environment of leading space agencies/companies.

![Query5](https://github.com/user-attachments/assets/a5bf0c0f-a081-4463-8a59-0e64879c36d7)

This was an incredibly insightful result, despite the slightly complex query. Notice the dominance of the Soviet agency (RSVN USSR) between 1963-1991, with the end of this dominance coincidicing directly with the dissolution of the Soviet Union that same year. Also note the 2000s when the highest launching agency reached double digits just twice, reflecting my hypothesis that the slow retirement of the space shuttle and wider economic factors contributed to a downturn in spaceflight investment, political ambition and concerns over safety.


## Data Visualisations in Power BI

The next and final practical stage of this project is visualisation the data that I have created, cleaned, modelled and briefly reviewing. This will be carried out within Power BI Desktop where I used a ODBC driver to import data from space_missions database on SQLite. Rather than including links to the visuals themselves, I will instead include screenshots of the graphic and then include a brief explanation - this is due to the license I have for Power BI, but also as it better reflects previous work in this project.

Prior to this project I had little experience with this software, although was accustomed with complex visualisations via Excel, the power query function, as well as writing DAX expressions to model data further within already imported data. It is a very intuitive piece of software in my opinion with a wide array of informative pieces online and within the product itself, however I do think it is important to have a good understanding of whatever data you are dealing with in this case. 

At this stage, I should explain a late hurdle that I ran into, as well as an underlying issue within the way I modelled my data based on an incorrect assumption. During my initial planning of the data, I assumed that the mission name would be a unique value for each row and as such used it as my index for both the space_missions and space_locations tables. I made this assumption because I felt there would be a standardised naming convention adopted by each agency and while this was the case for the majority of launches, there were a number of exceptions particularly when the launch was an early stage test for a new rocket - for example, there were four identical mission_names for early stage launches for the SpaceX Starship (see below).

![image](https://github.com/user-attachments/assets/4b4159dd-8696-4c6e-aaf5-09a84628cf9c)

I had a choice of solutions to create a unique index/ID field, each with various implications; the first, creating an ID from combining tables - for example combining mission_name with year - was impossible due to some identically named launches within the same year. My only alternative now was index the data with a brand new mission_id field, indexed by number. This is not something I was able to do within Power BI because I needed to ensure that the unique ID was assigned to the same launch and it was impossible for me to ensure that both the space_missions and space_locations were ordered identically. Instead I had to create a new database with the mission_id field added to my initial downloaded data from Kaggle, retreading my steps of creating the space_locations table for this. Although this felt like an annoying backward step, it was an important one to ensure data integrity and accuracy when visualising the data. Below is an updated ERD to show the inclusion of the mission_id field:

![image](https://github.com/user-attachments/assets/7a4b163d-9514-4444-bf72-3cce5f6b3f61)

I then ensured that this changed had worked, by ensuring Power BI recognised a one-to-one relationship between mission_id.space_missions and mission_id.space_locations.

As outlined in my project brief and reflecting the work done within SQL, were a number of key visuaulisations I needed to create:
* Trend of overall spaceflights over-time as well as a breakdown of flights by sector
    - in order to draw conclusions on growth of private sector and impact of real-world conditions on Government-funded spaceflight.
* Insight into composition of overall flights by Sector, as well as insight into the Country of origin for the agency faciliating the launch
    - also shows which sectors have contributed to the most flights and also which countries are chief proponents of private spaceflight.
* Mapping of flight data and mission counts by launch geography and also agency country of origin
    - to create a powerful and clear picture of which countries have contributed the most to space launch numbers
* Bonus is to see if I can show how flights by location have changed over time
    - to see if the above has changed over time and in relation to any changes in composition of pulic and private sector

### Count of Missions by Launch Geography

![image](https://github.com/user-attachments/assets/9088187b-35cd-4948-be86-78c612a8aed5)

This visualization highlights that the United States and Russia have dominated space launches, with Kazakhstan following due to its historical role as a USSR launch site. China and French Guiana also feature significantly, showcasing the contributions of both national and international (such as ESA) space programs.

### Geographical Mapping of Launch Density by Geo

![image](https://github.com/user-attachments/assets/a6462438-c446-450a-b7c7-c594bc0a59e5)

This dashboard shows the results of the previous visualisation, mapped geographically - for regions like the Yellow Sea, there are smaller spotlight graphics to show further detail. It is interesting to note that no single launch has taken place in continental Europe, in contrast with the number of European countries that have facilitated flights. With the exception of a few launches in the Barents Sea and Oceania, many historic flights have taken place close to the Equator - This spatial concentration near the equator relates to orbital mechanics, where lower latitudes offer energy efficiencies for launch vehicles.

### Trend of public and private sector space flights launched between 1957-2022

![image](https://github.com/user-attachments/assets/11ebdc6e-0879-4625-bc08-091379b4a484)

This graph shows how the number of yearly space flight has changed over history. Some interesting elements include the steady but slow growth of private space flight until a significant pick-up in the 2010s. There is a significant drop-off of public space flight after it's peak in the 1970s, the steepest decrease occured around the time that NASA shifted focus away from Apollo missions and budget towards the shuttle and ISS projects. Private sector missions began to outnumber public missions from the 1990s onward, particularly after the dissolution of the Soviet Union in 1991. The 2010s marked rapid growth in private sector launches, emphasizing the expanding role of commercial ventures.

![image](https://github.com/user-attachments/assets/3c034036-8491-4ee9-8088-42619b0d3fd6)

Further to the previous visualisation, this graph shows how the composition spaceflight has seen a significant switch towards private sector since the 1990s. while overall flight numbers have recovered from a rapid decrease in the 1980s to their highest ever levels from the 2010s, a combination of both private sector growth but also the emergence of new players such as China and India. Note: Although 2022 shows a decrease, it is worth noting that this is because this dataset cut-off at some point during this period, as opposed to a side-effect of COVID-19 and lockdowns.

### Launching Agencies by Sector

![image](https://github.com/user-attachments/assets/a98aaf4b-8a22-447f-8107-47e14b9ab0fa)

A sunburst chart provides a detailed breakdown of launches by country and sector. The United States shows a significant private-sector contribution, while launches from Russia, the Soviet Union, and China are predominantly state-funded. This difference reflects broader economic systems, with greater private industry involvement in the U.S.

![image](https://github.com/user-attachments/assets/9a29656c-4d89-4b0e-91f4-f3d4b8d0980e)

A deeper look at launching agencies reveals that most are government-backed. To aid readability, countries with a single agency are grouped as "Other." Notably, China has multiple private agencies, indicating an emerging private sector, albeit smaller than the government segment. Private joint agencies outnumber government-backed joint agencies, underscoring the collaborative, often commercial nature of joint ventures.

### Further Analysis of Geodata

![image](https://github.com/user-attachments/assets/e826f363-555f-4937-820b-817882750f3b)

This visual is similar to the bubble chart from before - but now uses a heatmap element to show magnitude of launches. It also includes a time slicer which can show the data within a range. Below I will manipulate this to show how launch patterns have changed over the date range. Unfortunately, unlike the bubble map, the filled map variant is unable to show offshore launches.

#### Launches since 2000
![image](https://github.com/user-attachments/assets/48d8e44b-9af8-4df3-ae7d-8ea3ddfcb999)
This shows a significant drop-off in Russian launches, while China and French Guiana show strongly in this set. Note the number of countries with launches taking place, reflecting the presence of numerous launch sites in the modern age.

#### Launches from 1957-1967 
![image](https://github.com/user-attachments/assets/7c61a760-f3ff-4422-add4-23149af466b1)
This range shows the first ten years of the space race, with the US once again leading the way but with significant launches in Russia and Kazakhstan (both part of the USSR at the time). 

#### Launches prior to the dissolution of the Soviet Union
![image](https://github.com/user-attachments/assets/6b18a9e3-83d2-4c69-a218-48bea1cc16d0)
This shows the relative dominance of the USSR prior to its dissolution in 1991. At the time, the space race was truly a two-horse race - since, the field has opened up significantly.

#### Public Sector Launches between 1957-2022
![image](https://github.com/user-attachments/assets/74b01292-ddad-411a-836d-534a6575f165)
This shows a pretty even spread behind a dominant Russia and USSR. Interestingly, China and the USA have very similar flight numbers by the public sector, demonstrating the impressive growth of the Chinese presence in the space race.

#### Private Sector Launches between 1957-2022
![image](https://github.com/user-attachments/assets/83c6575e-8d25-42a8-a58f-e8c97e918c68)
This shows the absolute dominance of the USA in private spaceflight with French Guiana and Japan with a strong showing. 


## Project criticisms

### Data Accuracy
Data accuracy in the AI-generated tables presented significant challenges throughout the project. Several notable errors highlighted these issues, such as the listing of the ABMA (Army Ballistic Missile Agency) as AMBA. This misidentification made it impossible to ascertain its correct sector, leading to potential inaccuracies in my analysis. Additionally, the RAE (Royal Aerospace Establishment) was incorrectly attributed to Brazil instead of its true origin, Great Britain. Such discrepancies not only undermine the reliability of the dataset but also complicate the analysis process by obscuring critical context about the agencies involved.

Despite these challenges, I still maintain that employing Generative AI was the most efficient way to create what has ultimately become a valuable dataset for further analysis. However, this experience has taught me the importance of exercising greater caution when relying on AI-generated content. In the future, I will implement a more rigorous review process for the initial results, taking extra steps to verify the accuracy of the data before proceeding with analysis.

For instance, I encountered similar issues with the ISA, which was mistakenly identified as the Indian Space Agency instead of the Iranian Space Agency. Correcting such errors not only required cross-referencing multiple sources but also necessitated an understanding of the nuanced differences between similarly named organisations. To rectify these inaccuracies, I meticulously revised each table to ensure that the information was correct and reflective of the actual data. This experience underscored the importance of diligence and critical thinking in data validation, particularly when using AI tools to assist with data generation.

![Querycorrection2](https://github.com/user-attachments/assets/d762c83f-d629-4685-8ed4-f18cefa0f900)


### Format/Data type issues
Another error I encountered was of my own doing. This error relates to how I saved and imported the space_locations table as an incorrect unicode. My knowledge of unicodes is limited alothough I tried to consistently use UTF-8 throughout, I must have not done so for this table. I realised this when joining the location and agency tables a receiving null results because the former table had incorrectly stored Armée de l''Air, producing an error instead of the special character. This created a headache and I had to manually UPDATE the table with the correct format. This is something I hope will not be repeated in the future - something I will mitigate by stating and understanding the unicode required for my data at the beginning.

![image](https://github.com/user-attachments/assets/d51b941b-186b-482d-a1e9-5e6e50d4ddf7)

### Naming Conventions
While less critical than issues related to data accuracy, adhering to best practice naming conventions is still an important area for improvement in future projects. In the current dataset, I noticed inconsistencies, such as fields that are capitalised and others that do not utilise underscores (_) for separation. These variations can lead to confusion and make the data less intuitive to navigate.

Consistent naming conventions not only enhance readability but also improve collaboration among team members and facilitate easier integration with other datasets. In later projects, I aim to establish a standardised naming convention from the outset, ensuring that all fields are consistently formatted. This includes using lowercase letters with underscores for separation and avoiding unnecessary capitalisation. By implementing these practices, I can enhance the clarity and usability of the data, ultimately contributing to more efficient analysis and reporting.

### Database Structure
During the data modelling phase of this project, I encountered a significant challenge that necessitated an adjustment to the data structure. I initially assumed that the mission_name field would uniquely identify each space mission, believing agencies would follow standardised naming conventions. However, I found identical mission_name entries for some test launches, particularly with SpaceX’s early-stage flights.

This assumption resulted in duplicate entries in the space_missions and space_locations tables, undermining data integrity. I first tried to create a composite identifier using mission_name and year, but this proved unfeasible due to identical names within the same year. Ultimately, I opted to introduce a unique mission_id field to ensure distinct identification. Implementing this change required revisiting the original dataset before re-importing it to Power BI, as the software could not generate stable unique IDs across multiple tables without a reference. Although this meant starting again, it was essential for maintaining database integrity and enabling accurate relationships in my visualisations.

This challenge highlighted the importance of planning for potential issues with unique identifiers when structuring a database. By addressing this issue, I was able to produce more accurate insights, ultimately strengthening the quality of my analysis.

## Conclusions and Review of Outcomes

### Early reflections
The project aimed to identify and analyse any trends within the entire history of spaceflight, focussing on geography, agency-type and temporal factors. I wanted to see if there had been growth of the spaceflight industry since the space race or whether the fervour of the Cold War created an influx in interest in space that would not be repeated. My intial thoughts were that, although governments were no longer able or willing to dedicate the same budget to spaceflight, the combination of evolution of private spaceflight and emerging players like China would support the growth of the industry as a whole.

* Growth and Shift in Sector Participation
* Geographic Distribution and Strategic Launch Sites
* Impact of Historical Events on Space Activity
* Global Contribution and Emerging Players
* 
