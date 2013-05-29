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

### Names
* Named objects should not be surrounded by backticks, no matter what MySQL says when it dumps the table structure for you.
  *  If you need to use backticks because of something in your table name, rename your table.

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
      FROM (  SELECT candidates.name, count(1)
                FROM candidates
                JOIN votes ON candidates.id = votes.candidate_id
            GROUP BY candidates.name) name_count
      JOIN city c ON name_count.name = c.mayor;
    
    /* Bad */
    SELECT *
      FROM (SELECT candidates.name, count(1)
        FROM candidates
        JOIN votes ON candidates.id = votes.candidate_id
        GROUP BY candidates.name) name_count
      JOIN city ON name_count.name = city.mayor;
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
* Table aliases and column aliases should be descriptive.
  Much like variable names, "a", "b", "x", etc are not generally useful in and of themselves outside of short examples.

* Tiny names for table aliases can sometimes work as abbreviations.
  As an example, if "releases" is referenced frequently, it might make sense to abbreviate it "r".  However, "rel" is almost as short, and much more descriptive.  Have a good reason for "r" instead of "rel".

* Subquery aliases should be even more descriptive.
  Subqueries effectively create ad-hoc tables in memory.  As such, if you name it "x", then there's absolutely nothing to suggest the intention behind the table to a later maintainer.
  
  ```SQL
  /* Good */
  SELECT *
    FROM (SELECT table1.id AS child, 
                 table2.id AS parent
            FROM table1
            JOIN table2 ON (table2.parent_id = table1.id) ) parentage;

  /* Bad */
  SELECT *
    FROM (SELECT table1.id AS child, 
                 table2.id AS parent
            FROM table1
            JOIN table2 ON (table2.parent_id = table1.id) ) x;

  /* Bad */
  SELECT *
    FROM (SELECT table1.id AS child, 
                 table2.id AS parent
            FROM table1
            JOIN table2 ON (table2.parent_id = table1.id) ) link;
  ```

  *  Abbreviations don't help in subquery aliases
