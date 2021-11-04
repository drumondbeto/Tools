# SELECT

### SELECT all
- Returns all registers in the choosen table:


        SELECT * FROM table_name;


### SELECT specific columns
- Returns the selected columns with all their items from the choosen table:


        SELECT column_1, column_2 FROM table_name;


### SELECT DISTINCT
- Returns from the selected columns all the values excluding the duplicates:


        SELECT DISTINCT column_1 FROM table_name;


### SELECT COUNT 
- Counts the specified values from the table due the conditions:


        SELECT COUNT ( DISTINCT column_1 ) FROM table_name;