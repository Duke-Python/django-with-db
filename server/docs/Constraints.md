[Back to SRS](SRS.md)\
[Back to server](README.md)

## Address Table
```
sa.CheckConstraint("(zip_code >= 1) and (zip_code <= 99950)", name="address_zip_code")
```

## User Role Table
```
sa.CheckConstraint('(role == "patient") or (role == "doctor") or (role == "administrator")', name="user_role_role")
```
