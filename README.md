# IPL-Dataset-Analysis
# Problem Statement - "Sports Basics" is a sports blog company that entered space recently. 
They wanted to get more traffic to their website by releasing a special edition magazine
on IPL 2024. This magazine aims to provide interesting insights and facts for
fans, analysts and teams based on the last 3 years' data.
The chief editor Tony Sharma oversees this publication, and he believes in data
analytics. He reached out to Peter Pandey, a journalist in his team who is a data
savvy cricket enthusiast.

# SQL Query to get the desired result

# Question(1)- Top 10 batsmen based on past 3 years total runs scored
Answer- select batsman, sum(runs) as total_runs from batting_summary 
group by batsman order by total_runs desc limit 10;
# Question(2)- Top 10 batsmen based on past 3 years batting average. (min 60 balls faced in each season)
Answer- select batsman, sum(balls) as total_balls, sum(runs) as total_runs, count(out_no_times) as total_outs, 
round(sum(runs)/count(out_no_times),2) as batting_average from batting_summary 
where out_no_times = 1 group by batsman 
having sum(balls) >= 60 
order by batting_average desc limit 10;
# Question(3)- Top 10 batsmen based on past 3 years strike rate (min 60 balls faced in each season)
Answer- select  distinct batsman, round(avg(strike_rate),2) as avg_strike_rate 
from batting_summary group by batsman 
having sum(balls) >= 60 
order by avg_strike_rate desc limit 10;
# Question(4)- Top 10 bowlers based on past 3 years total wickets taken.
Answer- select p.name as name, sum(b.wickets) as total_wickets from bowling_summary as b 
left join players_summary as p on p.name = b.bowler 
group by name order by total_wickets desc limit 10;
# Question(5)- Top 10 bowlers based on past 3 years bowling average. (min 60 balls bowled in each season)
 Answer- select bowler as bowler_name, round(sum(runs)/sum(wickets),2) as bowler_average 
 from bowling_summary group by bowler 
 having sum(balls) >= 60 
 order by bowler_average desc limit 10;
# Question(6)- Top 10 bowlers based on past 3 years economy rate. (min 60 balls bowled in each season) 
 Answer- select bowler as bowler_name, round(sum(runs)/sum(overs),2) as economy_rate 
 from bowling_summary group by bowler 
 having sum(balls) >= 60 
 order by economy_rate desc limit 10;
# Question(7)- Top 5 batsmen based on past 3 years boundary % (fours and sixes).
Answer-  SELECT batsman AS batsman_name,
CASE WHEN SUM(runs) = 0 THEN NULL ELSE ROUND(SUM(fours+sixes) * 100.0 / NULLIF(SUM(runs), 0),2) END AS boundary_percentage 
FROM batting_summary GROUP BY batsman 
having SUM(fours + sixes) * 100.0 / NULLIF(SUM(runs), 0) <>0 
ORDER BY boundary_percentage DESC LIMIT 5;
# Question(8)- Top 5 bowlers based on past 3 years dot ball %
Answer- select bowler as bowler_name, sum(zeroes)*100/sum(balls) as dot_ball_percentage 
from bowling_summary group by bowler 
order by dot_ball_percentage desc limit 5;
# Question(9)- Top 4 teams based on past 3 years winning %
Answer- with cte_table as( 
SELECT  team_name,  COUNT(*) AS matches_played, SUM(CASE WHEN winner = team_name THEN 1 ELSE 0 END) 
AS matches_won 
FROM ( SELECT team1 AS team_name, winner FROM match_summary WHERE winner IS NOT NULL UNION ALL SELECT team2 AS team_name, winner 
FROM match_summary WHERE winner IS NOT NULL) AS all_teams 
GROUP BY team_name) 
select team_name,  matches_won, matches_played, matches_won*100/matches_played as winning_percentage 
from cte_table  
order by winning_percentage desc LIMIT 4;
# Question(10)- Top 2 teams with the highest number of wins achieved by chasing targets over the past 3 years.
Answer- SELECT team_name, COUNT(*) AS matches_played, SUM(CASE WHEN winner = team_name THEN 1 ELSE 0 END) AS matches_won 
FROM (SELECT team1 AS team_name, winner FROM match_summary WHERE winner IS NOT NULL UNION ALL SELECT team2 AS team_name, winner FROM match_summary 
WHERE winner IS NOT NULL) AS all_teams 
GROUP BY team_name 
order by matches_won desc limit 2;





