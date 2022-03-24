# APPLICATIONS

## Collabrative Filtering ##  

Step 1: Obtain the data and command files. Create a graph model

> gsql 'DROP ALL'  
> gsql cf_model.gsql  
> gsql 'CREATE GRAPH gsql_demo(*)'  

Step 2: Load data:

> gsql -g gsql_demo cf_load.gsql  

Step 3: Install and execute the query:

> gsql -g gsql_demo cf_query.gsql  
> gsql -g gsql_demo 'INSTALL QUERY topCoLiked'  
> gsql -g gsql_demo 'RUN QUERY topCoLiked("id1", 20)'  

Step 4: Create a graph model

> CREATE VERTEX User (PRIMARY_ID id string)  
> CREATE DIRECTED EDGE Liked (FROM User, TO User) WITH REVERSE_EDGE = "Liked_By"  
> CREATE GRAPH gsql_demo(*)  

Step 5: The CREATE commands can be stored in one file and executed together.

> CREATE VERTEX User (PRIMARY_ID id string)  
> CREATE DIRECTED EDGE Liked (FROM User, TO User) WITH REVERSE_EDGE = "Liked_By"  

## GRAPH BASED QUERY

SELECT count(*) FROM User  > Display the number of User vertices  

SELECT count(*) FROM User-(Liked)->User  > Display the number of directed Liked edges from User type to User type  

SELECT approx_count(*) FROM User > Display the number of User vertices according to cached statistics. Response time may be faster than count(*).  

SELECT approx_count(*) FROM User-(Liked)->User  > Display the number of directed Liked edges from User type to User Type, according to cached statistics. Response time may be faster than count(*).   

SELECT * FROM User LIMIT 3 > Display all id, type, and attribute information for up to 3 User vertices.  

A LIMIT or WHERE condition is required, to prevent the output from being too large.  

SELECT * FROM User WHERE primary_id=="id2" > Display all id, type and attribute information for the User vertex whose primary_id is "id2".  

The WHERE clause can also specify non-ID attributes.  

SELECT * FROM User-(ANY)->ANY WHERE from_id=="id1" > Display all id,type, and attribute information about any type of edge which starts from vertex "id1".  

## MODIFICATIONS

Deleting: Suppose we want to delete vertex id3 and all its connections:

>DELETE FROM User WHERE primary_id=="id3"

Altering the schema:

>CREATE GLOBAL SCHEMA_CHANGE JOB cf_mod2 {  
	ALTER VERTEX User ADD ATTRIBUTE (name string);  
    ALTER EDGE Liked ADD ATTRIBUTE (weight float DEFAULT 1);  
}  
RUN JOB cf_mod2  

Modifying the attributes of existing objects:  

>UPSERT User VALUES ("id1", "Aaron")  
UPSERT User VALUES ("id2", "Bobbie")  
UPSERT User-(Liked)->User VALUES ("id2","id1",2.5)  


## Following these steps many more applications can be optimized on TigerGraph

### Example 2. Page Rank
### Example 3. Simple Product Recommendation
### Example 4. Same Name Search
### Example 5. Content-Based Filtering Recommendation of Videos  
### Example 6. People You May Know  
