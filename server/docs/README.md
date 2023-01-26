[Back to main](/README.md)

# Server Application

## Overview
The server application is the interface between information storage in the database and the applications that interact with that data. The server interface consists of a number of routes that different applications can call to create, read, update, and delete the database information. A large number of routes are POSTs as extra security.

## Server Routes
These server routes allow applications to interact with the database used to store user information.

* `POST /names`
  takes a JSON input of the current user id and the types of users and returns a JSON with a list of names and ids.  Only user types that are specified in the input JSON are listed in the output JSON.

  The JSON input contains a list of user types requested
  ```
  {
      "current_user_id": current_user_id
      "role": one of "patients", "doctors", or "administrators"
  }
  ```
  The returned JSON contains the type of user and a list of those users.
  ```
  {
      "role": one of "patients", "doctors", or "administrators"
      "users": dictionary of users (ids: names)

  }
  ```

* `POST /user/<user_id>`
  takes a JSON input of the current user id in the input JSON and returns information related to the requested user_id. The retrieved user information is limited by the <current_user_id>. Restricted values are returned as `null`. The input JSON is:
  ```
  {
      "current_user_id": <current_user_id>
  }
  ```
  The output JSON contains the general user information, the latest heart rate information, and a history of ECG and medical images. To save bandwidth, the history is a list of image ids and their time stamps.
  ```
  {
      "user_id": Requested user id
      "user": `<User JSON>`
      "heart_rate": `<Heart rate JSON>`
      "past_ECG_data": dictionary of ECG files (ids: file names)
      "past_med_images": dictionary of medical images (ids: time stamps)
  }
  ```
  The `<User JSON>` has the following fields:
  ```
  {
    "name": User name as a string
    "role": User role
    "address": User address as a dictionary
  }
  the address dictionary has keys for street, city, state, and zip.
  The `<Heart rate JSON>` has the following structure:
  ```
  {
    "heart_rate": Latest heart rate as an int
    "ECG_data": Latest ECG data file associated with this heart rate (optional)
    "datetime": Time stamp when ECG image was saved
  }

* `POST /user_since/<user_id>`
  takes a JSON input of the current user id and requested time and returns information related to the requested user_id. The retrieved user information is limited by the <current_user_id>. Restricted values are returned as `null`.
  ```
  {
      "current_user_id": <current_user_id>
      "datetime": date/time to retrieve information
  }
  ```
  The returned data is contained in a JSON:
  ```
  {
    "user_id": Requested user id.
    "latest_heart_rate": <Heart rate JSON>
    "heart_rates": Dictionary of heart rates since given dateime (id: time stamp)
    "ECG_images": Dictionary of ECG image since given datetime (id: time stamp)
    "med_images": Dictionary of medical images since given datetime (id: time stamp)
  }
  ```

* `POST /image/<image_type>/<image_id>`
  takes a JSON input of the current user id and the requested user id returns either an ECG image or a medical image depending on the <image_type>. The retrieved user information is limited by the <current_user_id>. Restricted values are returned as `null`.
  ```
  {
      "current_user_id": <current_user_id>
      "user_id": Requested user id
  }
  ```
  The returned JSON has the following structure:
  ```
  {
      "user_id": Requested user id
      "image_id": Requested image id
      "image_type": Either "ECG" or "medical"
      "image": String of bit64 characters
  }
  ```

* `POST /add_user`
  takes a JSON input of the current user id and user information and adds a new user to the database. The presence of restricted fields will not add the user.
  ```
  {
      "user": <User JSON>
      "heart_rate": <Heart rate JSON>
      "med_image": medical image
  }
  ```
  The medical image is a string of bit64 characters.
  The `<User JSON>` has the following fields:
  ```
  {
    "user_name": Log in name as a string
    "password": Encrypted password
    "name": User name as a string
    "role": User role
    "address": User address as a dictionary
  }
  ```
  the address dictionary has keys for street, city, state, and zip.
  The `<Heart rate JSON>` has the following structure:
  ```
  {
    "heart_rate": Latest heart rate as an int
    "ECG_image": Latest ECG image associated with this heart rate (optional)
  }

* `POST /update_user/<user_id>`
  takes a JSON input of the current user id and user information and updates the user id information in the database. The presence of restricted fields will not update the user.
  ```
  {
      "current_user_id": current user id
      "user": <User JSON>
      "heart_rate": <Heart rate JSON>
      "med_image": medical image as a string of bit64 characters
  }
  ```
  The `<User JSON>` has the following fields:
  ```
  {
    "name": User name as a string
    "role": User role
    "address": User address as a dictionary
  }
  the address dictionary has keys for street, city, state, and zip.
  Heart rate and medical image data is added to the existing data, while other data replaces the exising data.

## Database Schemas
The user information is stored in a relational database using SQLite. The naming convention uses plural for table names, `id` for primary keys, and <table_name>_id for foreign keys. <table_name> is the singular form of the table name.

### User Roles
```
role (pk, varchar(50)) restricted to "patient", "doctor", or "administrator"
priority (int) not null
```

### Users
```
id (pk)
name (varchar(50)) not null
user_name_id (fk) not null
address_id (fk) not null
user_role_id (fk) not null
added_by_user_id (fk) not null
```

### User Names
```
user_name (pk)
password (encrypted string)
```

### Addresses
```
id (pk)
house_no (int)
street (varchar(50))
city (varchar(50)) not null
state(varchar(50)) not null
zip_code (int) not null; range 00001 – 99950
```

### Heart Rates
```
id (pk)
user_id (fk) not null
heart_rate (int) not null
ECG_data_id (fk)
time_stamp (datetime) not null
```

### ECG Data
```
id (pk)
file_name (str) not null
file_type (str)
```

### Medical Images
```
id (pk)
user_id (fk)
image (text) bit64 string, not null
time_stamp (datetime) not null
```

## Database Migrations
This repository uses flask-migrate to migrate changes to the SQLite database schema. After initializing the database, flask-migrate migrations require two (2) steps. The first step creates a script to change the database schema, while the second step actually applies the schema change. This repository uses a database for testing and a database for development/production. In order to maintain the same schema in both databases we make slight changes to the generic initialization and migrations. To initialize the migrations run the following command:
```
flask --app run_server.py db init --multidb
```

Once the database migrations are initialized, updates to the schema occur as follows:
```
flask --app run_server.py db migrate -m <comment>  # create the script
flask --app run_server.py db upgrade  # apply the script
```
where `<comment>` is a string that is used in the title of the migration script. However, since we're applying migrations to two databases we need to edit the migration file. This requires adding the generic upgrade and downgrade functions to the specific upgrade and downgrade functions.

Constraints to the database models are not recognized by flask-migration and need to be manually added to the migration script. See the [constraints file](Constraints.md) for specific constraints used in this project.

## Software Requirements
Implementation requirements for the database schemas and the JSON schemas are outlined in the [server software requirements](SRS.md) document.