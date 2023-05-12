```
net user newuser Secre1P@ssword /add
net localgroup QlikComposeAdmins
net localgroup QlikComposeAdmins newuser /add
```

Open file ```C:\Program Files\Qlik\Compose\data\projects\GlobalRepo.sqlite``` with dbeaver and edit the last column (it's a json string)of the record with type "AuthorizationAclDto" to match your local machine name (as the domain) and the new user name created.