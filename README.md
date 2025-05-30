# PostgreSQL

### Command Prompt

'''
Microsoft Windows [Version 10.0.26100.4202]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Minfy>psql -U postgres
Password for user postgres:

psql (17.5)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.

postgres=# CREATE USER university_admin WITH PASSWORD 'your_secure_password';
CREATE ROLE
postgres=# CREATE DATABASE university_db OWNER university_admin;
CREATE DATABASE
postgres=# GRANT ALL PRIVILEGES ON DATABASE university_db TO university_admin;
GRANT
postgres=# \q

C:\Users\Minfy>psql -U university_admin -d university_db
Password for user university_admin:

psql (17.5)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.

university_db=> \list
                                                                          List of databases
     Name      |      Owner       | Encoding | Locale Provider |      Collate       |       Ctype        | Locale | ICU Rules |           Access privileges
---------------+------------------+----------+-----------------+--------------------+--------------------+--------+-----------+---------------------------------------
 postgres      | postgres         | UTF8     | libc            | English_India.1252 | English_India.1252 |        |           |
 template0     | postgres         | UTF8     | libc            | English_India.1252 | English_India.1252 |        |           | =c/postgres                          +
               |                  |          |                 |                    |                    |        |           | postgres=CTc/postgres
 template1     | postgres         | UTF8     | libc            | English_India.1252 | English_India.1252 |        |           | =c/postgres                          +
               |                  |          |                 |                    |                    |        |           | postgres=CTc/postgres
 university_db | university_admin | UTF8     | libc            | English_India.1252 | English_India.1252 |        |           | =Tc/university_admin                 +
               |                  |          |                 |                    |                    |        |           | university_admin=CTc/university_admin
(4 rows)


university_db=> \du
                                 List of roles
    Role name     |                         Attributes
------------------+------------------------------------------------------------
 postgres         | Superuser, Create role, Create DB, Replication, Bypass RLS
 university_admin |


university_db=> \conninfo
You are connected to database "university_db" as user "university_admin" on host "localhost" (address "::1") at port "5432".
university_db=> \dt
Did not find any relations.
university_db=> create table students{
university_db-> create table students (
university_db(> student_id integer,
university_db(> first_name varchar(50),
university_db(> last_name varchar(10),
university_db(> email varchar(20),
university_db(> dob date
university_db(> );
ERROR:  syntax error at or near "{"
LINE 1: create table students{
                             ^
university_db=> create table students (
university_db(> student_id integer,
university_db(> first_name varchar(50),
university_db(> last_name varchar(10),
university_db(> email varchar(20),
university_db(> dob date
university_db(> );
CREATE TABLE
university_db=> \dt
              List of relations
 Schema |   Name   | Type  |      Owner
--------+----------+-------+------------------
 public | students | table | university_admin
(1 row)


university_db=> \d students
                       Table "public.students"
   Column   |         Type          | Collation | Nullable | Default
------------+-----------------------+-----------+----------+---------
 student_id | integer               |           |          |
 first_name | character varying(50) |           |          |
 last_name  | character varying(10) |           |          |
 email      | character varying(20) |           |          |
 dob        | date                  |           |          |


university_db=> alter table studenta add column phone_number varchar(20)
university_db-> ;
ERROR:  relation "studenta" does not exist
university_db=> alter table students add column phone_number varchar(20);
ALTER TABLE
university_db=> \d students
                        Table "public.students"
    Column    |         Type          | Collation | Nullable | Default
--------------+-----------------------+-----------+----------+---------
 student_id   | integer               |           |          |
 first_name   | character varying(50) |           |          |
 last_name    | character varying(10) |           |          |
 email        | character varying(20) |           |          |
 dob          | date                  |           |          |
 phone_number | character varying(20) |           |          |


university_db=> alter table students drop column phone_number;
ALTER TABLE
university_db=> insert into students (student_id, first_name, last_name, email, dob)
university_db-> values
university_db-> (1, 'Alice', 'Smith', 'alice.smith@example.com', '2003-05-15');
ERROR:  value too long for type character varying(20)
university_db=> insert into students (student_id, first_name, last_name, email, dob)
university_db-> values
university_db-> (1, 'Alice', 'Smith', 'al@gmail.com', '2003-05-15');
INSERT 0 1
university_db=> INSERT INTO students (student_id, first_name, last_name, email, dob) VALUES
university_db-> (4, 'Diana', 'Prince', 'diana.prince@example.com', '2001-11-01'),
university_db-> (5, 'Ethan', 'Hunt', 'ethan.hunt@example.com', '2002-03-15'),
university_db-> (6, 'Fiona', 'Gallagher', 'fiona.g@example.com', '2003-07-20'),
university_db-> (7, 'George', 'Martin', 'george.martin@example.com', '2002-12-05'),
university_db-> (8, 'Hannah', 'Baker', 'hannah.baker@example.com', '2003-09-30');
ERROR:  value too long for type character varying(20)
university_db=> INSERT INTO students (student_id, first_name, last_name, email, dob) VALUES
university_db-> (4, 'Diana', 'Prince', 'di@ex.com', '2001-11-01'),
university_db-> (5, 'Ethan', 'Hunt', 'et@ex.com', '2002-03-15'),
university_db-> (6, 'Fiona', 'Gallagher', 'fi@ex.com', '2003-07-20'),
university_db-> (7, 'George', 'Martin', 'gm@ex.com', '2002-12-05'),
university_db-> (8, 'Hannah', 'Baker', 'hb@ex.com', '2003-09-30');
INSERT 0 5
university_db=> select * from students
university_db-> ;
 student_id | first_name | last_name |    email     |    dob
------------+------------+-----------+--------------+------------
          1 | Alice      | Smith     | al@gmail.com | 2003-05-15
          4 | Diana      | Prince    | di@ex.com    | 2001-11-01
          5 | Ethan      | Hunt      | et@ex.com    | 2002-03-15
          6 | Fiona      | Gallagher | fi@ex.com    | 2003-07-20
          7 | George     | Martin    | gm@ex.com    | 2002-12-05
          8 | Hannah     | Baker     | hb@ex.com    | 2003-09-30
(6 rows)


university_db=> INSERT INTO students (student_id, first_name, last_name, email, dob) VALUES
university_db-> (9, 'Irene', 'Adams', 'irene.ad@gmail.com', '2001-05-10'),
university_db-> (10, 'Jack', 'Brown', 'jackb@yahoo.com', '2002-07-22'),
university_db-> (11, 'Karen', 'Clark', 'kclark@outlook.com', '2003-01-18'),
university_db-> (12, 'Liam', 'Davis', 'liam.d@gmail.com', '2001-11-30'),
university_db-> (13, 'Mia', 'Evans', 'mia_ev@yahoo.com', '2002-04-25');
INSERT 0 5
university_db=> select * from students
university_db-> ;
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          1 | Alice      | Smith     | al@gmail.com       | 2003-05-15
          4 | Diana      | Prince    | di@ex.com          | 2001-11-01
          5 | Ethan      | Hunt      | et@ex.com          | 2002-03-15
          6 | Fiona      | Gallagher | fi@ex.com          | 2003-07-20
          7 | George     | Martin    | gm@ex.com          | 2002-12-05
          8 | Hannah     | Baker     | hb@ex.com          | 2003-09-30
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
         10 | Jack       | Brown     | jackb@yahoo.com    | 2002-07-22
         11 | Karen      | Clark     | kclark@outlook.com | 2003-01-18
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
         13 | Mia        | Evans     | mia_ev@yahoo.com   | 2002-04-25
(11 rows)


university_db=> SELECT * FROM students WHERE student_id = 2;
 student_id | first_name | last_name | email | dob
------------+------------+-----------+-------+-----
(0 rows)


university_db=> SELECT * FROM students WHERE dob < '2003-01-01';
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          4 | Diana      | Prince    | di@ex.com          | 2001-11-01
          5 | Ethan      | Hunt      | et@ex.com          | 2002-03-15
          7 | George     | Martin    | gm@ex.com          | 2002-12-05
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
         10 | Jack       | Brown     | jackb@yahoo.com    | 2002-07-22
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
         13 | Mia        | Evans     | mia_ev@yahoo.com   | 2002-04-25
(7 rows)


university_db=> SELECT * FROM students WHERE first_name LIKE 'B%' OR first_name LIKE 'C%';
 student_id | first_name | last_name | email | dob
------------+------------+-----------+-------+-----
(0 rows)


university_db=> SELECT * FROM students WHERE email LIKE '%@example.com';
 student_id | first_name | last_name | email | dob
------------+------------+-----------+-------+-----
(0 rows)


university_db=> SELECT * FROM students WHERE email LIKE '%@example.com';
 student_id | first_name | last_name | email | dob
------------+------------+-----------+-------+-----
(0 rows)


university_db=> SELECT * FROM students WHERE email LIKE '%@ex.com';
 student_id | first_name | last_name |   email   |    dob
------------+------------+-----------+-----------+------------
          4 | Diana      | Prince    | di@ex.com | 2001-11-01
          5 | Ethan      | Hunt      | et@ex.com | 2002-03-15
          6 | Fiona      | Gallagher | fi@ex.com | 2003-07-20
          7 | George     | Martin    | gm@ex.com | 2002-12-05
          8 | Hannah     | Baker     | hb@ex.com | 2003-09-30
(5 rows)


university_db=> SELECT * FROM students WHERE email IS NULL;
 student_id | first_name | last_name | email | dob
------------+------------+-----------+-------+-----
(0 rows)


university_db=> INSERT INTO students (student_id, first_name, last_name, email, dob)
university_db-> VALUES (6, 'Eve', 'Adams', NULL, '2003-07-10');
INSERT 0 1
university_db=> INSERT INTO students (student_id, first_name, last_name, dob)
university_db-> VALUES (7, 'Frank', 'Miller', '2004-04-20');
INSERT 0 1
university_db=> select * from students;
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          1 | Alice      | Smith     | al@gmail.com       | 2003-05-15
          4 | Diana      | Prince    | di@ex.com          | 2001-11-01
          5 | Ethan      | Hunt      | et@ex.com          | 2002-03-15
          6 | Fiona      | Gallagher | fi@ex.com          | 2003-07-20
          7 | George     | Martin    | gm@ex.com          | 2002-12-05
          8 | Hannah     | Baker     | hb@ex.com          | 2003-09-30
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
         10 | Jack       | Brown     | jackb@yahoo.com    | 2002-07-22
         11 | Karen      | Clark     | kclark@outlook.com | 2003-01-18
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
         13 | Mia        | Evans     | mia_ev@yahoo.com   | 2002-04-25
          6 | Eve        | Adams     |                    | 2003-07-10
          7 | Frank      | Miller    |                    | 2004-04-20
(13 rows)


university_db=> SELECT * FROM students WHERE email IS NULL;
 student_id | first_name | last_name | email |    dob
------------+------------+-----------+-------+------------
          6 | Eve        | Adams     |       | 2003-07-10
          7 | Frank      | Miller    |       | 2004-04-20
(2 rows)


university_db=> update students set email = 'priyeshraidelhi@gmail.com', first_name = 'Priyesh', last_name = 'Rai', dob = '2002-11-07' where student_id = 7;
ERROR:  value too long for type character varying(20)
university_db=> update students set email = 'praidel@gmail.com', first_name = 'Priyesh', last_name = 'Rai', dob = '2002-11-07' where student_id = 7;
UPDATE 2
university_db=> select * from students;
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          1 | Alice      | Smith     | al@gmail.com       | 2003-05-15
          4 | Diana      | Prince    | di@ex.com          | 2001-11-01
          5 | Ethan      | Hunt      | et@ex.com          | 2002-03-15
          6 | Fiona      | Gallagher | fi@ex.com          | 2003-07-20
          8 | Hannah     | Baker     | hb@ex.com          | 2003-09-30
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
         10 | Jack       | Brown     | jackb@yahoo.com    | 2002-07-22
         11 | Karen      | Clark     | kclark@outlook.com | 2003-01-18
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
         13 | Mia        | Evans     | mia_ev@yahoo.com   | 2002-04-25
          6 | Eve        | Adams     |                    | 2003-07-10
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
(13 rows)


university_db=> select * from students order by lastname asc;
ERROR:  column "lastname" does not exist
LINE 1: select * from students order by lastname asc;
                                        ^
HINT:  Perhaps you meant to reference the column "students.last_name".
university_db=> select * from students order by last_name asc;
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          6 | Eve        | Adams     |                    | 2003-07-10
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
          8 | Hannah     | Baker     | hb@ex.com          | 2003-09-30
         10 | Jack       | Brown     | jackb@yahoo.com    | 2002-07-22
         11 | Karen      | Clark     | kclark@outlook.com | 2003-01-18
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
         13 | Mia        | Evans     | mia_ev@yahoo.com   | 2002-04-25
          6 | Fiona      | Gallagher | fi@ex.com          | 2003-07-20
          5 | Ethan      | Hunt      | et@ex.com          | 2002-03-15
          4 | Diana      | Prince    | di@ex.com          | 2001-11-01
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
          1 | Alice      | Smith     | al@gmail.com       | 2003-05-15
(13 rows)


university_db=> select * from students where email like'%gmail.com' order by last_name asc;
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
          1 | Alice      | Smith     | al@gmail.com       | 2003-05-15
(5 rows)


university_db=> select * from students where email like'%gmail.com' order by first_name asc, last_name desc;
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          1 | Alice      | Smith     | al@gmail.com       | 2003-05-15
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
(5 rows)


university_db=> SELECT COUNT(email) AS students_with_email FROM students;
 students_with_email
---------------------
                  12
(1 row)


university_db=> SELECT COUNT(email) AS students_with_email FROM students where email is null or email = '';
 students_with_email
---------------------
                   0
(1 row)


university_db=> select count(*) as num_of_people
university_db-> from students
university_db-> group by email;
 num_of_people
---------------
             1
             1
             1
             1
             2
             1
             1
             1
             1
             1
             1
             1
(12 rows)


university_db=> select * from students
university_db-> ;
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          1 | Alice      | Smith     | al@gmail.com       | 2003-05-15
          4 | Diana      | Prince    | di@ex.com          | 2001-11-01
          5 | Ethan      | Hunt      | et@ex.com          | 2002-03-15
          6 | Fiona      | Gallagher | fi@ex.com          | 2003-07-20
          8 | Hannah     | Baker     | hb@ex.com          | 2003-09-30
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
         10 | Jack       | Brown     | jackb@yahoo.com    | 2002-07-22
         11 | Karen      | Clark     | kclark@outlook.com | 2003-01-18
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
         13 | Mia        | Evans     | mia_ev@yahoo.com   | 2002-04-25
          6 | Eve        | Adams     |                    | 2003-07-10
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
(13 rows)


university_db=> select count(*) as emails from students
university_db-> where email like '%@%'
university_db-> group by email;
 emails
--------
      1
      1
      1
      2
      1
      1
      1
      1
      1
      1
      1
(11 rows)


university_db=> select * from students;
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          1 | Alice      | Smith     | al@gmail.com       | 2003-05-15
          4 | Diana      | Prince    | di@ex.com          | 2001-11-01
          5 | Ethan      | Hunt      | et@ex.com          | 2002-03-15
          6 | Fiona      | Gallagher | fi@ex.com          | 2003-07-20
          8 | Hannah     | Baker     | hb@ex.com          | 2003-09-30
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
         10 | Jack       | Brown     | jackb@yahoo.com    | 2002-07-22
         11 | Karen      | Clark     | kclark@outlook.com | 2003-01-18
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
         13 | Mia        | Evans     | mia_ev@yahoo.com   | 2002-04-25
          6 | Eve        | Adams     |                    | 2003-07-10
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
          7 | Priyesh    | Rai       | praidel@gmail.com  | 2002-11-07
(13 rows)


university_db=> \d students
                       Table "public.students"
   Column   |         Type          | Collation | Nullable | Default
------------+-----------------------+-----------+----------+---------
 student_id | integer               |           |          |
 first_name | character varying(50) |           |          |
 last_name  | character varying(10) |           |          |
 email      | character varying(20) |           |          |
 dob        | date                  |           |          |


university_db=> Alter table students
university_db-> add constraint student_pkey primary key (student_id),
university_db-> add constraint unique_student_email unique (email),
university_db-> alter column first_name set not null,
university_db-> alter column last_name set not null,
university_db-> alter column dob set not null;
ERROR:  could not create unique index "student_pkey"
DETAIL:  Key (student_id)=(7) is duplicated.
university_db=> DELETE FROM students WHERE student_id = 7;
DELETE 2
university_db=> Alter table students
university_db-> add constraint student_pkey primary key (student_id),
university_db-> add constraint unique_student_email unique (email),
university_db-> alter column first_name set not null,
university_db-> alter column last_name set not null,
university_db-> alter column dob set not null;
ERROR:  could not create unique index "student_pkey"
DETAIL:  Key (student_id)=(6) is duplicated.
university_db=> DELETE FROM students WHERE student_id = 6;
DELETE 2
university_db=> Alter table students
university_db-> add constraint student_pkey primary key (student_id),
university_db-> add constraint unique_student_email unique (email),
university_db-> alter column first_name set not null,
university_db-> alter column last_name set not null,
university_db-> alter column dob set not null;
ALTER TABLE
university_db=> \d students
                       Table "public.students"
   Column   |         Type          | Collation | Nullable | Default
------------+-----------------------+-----------+----------+---------
 student_id | integer               |           | not null |
 first_name | character varying(50) |           | not null |
 last_name  | character varying(10) |           | not null |
 email      | character varying(20) |           |          |
 dob        | date                  |           | not null |
Indexes:
    "student_pkey" PRIMARY KEY, btree (student_id)
    "unique_student_email" UNIQUE CONSTRAINT, btree (email)


university_db=> select * from students;
 student_id | first_name | last_name |       email        |    dob
------------+------------+-----------+--------------------+------------
          1 | Alice      | Smith     | al@gmail.com       | 2003-05-15
          4 | Diana      | Prince    | di@ex.com          | 2001-11-01
          5 | Ethan      | Hunt      | et@ex.com          | 2002-03-15
          8 | Hannah     | Baker     | hb@ex.com          | 2003-09-30
          9 | Irene      | Adams     | irene.ad@gmail.com | 2001-05-10
         10 | Jack       | Brown     | jackb@yahoo.com    | 2002-07-22
         11 | Karen      | Clark     | kclark@outlook.com | 2003-01-18
         12 | Liam       | Davis     | liam.d@gmail.com   | 2001-11-30
         13 | Mia        | Evans     | mia_ev@yahoo.com   | 2002-04-25
(9 rows)


university_db=> create table courses(
university_db(> course_id serial primary key,
university_db(> course_name varchar(100) not null,
university_db(> credits integer not null
university_db(> );
CREATE TABLE
university_db=> \d courses
                                          Table "public.courses"
   Column    |          Type          | Collation | Nullable |                  Default
-------------+------------------------+-----------+----------+--------------------------------------------
 course_id   | integer                |           | not null | nextval('courses_course_id_seq'::regclass)
 course_name | character varying(100) |           | not null |
 credits     | integer                |           | not null |
Indexes:
    "courses_pkey" PRIMARY KEY, btree (course_id)


university_db=> insert into courses(course_name, credits)
university_db-> values
university_db-> (dsa,16),
university_db-> (daa,16),
university_db-> (oops,12),
university_db-> (cn,10),
university_db-> (project,18),
university_db-> (hpc,10);
ERROR:  column "dsa" does not exist
LINE 3: (dsa,16),
         ^
university_db=> insert into courses(course_name, credits)
university_db-> values
university_db-> ('dsa',16),
university_db-> ('daa',16),
university_db-> ('oops',12),
university_db-> ('cn',10),
university_db-> ('project',18),
university_db-> ('hpc',10);
INSERT 0 6
university_db=> \d courses
                                          Table "public.courses"
   Column    |          Type          | Collation | Nullable |                  Default
-------------+------------------------+-----------+----------+--------------------------------------------
 course_id   | integer                |           | not null | nextval('courses_course_id_seq'::regclass)
 course_name | character varying(100) |           | not null |
 credits     | integer                |           | not null |
Indexes:
    "courses_pkey" PRIMARY KEY, btree (course_id)


university_db=> \d students
                       Table "public.students"
   Column   |         Type          | Collation | Nullable | Default
------------+-----------------------+-----------+----------+---------
 student_id | integer               |           | not null |
 first_name | character varying(50) |           | not null |
 last_name  | character varying(10) |           | not null |
 email      | character varying(20) |           |          |
 dob        | date                  |           | not null |
Indexes:
    "student_pkey" PRIMARY KEY, btree (student_id)
    "unique_student_email" UNIQUE CONSTRAINT, btree (email)


university_db=>
'''
