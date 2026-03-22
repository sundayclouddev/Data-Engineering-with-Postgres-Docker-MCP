# Data-Engineering-with-Postgres-Docker-MCP
In this project, I’m going to connect Cursor to Docker and PostgreSQL MCPs, build a fully containerized PostgreSQL environment, and use it as the foundation for data exploration and optimization.

**Project Link:** [View Project](http://learn.nextwork.org/projects/mcp-data-engineer1)

**Author:** Sunday Obikoya  
**Email:** princelyob@gmail.com

---

![Image](http://learn.nextwork.org/loving_pink_serene_kea/uploads/mcp-data-engineer1_er-diagram-screenshot)

---

## Introducing Today's Project!

In this project, I’m going to connect Cursor to Docker and PostgreSQL MCPs, build a fully containerized PostgreSQL environment, and use it as the foundation for data exploration and optimization.

PostgreSQL is the relational database system that will store and manage over 40,000 rows of structured data, allowing efficient querying, analysis, and visualization.

Docker is the containerization platform that will be used to create and run a PostgreSQL database inside an isolated, reproducible container environment.

By the end of the project, I will load a large dataset into the database, query and visualize the data, and—as a final challenge—optimize the database’s performance for efficiency and scalability.

### Key tools and concepts

The key tools I used include MCP (Model Context Protocol) to connect my code editor to external tools, Docker for containerizing and managing the PostgreSQL environment, and PostgreSQL itself for database creation, querying, and performance tuning. 

I also used built-in database analysis tools such as EXPLAIN ANALYZE to evaluate query performance and validate optimizations.

Key concepts I learned include how MCP enables natural-language control of infrastructure, Docker fundamentals such as containers, images, and port mapping, PostgreSQL concepts like users, schemas, and indexing, and database performance principles including query planning, execution analysis, and optimization strategies.

### Challenges and wins

This project took 9+ hours to complete due to challenges configuring Docker MCP and PostgreSQL MCP in a local environment. Most of the time was spent resolving PostgreSQL authentication failures, TCP versus socket connection issues, and learning that MCP servers must be launched using uv run --with rather than as global CLI tools.
To resolve this, I corrected the MCP configuration, verified Postgres roles and passwords, ensured the database was accessible via localhost:5432, and confirmed Docker was properly exposed on Windows using named pipes. Cursor was restarted only after validating Docker and Postgres independently.
The most challenging part was understanding Dockerized PostgreSQL authentication and how environment mismatches impact MCP connectivity. The most rewarding moment was successfully connecting Cursor to both MCP servers and gaining full database access. Overall, this was a demanding but highly valuable, real-world project.


### Why I did this project

I did this project today to learn how to run and manage PostgreSQL inside Docker, connect it to Cursor using MCP, and troubleshoot real-world issues involving authentication, networking, and environment configuration. The goal was to gain hands-on experience building a database-backed system rather than following a purely theoretical setup.

Another database skill I want to learn is designing scalable schemas and optimizing query performance, including indexing strategies, query tuning, and building production-ready data pipelines that support analytics and reporting workloads.

---

## Setting Up Docker and UV

In this step, I'll be setting up the development environment I’ll need for the rest of the project. This includes installing the tools required to run containers and manage Python dependencies so I can interact with databases using Cursor’s AI chat.

Specifically, I will create a project folder for this series, install Docker Desktop to run containers, and install uv, a fast Python package manager. I’ll also verify that both installations are working correctly to ensure my environment is ready for building and managing databases.

![Image](http://learn.nextwork.org/loving_pink_serene_kea/uploads/mcp-data-engineer1_8h9i0j1k)

### What's Docker and why containers?

We use Docker containers because they make applications and databases easy to set up, run, and share in a consistent way.

Here’s why Docker is important:

1) Consistency across environments
Docker ensures the same setup works the same way on every machine—no “it works on my computer” problems.

2) Easy setup and teardown
You can start, stop, or reset databases and services in seconds without complex installation steps.

3) Isolation
Containers keep dependencies and configurations separate from your system, preventing conflicts with other software.

4) Reproducibility
Anyone can recreate the exact same environment using the same Docker configuration.

5) Scalability and portability
Docker containers run the same way on local machines, servers, and cloud platforms.

In this project, Docker lets us run PostgreSQL in a clean, controlled environment so we can focus on building, querying, and optimizing the database—without worrying about system-specific setup issues.

### Verifying installations

The tools I set up in my environment were dependencies I set up in my local environment which include Docker Desktop, Python, and uv. 

Docker is required to run containers, Python 3.12 or later is needed to run our MCP servers, and uv (the Python package manager) is used to install the MCP servers. 

Running the --version command for each dependency confirms that they are installed correctly and ready to use.

---

## Connecting MCP Servers

What are we doing in this step?

