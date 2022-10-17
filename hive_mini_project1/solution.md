Creating Schemas
1
```
create table agent_performance(
sl_no int,
Date date,
Agent_name string,
total_chats int,
avg_res string,
avg_reso string,
avg_rating float,
total_feed int
)
row format delimited
fields terminated by ','
```
2
```
create table agentloggingreport (
sl_no int,
Agent string,
Date date,
login_time string,
logout_time string,
duration string)
row format delimited
fields terminated by ','
tblproperties ("skip.header.line.count" = "1");

```

3) Display all the agent names
```
select distinct(agent_name) from agent_performance limit 5;
Abhishek
Aditya
Aditya Shinde
Aditya_iot
Agent Name

```


4.Find out agent average rating.
```
select avg(avg_rat),agent_name group by agent_name limit 5;

```
 
5.Total working days for each agents
```
select agent_name,count(date) from agent_performance group by agent_name limit 5;

Abhishek        30
Aditya  30
Aditya Shinde   30
Aditya_iot      30
Agent Name      1
```

6. Total query that each agent have taken
```
select sum(total_feed) as total_feed, agent_name from agent_performance group by agent_name limit 5;

0       Abhishek
0       Aditya
153     Aditya Shinde
131     Aditya_iot
NULL    Agent Name

```
7.Total Feedback that each agent have received
```
select sum(total_feedback),agent_name from agent_performance group by agent_name limit 5;

0       Abhishek
0       Aditya
153     Aditya Shinde
131     Aditya_iot
NULL    Agent Name
```

 
8.Agent name who have average rating between 3.5 to 4 
```
select agent_name,avg(avg_rat) as rating  from agent_performance group by agent_name having rating between 3.5 and 4 limit 5;

Boktiar Ahmed Bappy     3.567999982833862
Ishawant Kumar  3.543333347638448
Khushboo Priya  3.703666663169861
Manjunatha A    3.5946666876475017
```

9.Agent name who have rating less than 3.5 
```
select agent_name,avg_rat as rating  from agent_performance where avg_rat < 3.5 limit 5;
Nandani Gupta   3.14
Hitesh Choudhary        0.0
Sanjeevan       0.0
Anirudh         0.0
Shiva Srivastava        0.0
```

10.Agent name who have rating more than 4.5 
```
select agent_name,avg_rat as rating  from agent_performance where avg_rat > 4.5 limit 5;

Ameya Jain      4.55
Mahesh Sarade   4.71
Mukesh  4.62
Saikumarreddy N 5.0
Sanjeev Kumar   5.0
```

11.How many feedback agents have received more than 4.5 average
```
select agent_name, avg(total_feed) as total_feed from agent_performance group by agent_name having avg(total_feed) > 4.5;

Aditya Shinde   5.1
Ameya Jain      7.6
Aravind         7.766666666666667
Ayushi Mishra   10.966666666666667
Bharath         8.233333333333333
Boktiar Ahmed Bappy     10.366666666666667
Deepranjan Gupta        10.4
Harikrishnan Shaji      7.7
Hrisikesh Neogi 12.233333333333333
Ishawant Kumar  6.733333333333333
Jawala Prakash  8.333333333333334
Jaydeep Dixit   10.166666666666666
Khushboo Priya  9.633333333333333
Madhulika G     9.366666666666667
Mahesh Sarade   7.2

```



CONCEPT : Use of SPLIT function 
  If time is given as 00:12:30 and we have to convert it into second 
12.	Select agnet_name,split(avg_res,’:’) as col1 from agent_performance;     
               Ishawant Kumar  ["0","00","54"]
               Jawala Prakash  ["0","02","16"]
                Ayushi Mishra   ["0","01","16"]
 12 Now to find avg weekly response time
 ```
     Abhishek        0.0
     Aditya  0.0
     Aditya Shinde   0.00825925925925926
     Aditya_iot      0.009435185185185185
     Agent Name      NULL

```
13. average weekly resolution time for each agents 
select s.agent_name,avg(col1[0]*3600+col1[1]*60+col1[2])/3600  from(
select agent_name,split(average_resolution,':') as col1  from agent_performance)s group by s.agent_name;
```
     Abhishek        0.0
     Aditya  0.0
     Aditya Shinde   0.00825925925925926
     Aditya_iot      0.009435185185185185
     Agent Name      NULL
```


