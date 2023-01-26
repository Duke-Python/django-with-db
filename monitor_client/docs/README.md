[Back to main](/README.md)

# Monitoring Application

## Overview
The client used to monitor patients and their medical information is a web-based application. Its primary use is to allow doctors to examine heart rate information and medical images. The monitor client uses different routes to communicate with the server that interfaces with the database that contains the patient data. While running, the client periodically polls the database server for new information about a patient.

The client allows the user to select a patient and see their latest heart rate information. The client can display the latest ECG image and a chosen historical ECG image. The client can also display a given medical image. All chosen images can be saved to the local computer for later analysis.

## Monitor Client Routes

* `GET /`
  displays a login page. A user must have an existing account in order to log in.

* `POST /monitor`
  displays a list of patients that can be monitored based on the input JSON. If a doctor or administrator is logged in then the user can monitor any patient. If a patient is logged in then the user is restricted to their medical information.

  Choosing a patient displays their personal information and their latest heart rate information. The user has the option to display another ECG image alongside the current ECG image and previously saved medical image. Any of these images can be saved to the local computer.

  The user can also change the patient they are observing, at which point the monitor will display the new patient's information.

  Finally, the monitor will inform the user if any new heart rate information is entered into the system. This time between heart rate information updates is 30 seconds.

  The input JSON has the following structure:
  ```
  {
    "user_name": string of characters
    "password": string of encrypted characters
    "role": one of "patient", "doctor", or "administrator"
  }
  ```