In this step, I’m going to install MCP servers, which are programs that extend Cursor’s capabilities by allowing it to interact with external tools and services. Without MCPs, Cursor can only provide code suggestions and edits based on the files in the workspace. With MCPs, Cursor can communicate with configured servers, run tasks, and access additional functionality, making it far more powerful and context-aware.

To do this, I will initialize a Python project using uv, configure Cursor to use both MCP servers, and then restart Cursor so the MCPs are activated and ready to use.

### Python project setup

The command  "uv init"  set up a Python project because MCP servers are Python-based and require a properly initialized Python environment to manage dependencies and run correctly. 

The files created include pyproject.toml, which tracks project dependencies and configuration, .gitignore, which specifies files and folders to exclude from version control, README.md for project documentation and instructions, and main.py, which serves as an example Python script and entry point for the project.

### Installing Docker MCP

![Image](http://learn.nextwork.org/loving_pink_serene_kea/uploads/mcp-data-engineer1_mcp-config-screenshot)

---

## Building My Database

In this step, I am using Cursor together with the Docker and PostgreSQL MCPs to set up and work with a database environment. 

I will prompt Cursor to create a PostgreSQL container using the Docker MCP, configure database users with the PostgreSQL MCP, load demo data into the database, and finally visualize the database to explore its structure and contents.








![Image](http://learn.nextwork.org/loving_pink_serene_kea/uploads/mcp-data-engineer1_postgres-container-screenshot)

### Database security

Every PostgreSQL database comes with a default superuser, typically named postgres, which has full administrative privileges over the entire database system. I created an application user to handle all operations specific to our application, such as reading and writing data, without using the superuser account. This is a security best practice because it limits the potential impact of a security breach—if the application user’s credentials are compromised, the attacker cannot access or modify other databases or perform administrative tasks.

### Loading demo data

I loaded the e-commerce demo data by running the following command in Cursor’s terminal:
Get-Content demo_data.sql | docker exec -i pg-local psql -U app -d demo
This command inserts the contents of the demo_data.sql file into the PostgreSQL database. The data contains sample e-commerce records representing typical application usage, including users, transactions, and related entities. This allows us to test queries and application functionality without impacting real production data.

What this command does:
Get-Content demo_data.sql reads the SQL file containing the demo data.
The pipe (|) sends the contents of the file directly to the PostgreSQL container.
docker exec -i pg-local psql -U app -d demo executes the SQL statements inside the running PostgreSQL container as the app user, loading the data into the demo database.


### Verifying and visualizing the database

I verified my database by asking Cursor to generate a Mermaid ER diagram from the PostgreSQL schema. The Mermaid ER diagram shows tables as entities, columns as attributes within each table, and relationships as connections between tables based on primary and foreign keys. 

This visualization made it easy to confirm the database structure and validate how customers, products, orders, and order items are related.



![Image](http://learn.nextwork.org/loving_pink_serene_kea/uploads/mcp-data-engineer1_er-diagram-screenshot)

---

## Performance Audit

In this bonus challenge, I’m going to perform a database performance audit, which involves analyzing how efficiently the PostgreSQL database processes queries and identifying slow or expensive operations. 

Data Engineers or Database administrators care about this because slow queries in production can degrade application performance, increase infrastructure costs, and negatively impact user experience. This optimization helps by identifying bottlenecks, recommending indexes or query improvements, and ensuring the database can scale reliably under real-world workloads.

### Audit findings

Cursor’s audit analyzed the database by running EXPLAIN ANALYZE on key queries to measure execution time, planning time, scan types, and cache usage. It found bottlenecks in query performance caused primarily by missing indexes, which resulted in sequential scans on the orders and order_items tables and full table scans during joins and lookups as the data volume grows.

The recommendations included adding indexes on orders.customer_id, order_items.order_id, and order_items.product_id, which would significantly improve join performance, reduce full table scans, and ensure queries continue to perform efficiently as the dataset scales. The audit also noted that while current execution times are acceptable, query planning overhead and future data growth could lead to performance degradation if these optimizations are not applied.



![Image](http://learn.nextwork.org/loving_pink_serene_kea/uploads/mcp-data-engineer1_performance-screenshot)

### Performance gains

I implemented an index on the orders.customer_id column to optimize customer-based lookups. After creating the index, performance improved by dramatically reducing the number of rows scanned—from the entire orders table to only the matching rows for a specific customer.

The execution plan changed from a sequential scan to a bitmap index scan, which means PostgreSQL now uses the index to quickly locate relevant rows instead of scanning all 10,000 records. 
Although execution time was slightly higher on this small, cached dataset, the query cost dropped by nearly 10× and unnecessary row scanning was eliminated. 

As the table grows, this index will deliver clear and significant performance gains by keeping query execution efficient at scale.

---

---
