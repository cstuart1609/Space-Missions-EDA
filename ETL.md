Performing my ETL within Excel, note the error in the date field with all years showing as 30/12/1899, as there is already a year field, I shall simply remove this data as it is unimportant to my analysis.
![image](https://github.com/user-attachments/assets/01295f51-1176-45b0-8682-b1303dae8082)

I am now happy with the data and suitably happy that it is ready to queried consistently and logically. I saved this table as space_missions.csv.
![image](https://github.com/user-attachments/assets/3b9e617c-ae39-4d62-8608-8f0000ffdd45)

Before moving onto the next stage of this project, I wanted to create a second dataset so that I could have a database with multiple tables in order to perform joins. Returning to my goal of this project, to evaluate trends in public vs private sector spaceflight, I needed to create a dataset which could tell me which flights were public or private sector. 

For exclusively this part of the project, I utilised generative AI to streamline a specific part of the data preparation process—creating a table of existing company names and classifying them as public or private sector. By automating this step, I was able to focus more on the deeper analysis and insights, ensuring a more efficient workflow. AI tools were used responsibly and did not influence the core data analysis, but instead acted as a means to enhance productivity and accuracy during the initial data collection phase. In order to cross to ensure the resultant table returned the exact index of companies as the initial data set, I used an EXACT function which confirmed consistency across the two datasets. I saved this table as space_agencies.csv.

