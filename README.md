# MSSQL Prometheus Exporter

This solution generates metrics for SQL result sets and exposes them as Prometheus metrics. It relies on the services Prometheus SQL (dbhi/prometheus-sql) and SQL Agent (dbhi/sql-agent), both available as containers on Docker Hub, to execute and return the SQL result sets.

## Architecture overview

![alt text](https://bytebucket.org/deyvidw/prometheus-mssql-exporter/raw/9d9204e93273ceb19ff6e2a5baf4f4f383c1a1ee/Diagram.PNG?token=5675cce4c155e6d338f4915a4bf4c0a85d9e022f)


## Deployment

1) Run the **SQL agent** service.
```
docker run -d 
--name sqlagent 
-p 5000:5000 dbhi/sql-agent
```

2) Run the **Prometheus-SQL** service.
```
docker run -d 
--name prometheus-sql 
-p 8080:8080 
-v /usr/local/queries.yml:/queries.yml 
-v /usr/local/prometheus-sql.yml:/prometheus-sql.yml 
--link sqlagent:sqlagent dbhi/prometheus-sql 
-service http://sqlagent:5000 
-config prometheus-sql.yml
```

To see the metrics, open up the browser http://localhost:8080/metrics


**References:**

https://hub.docker.com/r/dbhi/sql-agent/

https://hub.docker.com/r/dbhi/prometheus-sql/
