# The SQL style guide

The goal of this style guide is to improve the readability of SQL queries.

## Formatting

### Keywords

* Keywords should be UPPERCASE.

    ```SQL
    /* Good */
    SELECT COUNT(1) FROM tablename WHERE 1;
    
    /* Bad */
    select count(1) from tablename where 1;
    ```

### Indentation and newlines

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

* Subqueries should be aligned as though the open parenthesis were the 0-column
  So, they should be indented as a unit, to identify them as subqueries.  They should continue to have the opening keywords right-aligned.

    ```SQL
    /* Good */
    SELECT *
      FROM (  SELECT a.name, count(1)
                FROM tablea a
                JOIN tableb b ON a.id = b.a_id
            GROUP BY a.name) name_count
      JOIN city c ON name_count.name = c.mayor;
    
    /* Bad */
    SELECT *
      FROM (SELECT a.name, count(1)
        FROM tablea a
        JOIN tableb b ON a.id = b.a_id
        GROUP BY a.name) name_count
      JOIN city c ON name_count.name = c.mayor;
    ```   

## Structure

* Column aliases should always use the keyword AS
  This becomes significant when a query has several columns selected with columns aliased.  Without the AS keyword, a dropped comma makes two columns become a single aliased column.

    ```SQL
    /* Good */
    SELECT ebe_ebs_sox_flag_set_for_all_crs AS sox_ok
      FROM tablename;
    
    /* Bad */
    SELECT ebe_ebs_sox_flag_set_for_all_crs sox_ok
      FROM tablename;
    
    ```    
