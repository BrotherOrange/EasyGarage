# Easy Garage

## Table of Contents

1. [Overview](#Overview)
2. [Database Structure](#Database-Structure)
3. [Technical Specifications](#Technical-Specifications)
4. [Manual](#Manual)
5. [Demo](#Demo)
6. [Lesson Learned](#Lesson-Learned)
7. [Future Work](#Future-Work)
8. [Acknowledgment](#Acknowledgment)

## Overview

### Description

This project is a web application called **Easy Garage**, which is a commercial parking garage arrangement system on the app using a **MySQL** for storage of all records. The garage provides **300** parking places for users to park their vehicles. Easy Garage supports **parking instantly** and **making reservations in advance**. It also provides services of parking vehicles in 3 different sizes. People could create their own accout to become a user of the app. Users could add, park, and arrange their vehicles in the app.

### App Evaluation
- **Name**: Easy Garage
- **Type**: Web Application
- **Category**: Services Provider
- **Mission**: Provide a Parking & Garage Management System for visiters and garage owners and bring every one convenience. Help people to park their cars more easily and help garage owners to manage the garage more effectively.

### Team Members
- **Jihao Zhang**
- **Xiaowei Gu**

## Database Structure

### UML

<img src="https://i.imgur.com/UlAGKzu.png">

&nbsp;
&nbsp;
&nbsp;

<img src="https://i.imgur.com/IyrrUvg.png">

### Schema

#### User
|Column|Type|Extra|
|---|---|---|
|id|INT|PK, Auto Increment|
|email|VARCHAR(45)|Unique, Not Null|
|name|VARCHAR(45)|Not Null|
|password|VARCHAR(90)|Encrypt, Not Null|
|Description|TEXT|

#### Vehicle
|Column|Type|Extra|
|---|---|---|
|id|INT|PK, Auto Increment|
|name|VARCHAR(45)|Unique, Not Null|
|status|INT|From 1, 2, or 3, Not Null|
|type|INT|Not Null|
|user_id|INT|FK -> `User.id`, Not Null, Cascade On Delete|

#### Place
|Column|Type|Extra|
|---|---|---|
|id|INT|PK, Auto Increment|
|floor|INT|From 1, 2, or 3, Not Null|
|number|INT|From 1 to 100, Not Null|
|status|INT|From 0, 1, or 2, Not Null|
|normal_price|DOUBLE|Not Null|
|late_price|DOUBLE|Not Null|
|vehicle_id|INT|FK -> `Vehicle.id`, Restrict On Delete|

#### Parking
|Column|Type|Extra|
|---|---|---|
|id|INT|PK, Auto Increment|
|begin|DATETIME|Not Null|
|end|DATETIME|
|cost|DOUBLE|
|status|INT|From 1, 2, or 3, Not Null|
|user_id|INT|FK -> `User.id`, Not Null, Cascade On Delete|
|vehicle_id|INT|FK -> `Vehicle.id`, Not Null, Cascade On Delete|
|place_id|INT|FK -> `Place.id`, Not Null, Cascade On Delete|
|reservation_id|INT|FK -> `Reservation.id`, Cascade On Delete|

#### Reservation
|Column|Type|Extra|
|---|---|---|
|id|INT|PK, Auto Increment|
|status|INT|From 0 or 1, Not Null|
|created_at|DATETIME|Not Null|
|updated_at|DATETIME|
|user_id|INT|FK -> `User.id`, Not Null, Cascade On Delete|

#### Comment
|Column|Type|Extra|
|---|---|---|
|id|INT|PK, Auto Increment|
|text|TEXT|Not Null|
|created_at|DATETIME|Not Null|
|updated_at|DATETIME|
|user_id|INT|FK -> `User.id`, Not Null, Cascade On Delete|
|parking_id|INT|FK -> `Parking.id`, Not Null, Cascade On Delete|

### Operations & API Links

- **User**
    - Create [Sign Up -> POST](http://easygarage.link:8088/api/register)
    - Read
        - All [Get all users -> GET](http://easygarage.link:8088/api/users)
        - By `id` [Get user -> GET](http://easygarage.link:8088/api/users/1)
    - Update [Edit profile -> PUT](http://easygarage.link:8088/api/me)
    - Delete [Delete user by id ->DELETE](http://easygarage.link:8088/api/users/5)

- **Vehicle**
    - Create [Add vehicle -> POST](http://easygarage.link:8088/api/vehicles/add)
    - Read
        - All [Get all vehicle -> GET](http://easygarage.link:8088/api/vehicles)
        - By `id` [Get vehicle by id -> GET](http://easygarage.link:8088/vehicles/id/1)
        - By `name` [Get vehicle by name -> GET](http://easygarage.link:8088/api/vehicles/name/Benz)
        - By `type` [Get vehicle by type -> GET](http://easygarage.link:8088/api/vehicles/type/1)
        - By `place_id` [Get vehicle by place_id -> GET](http://easygarage.link:8088/api/vehicles/place/4)
    - Update [Update vehicle -> UPDATE](http://easygarage.link:8088/api/vehicles/update)
    - Delete
        - By `id` [Delete vehicle by id -> DELETE](http://easygarage.link:8088/api/vehicles//id_delete/5)
        - By `name` [Delete vehicle by name -> DELETE](http://easygarage.link:8088/api/vehicles/name_delete/Test)
- **Place**
    - Create (By Procedure) [Delete Reserve -> POST](http://easygarage.link:8088/api/places/position)
    - Read
        - All [Get all->GET](http://easygarage.link:8088/api/places)
        - By `id`[Get by ID->GET](http://easygarage.link:8088/api/places/id/1)
        - By `floor` [Get all places categorized by floors -> GET](http://easygarage.link:8088/api/places/floors)
        - By `floor` & `number`[Get by floor -> GET](http://easygarage.link:8088/api/places/floors)
        - By `status` [Get all free spaces -> GET](http://easygarage.link:8088/api/places/free/all)
        - By `floor`, `type` & `status`[Get all free places of specific floor and type -> POST](http://easygarage.link:8088/api/places/free/param)
    - Update [Update place status -> PUT](http://easygarage.link:8088/api/places/update)
- **Parking**
    - Create [Begin parking -> POST](http://ec2-3-92-225-56.compute-1.amazonaws.com:8088/api/parkings/add)
    - Read
        - All [Find all parking -> GET](http://easygarage.link:8088/api/parkings)
        - By `id` [Find parking by id -> GET](http://easygarage.link:8088/api/parkings/id/1)

    - Update [Stop parking -> PUT](http://easygarage.link:8088/api/parkings/update)
    - Delete
        - By `id` [Delete by id -> DELETE](http://easygarage.link:8088/api/parkings/delete/id/19)
        - By `begin`[Delete by begin time -> DELETE](http://easygarage.link:8088/api/parkings/delete/time/2022-12-07%2016%3A41%3A36.000)
- **Reservation**
    - Create [Create reservation -> POST](http://easygarage.link:8088/api/reservations/add)
    - Read
        - All[Find all reservation -> GET](http://easygarage.link:8088/api/reservations)
        - By `id`[Find reservation by id -> GET](http://easygarage.link:8088/api/reservations/id/9)
        - By `created_at`[Find by created time -> POST](http://easygarage.link:8088/api/reservations/time)
    - Update[End a reservation -> PUT](http://easygarage.link:8088/api/reservations/update)
    - Delete
        - By `id`[Delete by id -> DELETE](http://easygarage.link:8088/api/reservations/delete/id/10)
        - By `created_at`[Delete by created time -> DELETE](http://easygarage.link:8088/api/reservations/delete/time/2022-12-07%2017%3A06%3A11.000)
- **Comment**
    - Create[make comment -> POST](http://easygarage.link:8088/api/comments/add)
    - Read
        - All [Read all comments -> GET](http://easygarage.link:8088/api/comments)
        - By `id`[Read comments by id -> GET](http://easygarage.link:8088/api/comments/id/1)
        - By `created_at`[Find comments by created time->GET](http://easygarage.link:8088/api/comments/time)
    - Update
        - [update comment->PUT](http://easygarage.link:8088/api/comments/update)
    - Delete
        - By `id`[Delete by id -> DELETE](http://easygarage.link:8088/api/comments/delete/id/2)
        - By `created_at`[Delete by created time -> DELETE](http://easygarage.link:8088/api/comments/delete/time/2022-12-07%2017%3A21%3A01.000)

## Technical Specifications

### Backend

- **Language**: Java 11
- **Framework**: Spring Boot 2.0
- **User Authentication**: Spring Security
- **Database Provider**: MySQL of RDS from AWS
- **Database Services**: JPA based on JDBC
- **Backend Build Tool**: Maven
- **Backend Running Server**: EC2 cloud server from AWS

### Frontend
- **Language**: JavaScript, HTML
- **Framework**: React, Node
- **Project manager**: Yarn

### Other
- **Domain Name Provider**: RS3 provided by AWS
- **APIs Debugging Tool**: Postman

## Manual

### Project Link
[Easy Garage](http://www.easygarage.link:3000)

### Source Code Repository
[Easy Garage Github Repository](https://github.com/EasyGarage/EasyGarage.git)

### User Flow

<img src="https://i.imgur.com/Wv1DGgD.png">

## Demo
[Easy Garage Demo Presentation Video](https://youtu.be/FJEAstPQQpk)

## Lesson Learned

#### Jihao Zhang
*"I have learned abundant stuff on this course. In Homework, Lessons and Prjects, I have grown from a fresh guy who doesn't know anything about database to a student who can draw UML and ER grams to design a database model, perform SQL sentences to make CRUD operations on a database, use Procedures, Functions and Triggers make a database more robust, and write filter statements for MongoDB. Besides, I can also talk about what the difference between Relational Database, and Non Relational Database is, what is SQL and what is NoSQL, what Dead Lock is and how to avoid them, how to recover a database and so on. After the project I can even better use SQL and JPQL in Java and Spring Framework to let database could be greatly connected and managed by a real application. Also, the assignments, exams and project are all included in the course, which are abundant but also kind of a heavy work load for me. As a result, I need to make my learning more effective by searching for extra learning resources like videos and ebooks to help me understand better and finish my weekly homework on time. Also, the time of exams are also very limited, so it's also a practice for me to be better at time managing and scheduling. Moreover, I also trained my skills of communicating and team work in the final project, To conclude, no matter the techs, concepts or learning methods, communications, learning methods, this course helped me more or less to become a better myself. Thanks!"*

#### Xiaowei Gu
*"From this course, I grew up a general understanding of the database. I leanrned how to use Unified Modeling Language (UML) as well as MySQL EER modeling tool. I learned how to use algebra operations to simplfied sql queries. From the assignments I completed, I learned how to send out commands to upgrate the information in the database. I have a very deep understanding on the SQL DBMS and noSQL DBMS. And I have practical experience about  connecting a computer application to a commercial relational database (MySQL) and access and manipulate data from the database within the application. Besides that, I learned how to use tools like mongoose, workbench. This lesson is very helpful and interesting. I had a very good time with it in this semester.   
"*

## Future Work

1. Triggers will be added to database to update the status and columns of reservations and parking records automatically to avoid dirty data in database and wrong cost value.
2. Make procedures of Places creating could be called by application each time when initializing to avoid dirty Place data records, which means the unreachable Places.
3. Add functions to database to initialize the whole database and make the size of Garage (number of floors, number of Places on each floor, number of Places for each type of Vehicle, number of Vehicle type...) editable.
4. Different authentications for account type, such as: Owner, Manager, Administrator and User could be added to the app to make the management system more robust.
5. Provide upgrade services for normal users to become VIP users to get better services and discounts when parking.
6. Make Profile Image editable and add more columns for User to save more information and create better using experience.

## Acknowledgment
