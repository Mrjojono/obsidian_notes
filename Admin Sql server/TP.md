
CREATE DATABASE bd_test
 ON PRIMARY
 ( NAME='bd_test'
 , FILENAME ='C:\DONNEES\bd_ test.mdf'
 , SIZE = 15360KB 
, MAXSIZE =UNLIMITED
 , FILEGROWTH = 64 MB 
)
 LOG ON
 ( NAME='bd_test_log'
 , FILENAME ='C:\DONNEES\bd_test_log.ldf'
 , SIZE = 8192KB 
, MAXSIZE =2048GB 
, FILEGROWTH = 10%
 )
 WITH CATALOG_COLLATION = DATABASE_DEFAULT;

les caractéristiques  d'un transactions  

Atomique 
Cohérente  
Isolée 
Durable 

