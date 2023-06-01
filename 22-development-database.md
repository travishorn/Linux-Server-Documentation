---
label: Using a Development Database
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -22
---

# Using a Development Database

## Prerequisites

- A production database in MariaDB

## Create a Sync Script

Instead of creating a development database by typing in commands manually, it is
better to create a script. This way, you can sync the development database with
the production one at any time with one command.

Create a sync script at `/usr/local/bin/sync_dev_db.sh`.

```sh
#!/bin/bash

# Duplicates production database to a development database.
# WARNING: All data on the development database will be lost!

# Enable errexit so the script stops as soon as it encounters an error
set -e

PROD_DB="[production database name]"
DEV_DB="${PROD_DB}_dev"
DUMP_FILE="/tmp/${PROD_DB}_$(date -u +"%Y-%m-%dT%H:%M:%SZ").sql"

# Check if the development database already exists
echo "Checking if development database already exists..."

if mariadb -e "USE $DEV_DB";
then
  # Development database already exists, ask to continue before dropping it
  echo "Development database '$DEV_DB' already exists."
  read -p "Remove all its data and replace it with a copy of the production database '$PROD_DB'? (y/N) " answer

  if [[ $answer = y ]] ; then
    # The answer is yes. Drop the database
    echo "Dropping existing development database..."
    mariadb -e "DROP DATABASE $DEV_DB;"
  else
    # The answer is no. Abort the script
    echo "Aborted."
    exit 0
  fi
else
  # Development database does not exists. No action needed
  echo "No existing development database found."
fi

# Create the development database
echo "Creating development database..."
mariadb -e "CREATE DATABASE $DEV_DB;"

# Dump the production database schema and data to file
echo "Dumping production database to temporary file..."
mysqldump $PROD_DB > $DUMP_FILE

# Import the dump file into the development database
echo "Importing temporary file into development database..."
mariadb $DEV_DB < $DUMP_FILE

# Delete the dump file as it's no longer needed
echo "Removing temporary file..."
rm $DUMP_FILE

echo "Done."
```

Make sure you replace `[production database name]` in the script above with the
actual name of your production database.

Make the script executable.

```sh
sudo chmod +x /usr/local/bin/sync_dev_db.sh
```

## Create/Sync the Development Database

!!!warning
The method in this section will erase all existing data in the development
database, if it exists.
!!!

Run the sync script.

```sh
sudo sync_dev_db.sh
```

A database that is an exact duplicate of your production database now exists in
MariaDB. The name has `_dev` appended to it. For example, if your production
database is called `mydatabase`, there is now a database called
`mydatabase_dev`.

Run this script any time you want to sync the data from production to
development.
