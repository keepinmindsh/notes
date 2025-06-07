# [ConnectionError] / Access to MSSQL on macos - Connection Failed

```
DBAPIError: (pyodbc.Error) ('01000', "[01000] [unixODBC][Driver Manager]Can't open lib 'ODBC Driver 18 for SQL Server' : file not found (0) (SQLDriverConnect)")
```

## Installation 

- [Install the ODBC Driver(macOS)](https://learn.microsoft.com/en-us/sql/connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos?view=sql-server-ver15)

## Connection Issues

- [Help Link](https://github.com/mkleehammer/pyodbc/issues/717)

## Solution 

- Change python version from 3.12.3 to 3.10.17 
  - Loading driver with python 3.12 version not worked, so i downgraded python verstion to 3.10.17
  - If i have a time, i will check the dependany and the siutation 

## References 

> https://pypi.org/project/pyodbc/
> https://github.com/mkleehammer/pyodbc/issues/1082
> https://github.com/mkleehammer/pyodbc/issues/1054