// CREATE commands

    CREATE TABLE database_name (
    column names DATATYPE,
    etc.
    )

    e.g. CREATE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50)
    )
    -- SERIAL will auto increment w each additional record insertion

// READ command(s)

    e.g.
    SELECT name FROM Cities WHERE name = "Austin"
    SELECT \* FROM Cities
    -can do calculations   and name calculation column
        e.g. SELECT name, population / area  AS population_density
             FROM cities

// DELETE command(s)

    DROP TABLE cities
    DELETE FROM cities WHERE name = "Tokyo";

// UPDATE command(s)

    UPDATE cities SET population = blahblahblah WHERE name = "Tokyo"
        -in this case only one city  has the name Tokyo

    INSERT INTO database_name (column name) VALUES value of what we want to add
    e.g. INSERT INTO cities (name, country, population, area)
    VALUES
    ('Delhi, 'India', 28)

// RELATIONSHIPS

    One-to-Many / Many-to-One
    -same sort of relationship, inverse relationship - depends on perspective
    -e.g. Instagram Post and Likes,  User and Instagram Posts

    Many-to-Many
    -e.g Students and Classes, Players and Football Matches, Movies and Actors

    One-To-One
    -e.g. Company and CEO, Student and Desk, Capitol City and Country


    // Primary Keys and Foreign Keys

        - Primary keys used uniquely identify a row of a record
        -Foreign keys used to identify a row with an another row of an associated TABLE
            CREATE TABLE photos (
            id SERIAL PRIMARY KEY
            url VARCHAR(200)
            user_id INTEGER REFERENCES users(id) (can also set DELETE CONTRAINTS see below)
            )

        -Queries to run with Associated Data
            SELECT * FROM data-base-name WHERE table1_id = 4
                --like all photos from one user (WHERE user_id=4)

        -DELETE constraints
            ON DELETE NO  ACTION (default)/ON DELETE RESTRICT
                - throws error
            ON DELETE CASCADE
                - delete the record in the one table (in one to many relationship) , deletes all associated reocrds  (user and photos)
            ON DELETE SET NULL
                - set the foregin key to null when parent table/record is deleted
            ON  DELETE SET DEFAULT NULL
                -  - set the foregin key to a default valye if one is provided when parent table/record is deleted

    //JOIN statements
      -INNER JOINS
         e.g. SELECT url, username
              FROM photos
              JOIN users ON users.id  = photos.user_id

              SELECT contents, username
              FROM comments
              JOIN users ON users.id = comments.user_id;

              --if there is no match, the row is not returned in our query
        -LEFT JOIN
            SELECT url, username
              FROM photos
              LEFT JOIN users ON users.id  = photos.user_id
                --if there is no match, the row is returned w null values respectively
                -- in example it's returning all records of photos
        -RIGHT JOIN
            -- same as above, but opposite, any mismatched rows are not returned, but all records from 'right' table
        -FULL JOIN
            -- joins all records from both tables and any non corresponding records get NULL values set respectively

        -Can add WHERE statement at THE END of the query to filter out whatever
            SELECT url, contents
            FROM comments
            JOIN photos ON photos.id = comments.photo_id
            WHERE comments.user_id = photos.user_id;
                -- this is joining comments and photos where the user id of the comment is the user id of the photo

        -Three Way Joins
            SELECT url, contents, username
            FROM comments
            JOIN photos ON photos.id = comments.photo_id
            JOIN users ON users.id = comments.user_id AND users.id = photos.user_id
            WHERE comments.user_id = photos.user_id;

// Grouping -
cannot select any column BUT the one it is grouped by

ex. find the number of comments for each photo

    SELECT photo_id, COUNT(*)
    FROM comments
    GROUP BY photo_id;

ex. write a query that prints an authors name (rather than ID) and the number of books they have authored

    SELECT name, COUNT(*)
    FROM books
    JOIN authors ON authors.id = books.author_id
    GROUP BY authors.name;

    ---HAVING-----
    filters queries using GROUP BY *will only see HAVING when there is a GROUP BY*
    typically includes an aggregate keyword

    ex. find the number of comments for each photo where the photo_id is less than 3 (will use WHERE because it deals with the value of a row) and the photo has more than 2 comments (will use HAVING because it uses an aggregate function and some filtering)

    SELECT photo_id, COUNT(*)
    FROM comments
    WHERE photo_id < 3
    GROUP BY photo_id
    HAVING COUNT(*) > 2

    ex. find users (user_ids) whhere the user has commented on the first 50 photos and the user added more than to 20 comments on those photos

    SELECT user_id, COUNT(*)
    FROM comments
    WHERE photo_id < 50
    GROUP BY user_id
    HAVING  COUNT(*) > 20

    ex. Given a table of phones, print the names of manufacturers and total revenue (price * units_sold) for all phones.  Only print the manufacturers who have revenue greater than 2,000,000 for all the phones they sold.

    SELECT manufacture, SUM(price * units_sold)
    FROM phones
    GROUP BY manufacturer
    HAVING SUM(price * units_sold) > 2000000

MISC SORTING

    ORDER BY (column name)
        --can add DESC for descending result
        --default is ascending

    OFFSET
        -- skips x number of rows

        SELECT *
        FROM users
        OFFSET 40;

    LIMIT
        -- only takes certain number of rows  for query
        -- would use if needed top x records (most expensive or something)

        SELECT *
        FROM users
        LIMIT 2;

    * by convention we would put LIMIT first if using both *

// WORKING WITH MULTIPLE SETS OF DATA

    UNION

    - put () around multiple queries
    - joins results of two wueries and removes duplicate rows
    - UNION ALL includes duplicate rows

    INTERSECT

    - finds rows in common in results of two queries - removes duplicates
    - INTERSECT ALL includes duplicates

    EXCEPT

    - find rows that are present in first query but not second - removes duplicates
    - EXVEPT ALL does the same but includes duplicates
