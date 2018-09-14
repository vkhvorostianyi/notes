# notes

## bash script for posgres db connection
/usr/bin/env bash
docker-compose exec postgres  \
psql -d app -U app_user -A -F"," -P footer=off -c "SELECT * FROM table"

________________________________________
