# Bulbul Api

A Minimal Api written in .NET 7.

## Description

A minimal api that aims to become a social media. Uses MySql for database. In the beginning i went with entity framework core. Using link queries is awesome. Using link methods looks more readable to me. Performance tests will be do in future. After ef core i tried to use Dapper. With dapper i have to write raw sql and in my opinion this so cool. I had to understand how to make manually left-right joins. External sources says dapper's performance metrics so much better but i haven't tested yet. Using docker for hosting on our external server is almost crucial general way so i use docker for publishing. I will write a Android app for this api. Stay tuned!

## Getting Started

### Dependencies

* Microsoft.JwtBearer
* NewtonsoftJson
* MySql.Data
* <strong>Dapper</strong>
* <strong>Docker</strong> for running

### What do offers is Bulbul Api?
* User Registiration
* Post Creation
* Media Uploading
* User Following
* Like/Unlike posts
* Caching for decreasing response time

### How is infrastructure
Because this project uses MySql, all structure based on relational data. You can post a Post over a Post like tweeting over a tweeting (ReTweet). You can search for who is following you or who am i following. Who like or unlike this post. How much did I post.

### All endpoints
* <strong>/auth</strong>
   - /login - POST
   - /register - GET 
* <strong>/user</strong>
  - /{id} - GET
  - /bio - PUT (Can be move for more logical endpoint)
* <strong>/posts</strong>
   - / - POST - Create a Post
* <strong>/reaction</strong>
   - / - POST - Create a Reaction
   - isCancel - default false, This is stands for user if undo it's operation
* <strong>/media</strong>
   - /{id} - POST - Upload a media for post
   - /download/{id} - GET - Download media with id
   - /{post_id} - GET - Get all media entity owns by specified post_id
 - <strong>/follow</strong>
   - / - POST - Follow a user, has isCancel
 - <strong>/following</strong>
   - /{id} - GET - Gets all following user that specified user_id
 - <strong>/follower</strong>
   - /{id} - GET - Gets all follower user that specified user_id

### Overview of Data
This tables represents data structures are used in project
Nullable fields are specified
* <strong>User</strong>
  * id : varchar(36) , PRIMARY, UUID()
  * user_name : varchar(127)
  * show_name : varchar(127)
  * biography_info : varchar(255)
  * follower_count : bigint
  * following_count : bigint
  * profile_picture_url : varchar(255)
  * create_time : datetime = CURRENT_TIME_STAMP
  * post_count : bigint
    * Constraints
      * user_name and show_name must be unique

* <strong>UserCredentials</strong>
  * Credential related data
* <strong>Posts</strong>
  * id : varchar(36), PRIMARY, UUID()
  * user_id : varchar(36), FK in Users Table
  * text_content : varchar(255), NULL
  * like_count : bigint
  * create_time : datetime = CURRENT_TIME_STAMP
  * child_post_id : varchar(36) : FK in Posts table
  * parent_post_id : varchar(36) : FK in Posts table
* <strong>Medias</strong>
  * id : varchar(36), PRIMARY, UUID(),
  * post_id : varchar(36)
  * address : varchar(255)
  * create_time : datetime
* <strong>Reactions</strong>
  * id : int, PRIMARY, AUTO INCREMENT
  * user_id : varchar(36), FK in Users Table
  * post_id : varchar(36), FK in Posts Table
  * reaction_type : enum('like','unlike')
  * created_at : timestamp = CURRENT_TIMESTAMP
    * Constraints
      * User can not give a reaction more than once
      * User can not give like and unlike same time 
* <strong>Follows</strong>
  * id : bigint, PRIMARY, AUTO INCREMENT
  * source_user_id : varchar(36), FK in Users
  * target_user_id : varchar(36) FK in Users
  * time_stamp : timestamp
    * Constraints
      * User can not follow one user more than once

### Triggers
This triggers set for data consistency
*  Auto increment post_count when a user post a post / auto decremenet same way
*  Prevent same user gives a reaction like and unlike same time
*  On undo and on give reaction increment or decrement posts like_count and unlike_count
*  increase and decrease follower_count on following
*  increase and decrease following_count


## Help
Please contact me on: ruchanadiguzel1@gmail.com

## What gave me this project
A General acknowledgment of database trigger and other constraints and api design for this kind of database design. 
Raw sql queries writing skill is improved.
