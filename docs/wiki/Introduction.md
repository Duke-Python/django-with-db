# Patient Monitoring Server/Client Project

## Overview
This project extends the Fall 2022 BME547 patient monitoring server/client project. The project integrates two clients and a server to input and monitor patient heart rate data.

The project has three (3) levels of users; administrators, doctors, and patients. Administrators have full access to the system, while doctors can add patients, patient information, and view all patients and their information. Patients can add themselves and certain information to the system.

Personal information consists of an address, a specified user role, a user name, and a password. The address contains a minimum of their zip code, city, and state.

Patients access the system with a user name and password. Patients have personal and medical information associated with them and can add themselves to the system. New patients can add themselves to the system. They can update their contact information and also upload new average heart rate. Patients can view their information but no other patient or doctor. Patients are assigned a single attending doctor.

Doctors access the system with a user name and password. Doctors have personal information and zero or more patients that they attend to. New doctors can add themselves to the system. They can update all patient contact information, add new patients and their data, and view all patient information. Doctors can view their own information but not other doctor information.

Administrators access the system with a user name and password. They can view all information in the system and perform all patient and doctor tasks. In addition, adminstrators can add new administrators to the system and remove patients from the system.

## Applications
The system is divided into two (2) distinct parts, a monitoring interface and a patient interface. The monitoring interface is a web-based app that allows either doctors or administrators to examine patient heart rate data. The monitoring interface does not allow changes to the patient data. The patient interface is a web-based app that allows doctors and patients to add new patient data or update existing patient data.

### Monitoring Application
The monitoring interface allows doctors and administrators to specify an existing patient and examine both the most recent heart rate information and the historical heart rate information. If new information for the selected patient is added by the patient interface then the monitoring interface will inform the user that new data is available. The monitoring interface can display up to two (2) ECG images and allow these images to be saved to a local computer. Detailed requirements for the monitoring interface can be found [here](monitor_client/docs/README.md).

### Patient Interface Application
The patient interface allows doctors, patients, and administrators to add new users (patients, doctors, and administrators), add and update user records, and delete usersand their records. Changes to the user records depends on the type of user. Patients can add themselves and their average heart rate. Doctors can add themselves and their information, add patients, patient information and their own information. Administrators can add patients, doctors, and administrators, update patient records, and delete patient records. Detailed requirements for the patient interface can be found [here](doc_client/docs/README.md).

### Server Application
Patient records are stored in a relational database. The monitoring interface and the patient interface do not directly interact with the database but rather interact with a series of server routes which, in turn, interact with the database. Detailed requirements for the server, including the database, can be found [here](server/docs/README.md).

## Installation
TBD.
