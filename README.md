# The SQL style guide

The goal of this style guide is to improve the readability of SQL queries.

## Keywords

* Keywords should be UPPERCASE.

    ```SQL
    /* Good */
    SELECT COUNT(1) FROM tablename WHERE 1;
    
    /* Bad */
    select count(1) from tablename where 1;
    ```

## Indentation and newlines

* Newlines should be used for any query that is at all complex or longer than 72 characters.

* Each clause should begin a new line.
  SELECT, JOIN, LEFT JOIN, OUTER JOIN, WHERE, UNION, etc. are keywords that begin new clauses.

    ```SQL
    /* Good */
    SELECT COUNT(1)
      FROM tablename
     WHERE really_loooong_column = CONCAT(other_column, ' street');
    
    /* Bad */
    SELECT COUNT(1) FROM tablename WHERE really_loooong_column = CONCAT(other_column, ' street');
    ```    

* The keywords that begin a clause should be right-aligned.
  The idea is to make a single character column between the keywords and their objects.

    ```SQL
    /* Good */
    SELECT COUNT(1)
      FROM tablename
     WHERE 1;
    
      SELECT key_column, COUNT(1)
        FROM tablename
    GROUP BY key_column;
    
    /* Bad */
    SELECT COUNT(1)
    FROM tablename
    WHERE 1;
    ```