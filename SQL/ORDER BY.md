# ORDER BY

        SELECT column1, column2, ...
        FROM table_name
        ORDER BY column1, column2, ... ASC|DESC;

* When the order is not setted, the default order is ASC.

## Exemples

        SELECT * FROM Customers
        ORDER BY Contry, CustomerName;

        SELECT * FROM Customers
        ORDER BY Contry ASC, CustomerName DESC;

* When you set two columns after the ORDER BY keywork, the first one is the primary ordered column and the second one is secondary ordered when there's more than a result with the same value in the pimary ordered column.