14.Find the number of chat on which they have received a feedback 
```
select agent_name,sum(total_feed) as total_feed from agent_performance group by agent_name;
Mukesh Rao      5
Muskan Garg     37
Nandani Gupta   308
Nishtha Jain    257
Nitin M 0
Prabir Kumar Satapathy  222
Prateek _iot    107
```

15. Total contribution hour for each and every agents weekly basis 
```
select s.agent,sum(col1[0]*3600+col1[1]*60+col1[2])/3600,s.weekly  from(
select agent,split(duration,':') as col1 ,weekofyear(Date) as weekly from agent_logging_report)s group by s.agent,s.weekly limit 2;

```

16.Perform inner join, left join and right join based on the agent column and after joining the table export that data into your local system.
Inner Join
```
1>select a.agent,a.date,b.total_feed,b.total_charts from agentloggingreport a inner join  agent_performance b on a.agent=b.agent_name limit 5;

Prerna Singh    30-Jul-22       9       11
Prerna Singh    29-Jul-22       9       11
Prerna Singh    29-Jul-22       9       11
Prerna Singh    29-Jul-22       9       11
Prerna Singh    27-Jul-22       9       11
```


Left Join
```
2>select a.agent,a.date,b.total_feed,b.total_charts from agentloggingreport a left join  agent_performance b on a.agent=b.agent_name limit 5;

Agent   Date    NULL    NULL
Shivananda Sonwane      30-Jul-22       1       4
Shivananda Sonwane      30-Jul-22       9       14
Shivananda Sonwane      30-Jul-22       4       5
Shivananda Sonwane      30-Jul-22       18      26

```
Right Join
```
3>select a.agent,a.date,b.total_feed,b.total_charts from agentloggingreport a right join  agent_performance b on a.agent=b.agent_name limit 5;

NULL    NULL    NULL    NULL
Prerna Singh    30-Jul-22       9       11
Prerna Singh    29-Jul-22       9       11
Prerna Singh    29-Jul-22       9       11
Prerna Singh    29-Jul-22       9       11
```
                                                      Exporting hive table into home 
```                                               
$  hive -e 'Select alr.agent,alr.date,ap.total_chats,ap.total_feedback from hive_assignment2.agent_logging_report alr join hive_assignment2.agent_performance ap on alr.agent = ap.agent_name limit 5' > /home/cloudera/hive_Assignment2/inner.join.csv;

$  hive -e 'Select alr.agent,alr.date,ap.total_chats,ap.total_feedback from hive_assignment2.agent_logging_report alr left join hive_assignment2.agent_performance ap on alr.agent = ap.agent_name limit 5' > /home/cloudera/hive_Assignment2/left.join.csv;

$  hive -e 'Select alr.agent,alr.date,ap.total_chats,ap.total_feedback from hive_assignment2.agent_logging_report alr right join hive_assignment2.agent_performance ap on alr.agent = ap.agent_name limit 5' > /home/cloudera/hive_Assignment2/right.join.csv;

```
17. Perform partitioning on top of the agent column and then on top of that perform bucketing for each partitioning.
```
Create table ALR_partition_Bucket
(
sr_no int,
Date date,
Login_time string,
Logout_time string,
Duration string
)partitioned by (Agent string)
CLUSTERED BY (Date) sorted by (Date) INTO 4 BUCKETS
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';


set hive.exec.dynamic.partition=true;
set hive.exec.dynamic.partition.mode=nonstrict; 

hive> insert into table ALR_partition_Bucket partition(Agent) select sl_no,Date,Login_time,Logout_time,Duration,Agent from agent_logging_report;

Create table AP_partition_Bucket
(
sr_no int,
Date date,
Total_chat string,
Average_Response_Time string,
Average_Resolution_Time string,
Average_Rating float,
Total_Feedback int
)partitioned by (agent_name string)
CLUSTERED BY (Date) sorted by (Date) INTO 8 BUCKETS
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';

insert into table AP_partition_Bucket partition(agent_name) select sl_no,Date,Total_chats, Average_Response_Time, Average_Resolution,Average_Rating,Total_Feedback,Agent_name from Agent_performance;
```


