# WHERE

- WHERE is a clause used to filter the registers.

        SELECT * FROM table_name
        WHERE Field = 'Value';

## Operators in the WHERE clause

### Equal ( = )

        SELECT * FROM Customers
        WHERE Country = 'Mexico';

### Greater Than ( > )

        SELECT * FROM Products
        WHERE Discount > 15;

### Less Than ( < )

        SELECT * FROM Products
        WHERE Price < 50;

### Greater Than or Equal ( >= )

        SELECT * FROM Products
        WHERE Discount >= 30;

### Less Than or Equal ( <= )

        SELECT * FROM Products
        WHERE Price <= 50;

### Not Equal ( <> )

        SELECT * FROM table_name
        WHERE CustomerID <> 4;

### BETWEEN

- Search a certain range

        SELECT * FROM table_name
        WHERE Price BETWEEN 50 AND 60;

### LIKE

- Search for a patter.

- 's%' means the value starts with 's':

        SELECT * FROM table_name
        WHERE City LIKE 's%'

- '%r%' means the value has a 'r' somewhere:

        SELECT * FROM table_name
        WHERE City LIKE '%r%'

- '%a' means the value ends with an 'a':

        SELECT * FROM table_name
        WHERE City LIKE '%a'

### IN

- Search for multiples values.

        SELECT * FROM table_name
        WHERE City IN ('London', 'Paris');
