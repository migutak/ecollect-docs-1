# E-Collect DR Documentation
This documentation contains the steps to implement changes for DR
1. Database connection jdbc
2. Reports
3. pm2 service 


## 1. Database connection jdbc
Servers:[xx.xx.xx.71, xx.xx.xx.72, xx.xx.xx.74]\
Change datasource.json with the DR connection details

```
{
  "oracle": {
    "host": "xx.xx.xx.xx",
    "port": "1521",
    "database": "ECOLLECT",
    "password": "xxx",
    "name": "oracle",
    "tns": "",
    "user": "ecol",
    "connector": "oracle"
  }
}
```
Restart ecollect_apis service
```
$ pm2 restart ecollect_apis
```
Confirm service is running by checking the start logs
```
$ pm2 logs ecollect_apis
```

## 2. Reports
Server: [xx.xx.xx.74]\
Change tomcat JNDI Datasource configuration. Edit server.xml with the new database details
```
$ sudo vi /app/tomcat/server/server.xml

<Resource name="jdbc/TestDB" 
		auth="Container" 
		type="javax.sql.DataSource"
        maxTotal="100" 
        maxIdle="30" 
        maxWaitMillis="10000"
        username="javauser" 
        password="javadude" driverClassName="com.mysql.jdbc.Driver"
        url="jdbc:mysql://xxxxx:3306/ECOLLECT"/>
````

Restart tomcat
```
$ sudo ./app/tomcat/bin/catalina.sh stop
$ sudo ./app/tomcat/bin/catalina.sh start
```
