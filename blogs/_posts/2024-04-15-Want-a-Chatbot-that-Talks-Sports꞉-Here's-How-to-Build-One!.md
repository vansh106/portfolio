---
layout: post
title: "Want a Chatbot that Talks Sports? Here's How to Build One!"
sitemap: true
hide_last_modified: true
---

Being an avid football fan, I always deep dive in the nitty gritty details of matches, and keep wondering about the various statistical driven insights for shaping better opinions about teams.

But as the in-depth match/team/player statistics multiplies, it gets harder for humans to manually look for it, compare it or research about it.

Being a curious tech nerd, without a thought I started devising my own Premier League Chatbot for the latest season. But why just for the latest season?
Conventionally, you need to look up on different websites for different queries. And building a simple football oriented GPT wrapper would not suffice the insights of latest season’s . My gen AI chatbot infers upon the latest season’s data.

Dividing he project into simple 5 phase process:

### Phase 1: Data collection
My initial strategy was to scrap data from [fbref.com](http://fbref.com), visualize it and clean it. This could have enabled me to fine tune my model to specific in depth use cases, such as fbref could have provided me intimate data like Expected Goals, (xG, the addition of probabilities of goals from all the chances) etc. 

But I realized I don’t need to do such burdensome scraping for building a prototype. I found API for football data collection through Rapid API. [Football APIs](https://www.api-football.com/)

### Phase 2: Database Design

Building my sports data analysis app, I knew I needed a rock-solid foundation to organize all the information.  That's where a relational database comes in – it's like a filing cabinet for your data, but way more powerful!  I explored a few options and landed on PostgreSQL for its fast querying.

To keep things organized, I designed the database with three main tables: fixtures, team_stats, and league_stats.  The fixtures table tracks upcoming matches,  team_stats stores past performance data, and league_stats helps analyze overall league trends.  This separation makes it easier to find what I need and ensures everything stays neat and tidy.

### Phase 3: Leveraging GPT

To customize your GPT instance, you need to fine tune GPT with a lot of football data and that won’t be effective at all because it will take forever to do that, it will be very costly and moreover, we update our data every week after premier league gameweek. So it’s not the best strategy.
So I came up with a simple solution, generating effective queries for my SQL DB schema using chatgpt. 

Simple prompt engineering algorithm: 

```go
func GenSqlQuery(prompt string) string{
	return fmt.Sprintf(`%s 
							 %s
							 %s
							 This is the database schema, Return SQL query for the data that could be required for inference for below question.
							 NOTE: Column names in the query should be in inverted commas.
							 NOTE: Query should ignore all the NULL values.
							 NOTE: Player names might be in short form so use substring but don't use substring to reduce the name size, Return full row.
							 NOTE: No. of rows in output should be never be more than 20.
							 NOTE: Team names and player names should be case insensitive in query.
							 NOTE: Give out statistics for all the teams or players asked in the prompt.
							 %s`, TeamsSchema, playersSchema, fixturesSchema, prompt)
}
```

Database response to the query from the above output is passed again as a prompt to ChatGpt.

### Phase 4: Wrapping GPT

After everything was planned out, it took me less than a day to complete the server side code. I also log every prompt and corresponding SQL query generated. 

I used [Python Streamlit](https://docs.streamlit.io/develop/tutorials/llms/build-conversational-apps) for frontend. 

I containerized the whole project and deployed in AWS. 

### Phase 5: Cron Job:

Maintaining data accuracy is crucial for any data-driven project. To ensure my sports data remains current, I implemented automated data pipelines. These pipelines utilize scripts to fetch data from relevant APIs.  A server endpoint allows for seamless data integration into the PostgreSQL database. This endpoint can be triggered manually for on-demand updates or configured to run on a recurring basis using AWS cron jobs. Scheduled updates, typically set for weekly execution, guarantee the data remains fresh and minimizes the risk of outdated information impacting analysis.

Results:
![1](/assets/img/plbot/1_2.jpg) ![2](/assets/img/plbot/2_2.jpg) ![3](/assets/img/plbot/3_2.jpg)

### Future scope: 
A lot of features could be added

- Statistical driven predictions.
- Real time matches data streaming pipeline
- More number of seasons data.
- Better algorithm for leveraging AI for data analytics.
- User’s OpenAI key integration if planning to deploy it to public.

> Reach out to me if you want to talk more about it in depth. 
> I am currently looking for a job, so if you can help me out, reach out to me.