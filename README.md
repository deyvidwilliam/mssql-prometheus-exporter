# MSSQL Prometheus Exporter

This solution generates metrics for SQL result sets and exposes them as Prometheus metrics. It relies on the services Prometheus SQL (dbhi/prometheus-sql) and SQL Agent (dbhi/sql-agent), both available as containers on Docker Hub, to execute and return the SQL result sets.

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
