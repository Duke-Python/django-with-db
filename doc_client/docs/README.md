[Back to main](/README.md)

# Patient Interface Application

## Overview
Patient interface requirements.

## Patient Interface Routes

* `GET /`
  displays the login page. The login page requires a user_name and a password. If the user_name doesn't exist in the system, then the user is added as a patient.

* `POST /patient`
  displays the patient interface page based on the input JSON. The patient interface page allows users to add users, heart rate information, and medical images to the system. Only administrators can add new users on this page.
  Images stored on the local computer can be uploaded to the system.
  The input JSON has the following structure:
  ```
  {
      "user_name": string of characters
      "password": string of encrypted characters
      "role": one of "patient", "doctor", or "administrator"
  }
  ```