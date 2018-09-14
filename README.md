# notes

#### bash script for posgres db connection
/usr/bin/env bash
docker-compose exec postgres  \
psql -d app -U app_user -A -F"," -P footer=off -c "SELECT * FROM table"
________________________________________
#### to get string from file
$(cat session.txt | awk 'NR == 1') # to get string from file to cli linux/unix
