# AdevintaPatriciaCarmona
This report was created using Python and pandas analysis. Main codding activity was developed in a Jupyter Notebook, where you can find:

1. Data Exploration
2. CTR calculation
3. Zero rate calculation
4. Results people tend to try [incomplete]
5. Sessions lengths calculation [incomplete]


## Data Exploration

First, data analysis with pandas helped to know better type of data, how many records there were, if there were null values, etc.

Based on this info, main KPI's were defined: clicks to calculate CTR and Zero results. 

Main insights from Data Exploration:

- Each session has an id so it isn't an user, instead we can only treat them as sessions. 
    * Exploring session_id serie we can see that there are 68K sessions that got 400K records in the dataset because those sessions could visit different pages during the same session.

- Grouping the dataframe by page_id serie, there is an volume of 176K pages. It makes sense for website where there are unique page for job offers or classifieds. 

- Considerations:
    * visitPage. Since there is an action tagged as visitPage, that is when the users clicks in a link in the results, I considered this as click.

    * visits. To calculate CTR I used number of clicks divided by total amount of visits.

    * page_id. Since there is less page_id volume than sessions_id, I considered each page_id as a page from the website.


## CTR Calculation

At the begginning, I found that checkin serie could help with clicks thanks to the tag "visitPage". Every record tagged as "visitPage" was considered a click. The first attempt was manually, but later I developed a function (>> src/rates) to know which was the overall CTR for those days. Later I created a dataframe to store results day by day and group by group. 

**Issue with timestamp.** This took me some time to know that timestamp serie had 16 units and was causing a problem when casting to datetime because it only considers 14 units. I round to those and then cast to datetime to get each day.

**Group by day.** Once timestamp series was casted, I used resample method to group by day and get CTR daily.

**CTR daily by group.** Finally, I calculated CTR for each group daily and included it in the results dataframe.


## Zero results calculation

I considered that zero results rate was about all checkin null records, because it kept time on each page. So if there was no time on a page, it is a zero result that could be useful to know how many visits didn't end up as they should because users left the funnel or the journey.

First, I calculated the overall zero results divided by total visits and grouped by day. Then I calculated for each group, to know which is performing better and wore. I followed a similar process to the CTR previous. 

## First results day by day

To know which are the results that first tend to try users, I tried to group the initial dataset by day and then page page_id to know how many page_id where visited each day and finally sort by count. That way I would be able to know which were the best pages performing each day.

**Groupby issue.** I had an issue trying to group the dataframe by day and page_id aggregated by count, I had a timestamp issue that tried to solve with different resamble and groupby options. Later when I got the dataframe grouped I didnd't find the best way to sort descending by day. 

## Session length

To know each session length I will grouped the dataframe by day and then by session_id to know each user records daily. I need to take care about  sessions with a duration ~ of 20minutes (or the time that was set to cut down the session if the user was inactive). In the notebook you can check that the difference between the minimun and the maxium timestamp for the user 'b254341e78af2f1a' during a day were 21h. So I would have to group by short periods of time. 