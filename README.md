# driveFS_DUMP
Simple tool to extract basic forensically relevant information from Google Drive FS artifacts and recover cached content.

Context
=======

Suspects of breach of confidence sometimes keep relevant information in their own personal Google Drive File Stream.
In these cases, it is of interest to identify and extract evidence regarding presence and use of confidential information.

In Windows systems, Google FS artifacts reside under "%USERPROFILE%\AppData\Local\Google\DriveFS" (see https://support.google.com/a/answer/7644837).

Two databases are of interest: the "root_preference_sqlite.db" and the "metadata_sqlite_db" under every "profile" folder.
A profile id is a 21 length numerical code.
From the root preference DB we can extract the devices bound to the service.
From the metadata DB we can get all the information needed to list the content hosted and try to recover the cached information to a proper directory structure.

The app caches files in a two level folder structure under "content_cache", in the profile directory.
The stored files have a 6 digits name and reside in a two levels folder structure of nodes named "dXXX".

Usage
=====

googleFS_DUMP_.py -p PROFILE -c CSV_DESTINATION [-r] RECOVERY_DESTINATION [-d]

Requires a PROFILE folder that contains a "metadata_sqlite_db" DB.
It reads and reconstructs the hosted directory structure, up to the root node (stable_id = 101, named "My Drive") and the corresponding "viewed_by_me_date" values.
Outputs to a csv named "driveFS-fullPath_viewDate-{googleId}_{TimeStamp}.csv" under the provided CSV_DESTINATION folder.

If a recovery destination is provided, it reads the cached files, gets their corresponding names and location from the database and copies them to a proper folder structure.

"-d" generates a list of devices bound to the profile.

DISCLAIMER:
Python is not my native language but I tried to minimize queries and take advantage of Python data structures to make the process more agile.
The results are fit for my actual purposes, but the tool wasn’t thoroughly tested. Use it at your own risk.
Also, I am unaware of any variability on the Google Drive FS artifacts.
