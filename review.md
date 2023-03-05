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

SELECT name FROM Cities WHERE name = "Austin"
SELECT \* FROM Cities

// DELETE command(s)

// UPDATE command(s)
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
         e.g. SELECT url, username FROM photos
              JOIN users ON users.id  = photos.user_id