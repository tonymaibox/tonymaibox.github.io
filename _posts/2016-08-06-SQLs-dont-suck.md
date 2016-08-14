---
layout: post
title: Not all SQLs suck
---

Not Structured Query Language (SQL), anyway.

For this post, I'll be writing about SQL, it's uses with data/databases, and why/how it doesn't have to be so darn challenging.

<img align="center" src="http://s2.quickmeme.com/img/a9/a90ec1f2a5df27387083d2f097ae4e18858a2de97761f6b15b688551a480ded9.jpg" alt="...">

I'm in the camp that favors or enjoys SQL. I understand SQL is a rather strict and unforgiving language compared to Ruby. But, Ruby was built with the user in mind. Yet, that's not to say SQL isn't, either. After all, the langauge is meant to "speak" to data, specifically databases.

SQL is written for and operates on Relational DataBase Management Systems ([RDBMS](https://en.wikipedia.org/wiki/Relational_database_management_system)). The idea is to present data to the user as relations; a collection of tables with rows and columns. This is key to thinking about and querying via SQL: think tables or grids. The user is looking for rows, columns, or pinpoints of data. The use of this data will be up to the user. SQL doesn't analyze the data, users do. SQL is just a language to access the desired data.

SQL doesn't have to be challenging. I'd like to think that SQL employs a very good sequence of operations akin to how I choose to approach life in general: 
1. **_What do I want?_**
2. **_Where/How do I get it?_**
3. **_Any other details?_**

Before we employ this mantra, let's talk about the four functions of data. A user can input SQL statements or commands to manipulate a database to find or sort data for analysis. At its basic core, SQL or any data for that matter has four functions.

* **C**reate
* **R**ead
* **U**pdate
* **D**estroy

### **Creating** a table is straightfoward and formulaic:

<pre><code data-trim class="sql">
{% sql %}
CREATE TABLE members (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT,
    age INTEGER,
    date DATE,
    lifemotto VARCHAR(max)
    );
{% endsql %}
</code></pre>

|id     |name   |age    |date           |lifemotto  |
|-------|-------|-------|---------------|-----------|
|1      |Me     |70     |August 6, 2016 |Be happy.  |
|2      |You    |70     |August 6, 2016 |I'm Happy. |

In the above code, the "members" table is named and created, opened by open parenthesis "(" followed by the name of columns to be created, and the types of data to be inputed per row in that column. The "Primary Key" makes the id column unique and manageable when it comes to duplicates. Sometimes, the table intentionally holds duplicates, its the PRIMARY KEY that helps differentiate the instances of duplicates - perhaps, which came first in this case as the AUTOINCREMENT command increments the id number of any and all additions to the table. Next, name is inputed with a text string, meaning you using quotations marks when adding to the table (e.g. "John", "Jane", etc.). Age is an integer or number. The date will appear in month, day, year format. "Lifemotto" has a VARCHAR(max) input field which limits the user _up to_ 231 characters. VARCHAR means variable-length characters, so up to a certain size. Using CHAR would mean, the column can only have _exactly_ a set amount of characters set with a number inside parenthesis: CHAR(20).

It's important to note, that when creating the columns of a table in the list format, each column item has to be separated by a comma. For me, this is crucial also because the way I write and approach SQL is in a very structured list format. You'll see in the next section.

### **Reading** a table is like setting goals:

I revisit my life approach here:
1. **_What do I want?_**
2. **_Where/How do I get it?_**
3. **_Any other details?_**

```sql
SELECT *
FROM members
WHERE id = 1;
```

Return will be:

|id     |name   |age    |date           |lifemotto  |
|-------|-------|-------|---------------|-----------|
|1      |Me     |70     |August 6, 2016 |Be happy.  |

In the first line, as all SQL queries begin, I start with the *SELECT* command. The * means all. I'm looking for ALL columns. I'm looking in the *members* table as oppose to any other tables I might have. Specifically, I'm looking for data that includes the id of 1. The *WHERE* command sets a very specific detail for the search and return data.

1. **_What do I want?_** 
  * All columns
2. **_Where/How do I get it?_**
  * "members" table
3. **_Any other details?_**
  * id of 1

Let's query two tables!

```sql
SELECT name, lifemotto
FROM members
JOIN partygoers
ON members.age = partygoers.age;
```

Return will be:

|name   |lifemotto  |
|-------|-----------|
|Me     |Be happy.  |
|Jane   |YOLO!      |

Checklist:

1. **_What do I want?_** 
  * name, lifemotto columns
2. **_Where/How do I get it?_**
  * "members" JOINED by "partygoers" tables
3. **_Any other details?_**
  * the member age data has to match the partgoers age data.

As you can see from the returned data. I asked for just *name* and *lifemotto* data. I wanted to see data from two tables (and I could query more tables with the *JOIN* command): *members* and *partygoers*. And I wanted to find matches for *age*. So, my returned data is a row with name and lifemotto from *members* that matched a row with name and lifemotto from *partygoers*. If the table(s) didn't have the targeted/linking rows, my query could've errored or partially return what I was looking for. Also, if there was no such match for my query, my return would be blank. But, there's a way to search for even partial matches.

There's more advanced *JOIN* options for tables. For instance, my *JOIN* was an implicit *INNER JOIN* which means I was looking for exact and all data that I detailed for.  If a row has *NULL* or blank fields, I wouldn't be able to see that in my return data. If I did a *FULL OUTER JOIN* with another table with relaxed or no *WHERE* specifics, I could then see the partial matches with their *NULL* or blank fields. This is helpful to users who might want to see who or what is missing who or what. Here's a [nice graphic](http://i.stack.imgur.com/1UKp7.png) to help visualize the different *JOIN* commands.

### **Updating** a table:

Let's follow the theme of setting goals here.

```sql
INSERT INTO members
(name, age, date, lifemotto)
VALUES
("John", 71, "August 6, 2016", "I'm gonna pop some tags.");
```

Return will be:

|id     |name   |age    |date           |lifemotto                |
|-------|-------|--- ---|---------------|-------------------------|
|1      |Me     |70     |August 6, 2016 |Be happy.                |
|2      |You    |70     |August 6, 2016 |I'm Happy.               |
|3      |John   |71     |August 6, 2016 |I'm gonna pop some tags. |

Guide list:

1. **_What do I want?_** 
  * Add something
2. **_Where/How do I get it?_**
  * INTO the "members" tables
3. **_Any other details?_**
  * name: "John", age: 71, date: "August 6, 2016", lifemotto: "I'm gonna pop some tags."

Oops, I made a mistake in my age. Let's update my row:

```sql
UPDATE members
SET age = 60
WHERE name = "Me";
```

Return will be:

|id     |name   |age    |date           |lifemotto  |
|-------|-------|-------|---------------|-----------|
|1      |Me     |60     |August 6, 2016 |Be happy.  |

Guide list:

1. **_What do I want?_** 
  * Update something
2. **_Where/How do I get it?_**
  * "members" tables
3. **_Any other details?_**
  * change age to 60, make sure it's on the same row WHERE name is "Me". (The other and more safe way of making the change is to match up the id's incase there's more than one "Me" name, but we only want to change a certain "Me's" age.)

### **Deleting** a table doesn't need a plan:

```sql
DROP TABLE members;
```

Just like dropping a goal, deleting or dropping a whole table won't require or life-goals guidelines checklint. If you need a plan to drop a goal, then you don't have a goal. You have a problem.

That's it for now, just my perspective of looking at the basics of SQL. Below are some links that have helped me thus far.

### Resources
  * [W3School - SQL Basics](http://www.w3schools.com/sql/)
  * [Tutorialspoint - SQL Basics](http://tutorialspoint.com/sql/)
  * [W3Schools - TryIt Editor](http://www.w3schools.com/SQL/trysql.asp?filename=trysql_select_all)
  * [PGExercises - Quizzes, Solutions, Explanations](https://pgexercises.com/questions/basic/)

_If there's anything here that you're confused by or think I'm confused by, please drop a comment. I'm always in the mood to learn._
