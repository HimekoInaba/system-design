Counting things
====================================
1. Youtube views  
2. Monitoring System  
3. Analyse data in real time  
 
Requirements clarification
====================================
1. Users/ Customers:
    Who will use the system?
    How the system will be used?
2. Scale (read and write)
    How many read queries per second?
    How much data is queried per request?
    How many video views are processed per second?
    Can there be spikes in traffic?
3. Performance
    What is expected write-to-read data delay?
    What is expected p99 latency for read queries?
4. Cost (Budget)
    Should the design minimize the cost of development?
    Should the design minimize the cost of maintenance?

Functional Requirements - API
====================================  
The system has to count video view events.  
-> countViewEventr(videoId)  
-> countEvent(videoId, eventType) ("like", "view", "share")  
-> processEvent(videoId, eventType, function) ("sum", "count", "average")  
-> processEvents(listOfEvents)   

The system has to return video views count for a time period.  
-> getViewsCount(videoId, startTime, endTime)  
-> getCount(videoId, eventType, startTime, endTime)  
-> getStats(videoId, eventType, function, startTime, endTime)  

Non functional Requirements
====================================
Primary concerns:  
* Scalable (tens of thousands of video views per second)  
* Highly Performant (few tens of milliseconds to return total views count for a video)  
* Highly Available (survives hardware/network failures, no single point of failure)  
----------------------------------------
* Consistency (CAP theorem: choose Availability or Consistency)  
* Cost (hardware, development, maintenance)  

Start with the data. What we store?  
====================================  
Individual events (every click)  
videoId | Timestamp | ...

Pros:
* Fast writes
* Can slice and dice data however we need
* Can recalculate numbers if needed
Cons:
* Slow reads (need to aggregate every request)
* Costly for a large scale (many events)
-------------------------------------------------------------------------
Aggregate data (e.g. per minute) in real time
videoId | Timestamp | Count

Pros:
* Fast reads
* Data is ready for decision making
Cons:
* Can query only the way data was aggregated
* Requires data aggregation pipeline
* Hard or event impossible to fix errors


Where we store?
====================================
How to scale writes?
How to scale reads?
How to make both writes and read fast?
How not to lose data in case of hardware faults and network partitions?
How to achieve strong consistency? What are the tradeoffs?
How to recover data in case of an outage?
How to ensure data security?
How to make it extensible for data model changes in the future?
Where to run (cloud vs on-premises data centers)?
How much money will it all cost?

SQL database
====================================
![alt text](images/sql_scale.PNG)


















System design

Top K heavy hitters
Examples:
1. Number of likes on youtube video on the last 1 minute/hour/day
2. Youtube trends, popular products, volatile stocks
3. DDOS attack prevention 