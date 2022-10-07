# JDBC

## Topics

* Connect to and perform database SQL operations, process query results using JDBC API 

## Link

* https://docs.oracle.com/javase/tutorial/jdbc/basics/

## Classi

* P java.sql
* P javax.sql. Contiene estensioni enterprise
    - The DataSource interface as an alternative to the DriverManager for establishing a connection with a data source
    - Connection pooling and Statement pooling
    - Distributed transactions
    - Rowsets

* C java.sql.SQLException (checked). Contiene:
    - int errorCode:  vendor-specific exception code
    - String SQLState:   vendor-specific SQLState
* I java.sql.Connection
* I java.sql.Driver
* C java.sql.DriverManager
* I java.sql.Statement
* I java.sql.PreparedStatement extends Statement
* I java.sql.CallableStatement extends Statement
* I java.sql.ResultSet
* I javax.sql.RowSet extends ResultSet
* I javax.sql.DataSource

