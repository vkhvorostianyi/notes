# notes

#### bash script for posgres db connection
/usr/bin/env bash
docker-compose exec postgres  \
psql -d app -U app_user -A -F"," -P footer=off -c "SELECT * FROM table"
________________________________________
#### to get string from file
$(cat session.txt | awk 'NR == 1') # to get string from file to cli linux/unix

_________
#### sql queries
select * from crop where crop.updated between '2018-05-06' and '2018-05-08' and crop.user is '';
_____
#### flask
pip install virtualenv
virtualenv venv -p python3.6
source venv/bin/activate 
pip install -r requirements.txt
export FLASK_APP=task.py
python task.py
________
### docker
sudo groupadd docker 
sudo usermod -aG docker $USER
cd tool/ docker build -t <short docker image name> .
docker run -d -v ~/fr_office_frames:/app/static/frames -p 8000:8000 stable_build 
docker run -v ~/a:/app/static/frames -v ~/b:/app/static/frames/frames10052018 -d -p 8000:8000  test_image
docker container start <existing container name>
docker cp heuristic_engelbart:/app.db app.db
docker stop container
docker save -o <path for generated tar file> <image name>
docker load -i <path to image tar file>

docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
docker rmi $(docker images -a -q)
docker images -f 'dangling=true' -q
docker run -d -v ~/fr_office_frames:/app/static/frames -v ~/fr_office_frames_10052018:/app/static/frames/new_frames -p 8000:8000 fixed_bb
docker run -d -v ~/fr_office_frames_10052018:/app/static/frames/new_frames it --env DBPASS="p@ss@!worD" --env DBHOST="localhost:5432" --env DBUSER="app_user" --env DBNAME="app" -p 8010:8000 posgres_version
docker-compose  exec app bash 
docker-compose exec postgres  psql -U app_user app 
__
#### Ubuntu
which version
lsb_release -a
find DIR_NAME -type f | wc -l #count files in dir
diff -r ~/fr_office_frames ~/fr_office_frames_10052018/ | wc -l #get duples list
docker-compose  exec app bash 
du -sh # disk usage statistic
dpkg-reconfigure tzdata  #change timezone
__
#### postgres
 \c testdatabase
