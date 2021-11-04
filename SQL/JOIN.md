# JOINs

## INNER JOIN

- Returns records that have matching values in both tables.

        SELECT column_name(s)
        FROM table1
        INNER JOIN table2
        ON table1.column_name = table2.column_name;

## LEFT OUTTER JOIN

- Returns all records from the left tables and the matched records from the right table.

        SELECT column_name(s)
        FROM table1
        LEFT JOIN table 2
        ON table1.column_name = table2.column_name;

## RIGHT OUTTER JOIN

- Return all the records from the right table and the matched records from the left table.

        SELECT column_name(s)
        FROM table1
        RIGHT JOIN table2
        ON table1.column_name = table2.column_name;

## FULL OUTTER JOIN

- Return all the records when there's a match in either left or right tables.

        SELECT column_name(s)
        FROM table1
        FULL OUTTER JOIN table2
        ON table1.column_name = table2.column_name
        WHERE condition;
