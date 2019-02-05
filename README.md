# notes

#### to change tz settings
```bash
dpkg-reconfigure tzdata
```

#### tar file save unpack
```bash
tar -czvf name-of-archive.tar.gz /path/to/folder-or-file   
tar -czvf files.tar.gz /usr/local/myfiles  
```
#### posgres config user fro web app
```bash
sudo su postgres
psql
create database testdb;
create user test_user with password 'test_psw';
ALTER ROLE test_user SET client_encoding TO 'utf8';
ALTER ROLE test_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE test_user SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE testdb TO test_user;
```

#### sutup python dependencies ubuntu server
```bash
sudo apt-get install -y \
build-essential autoconf libtool \
pkg-config python-opengl python-imaging \
python-pyrex python-pyside.qtopengl idle-python2.7 \
qt4-dev-tools qt4-designer libqtgui4 libqtcore4 \
libqt4-xml libqt4-test libqt4-script libqt4-network \
libqt4-dbus python-qt4 python-qt4-gl \
libgle3 python-dev python3-dev libssl-dev \
```
#### cli progress bar
```python
# Print iterations progress
def printProgressBar (iteration, total, prefix = '', suffix = '', decimals = 1, length = 100, fill = '█'):
    """
    Call in a loop to create terminal progress bar
    @params:
        iteration   - Required  : current iteration (Int)
        total       - Required  : total iterations (Int)
        prefix      - Optional  : prefix string (Str)
        suffix      - Optional  : suffix string (Str)
        decimals    - Optional  : positive number of decimals in percent complete (Int)
        length      - Optional  : character length of bar (Int)
        fill        - Optional  : bar fill character (Str)
    """
    percent = ("{0:." + str(decimals) + "f}").format(100 * (iteration / float(total)))
    filledLength = int(length * iteration // total)
    bar = fill * filledLength + '-' * (length - filledLength)
    print('\r%s |%s| %s%% %s' % (prefix, bar, percent, suffix), end = '\r')
    # Print New Line on Complete
    if iteration == total: 
        print()

# 
# Sample Usage
# 

from time import sleep

# A List of Items
items = list(range(0, 57))
l = len(items)

# Initial call to print 0% progress
printProgressBar(0, l, prefix = 'Progress:', suffix = 'Complete', length = 50)
for i, item in enumerate(items):
    # Do stuff...
    sleep(0.1)
    # Update Progress Bar
    printProgressBar(i + 1, l, prefix = 'Progress:', suffix = 'Complete', length = 50)

# Sample Output
Progress: |█████████████████████████████████████████████-----| 90.0% Complete
```

#### bare repo 
```bash
cd repo
mv .git ../repo.git # renaming just for clarity
cd ..
rm -fr repo
cd repo.git
git config --bool core.bare true
```
#### bash script for posgres db connection
```bash
/usr/bin/env bash
docker-compose exec postgres  \
psql -d app -U app_user -A -F"," -P footer=off -c "SELECT * FROM table" > output.csv
```
```python
#python
con = psycopg2.connect("dbname=app user=app_user password=p@ss@!worD")
    df = pd.read_sql('''
                SELECT 
                DISTINCT user from table''')
```
```python
virtualenv -p python3 envname
```
_________
#### sql queries
```sql
select * from crop where crop.updated between '2018-05-06' and '2018-05-08' and crop.user is '';
```
_____
#### flask
```bash
pip install virtualenv
virtualenv venv -p python3.6
source venv/bin/activate 
pip install -r requirements.txt
export FLASK_APP=task.py
python task.py
```
________
#### docker
```bash
sudo groupadd docker  
sudo usermod -aG docker $USER  
cd tool/ docker build -t <short docker image name> .  
docker run -d -v ~/fr_office_frames:/app/static/frames -p 8000:8000 stable_build  
docker run -v ~/a:/app/static/frames -v ~/b:/app/static/frames/frames10052018 -d -p 8000:8000  test_image . 
docker container start <existing container name> . 
docker cp heuristic_engelbart:/app.db app.db . 
docker stop container  
docker save -o <path for generated tar file> <image name>  
docker load -i <path to image tar file>  

docker stop $(docker ps -a -q) \
docker rm $(docker ps -a -q)  
docker rmi $(docker images -a -q)  
docker images -f 'dangling=true' -q  
docker run -d -v ~/fr_office_frames:/app/static/frames -v ~/fr_office_frames_10052018:/app/static/frames/new_frames -p 8000:8000 fixed_bb  
docker run -d -v ~/fr_office_frames_10052018:/app/static/frames/new_frames it --env DBPASS="p@ss@!worD" --env   DBHOST="localhost:5432" --env DBUSER="app_user" --env DBNAME="app" -p 8010:8000 posgres_version  
docker-compose  exec app bash   
docker-compose exec postgres  psql -U app_user app
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container_name # get container ip
```
___
#### Ubuntu
```bash
which version  
lsb_release -a  
find DIR_NAME -type f | wc -l #count files in dir  
diff -r ~/fr_office_frames ~/fr_office_frames_10052018/ | wc -l #get duples list  
docker-compose  exec app bash  
du -sh # disk usage statistic  
dpkg-reconfigure tzdata  #change timezone  
$(cat session.txt | awk 'NR == 1') # to get string from file to cli linux/unix
sudo service postgresql stop
```
___
#### postgres
```sql
 \c testdatabase
 ALTER TABLE table_name
RENAME TO new_table_name;
 ```
___
#### AWS
###### Athena python connection
```python
from pyathenajdbc import connect
import pandas as pd
import os

conn = connect(access_key=os.environ['access_key'],
               secret_key=os.environ['secret_key'],
               s3_staging_dir='s3://path/to/s3',  
               region_name = os.environ['region'],  
               jvm_path='/Users/viacheslavkhvorostianyi/Downloads/AthenaJDBC42_2.0.5.jar') 


df = pd.read_sql("""SELECT dt, case when service = 'default' then 'backend_default' else service end,  
                    substring(json_extract_scalar(json, '$["@timestamp"]'), 1,16) as ts,  
                    count(1)as cnt,    
                    replace(substring(cast(from_unixtime(cast(json_extract_scalar(json, '$["some_key"]') as bigint)) as varchar),1,16),'    ','T') as consumed_at . 
                    FROM db  
                    WHERE dt = '2018-09-13 12'  
                    group by 1,2,3,5 """, conn)  
```
