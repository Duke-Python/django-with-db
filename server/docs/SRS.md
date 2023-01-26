[Back to server](README.md)\
[Back to main](/README.md)

# Server Software Requirements

## Overview

## Database Schemas
Database schemas (models) are specified as subclasses of the Flask-SQLAchemy Model class (SQLAlchemy.Model). New database rows are saved by creating database models, then inserting these models into the database. Validation of column types and model constraints are contained in the database models. Invalid
inputs raise a ModelError.

### Addresses
Address creation requires a city, a state, and a zip code. House number and street name are optional. The city, state, and street names are strings while the zip code and house number are integers. Creating an Address using either missing inputs or inputs of the wrong type causes a ModelError.

Along with the type of data contained in the Address object, there are character length constraints and a valid range of zip codes. Any updates to the address fields that do not follow these constraints causes a ModelError.

Addresses in the database can be modified and can only be deleted if no users have the address.

### User Roles
Creating a User_role object requires the user role, a string describing one of the allowable user roles. Creating a User_role without specifying the user_role results in a TypeError. Specifying an invalid user role or specifying a non-string for the user role results in a ModelError. Since the priority is directly tied to the user role changing the user role results in changing the priority. The priority can only be specified by changing the user role. Attempting to directly change the priority results in a ModelError.

User roles in the database cannot be modified but can be deleted if no users have that role.