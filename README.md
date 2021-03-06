# notes
#### ssh 
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
#### cuda related 
```bash
sudo ldconfig /usr/local/cuda/lib64
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:/usr/local/cuda-10.0/lib64
```

##### tensorboard
```bash
export TMPDIR=/tmp/$USER; mkdir -p $TMPDIR; tensorboard --logdir=. --port=1025
```
##### ssh-commands
```bash
ssh-keygen -t rsa
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
ssh-add -k ~/.ssh/id_rsa
pbcopy < ~/.ssh/id_rsa.pub

```

##### conda create env
```bash
wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh
conda create -n anti-aliased  python=3.6 -y
```
##### bind the jupyter-notebook
```bash
jupyter notebook --no-browser --port=8889
ssh -N -f -L localhost:8888:localhost:8889 username@your_remote_host_name
python -m ipykernel install --user --name extd --display-name "extd" # add invornment
```
#### Python tips

```python 
sum([[1],[2]], []) # => [1,2]
```

```python
except Exception as e:
        logging.fatal(e, exc_info=True)  
        log.append([k,len(iou_dict), e.__doc__])
 ```
 ```python
 import io
from contextlib import redirect_stdout

with io.StringIO() as buf, redirect_stdout(buf):
    print('redirected')
    output = buf.getvalue()
```
```python
from boto3.session import Session
import configparser

ACCESS_KEY = cnfg['AWSacc']['aws_access_key_id']
SECRET_KEY = cnfg['AWSacc']['aws_secret_access_key']

session = Session(aws_access_key_id=ACCESS_KEY,
                  aws_secret_access_key=SECRET_KEY)
s3 = session.resource('s3')
bucket = s3.Bucket('backet_name')
prefix = 'path/to/dir/'

bucket.objects.filter(Prefix=prefix)
```
 
#### Octave/Matlab
```matlab
g = (1 ./ (1 .+ e.^(-z))) % elementwise operations
func = @(x) (1 ./ (1 .+ e.^(-x))); g = func(z);
```

#### RedShift sql
```sql
select json_extract_path_text(json, 'key1', 'key2') from table 
--
select row_number() OVER(PARTITION BY device_id ORDER BY timestamp DESC) AS rn
--
select id from table where "timestamp" >= dateadd(day,-1,(SELECT min(date("timestamp")) FROM table)) AND "timestamp" <= (SELECT max("timestamp") FROM table
--
create or replace view schema.new_tbl as select * from tbl where "timestamp" >= dateadd(day,-7,(SELECT max(date("timestamp")) FROM  tbl))
--
select CURRENT_DATE - 7 -- last week
--
select id, type, 
case when regexp_substr(values, '[0-9\\.]+') in ('', NULL) then 0 else regexp_substr(values, '[0-9\\.]+')::real end
as column_name
from table
--
mode_table as (
SELECT device_id, mic_diag 
FROM (SELECT device_id, mic_diag, RANK() OVER(PARTITION BY device_id ORDER BY cnt DESC) rnk
     FROM (SELECT device_id, mic_diag, COUNT(*) AS cnt
           FROM tbl c
           GROUP BY 1,2) AS s1) AS s2
WHERE rnk = 1
)
--
select salesid, pricepaid::decimal(38,2)*100000000000000000000
as value from sales where salesid<10 order by salesid;
--
SELECT created_at, id, (lower(replace(mac_id,':','')) from table
--
select id where 
 (CASE WHEN round(loc.latitude ,4) = 34.05
           AND round(loc.longitude,4) = -118.24 
           THEN 0 
           ELSE ACOS(SIN(RADIANS(34.05)) * SIN(RADIANS(round(loc.latitude,4) )) + COS(RADIANS(34.05)) * COS(RADIANS(round(loc.latitude,4) )) * COS(RADIANS(round(loc.longitude,4)  - (-118.24) ))) * 6371 
    END <= 25.0)
--
SUBSTRING ( MyColumn, 1 , 1 ) for the first character and SUBSTRING ( MyColumn, 1 , 2 ) for the first two.
```
#### Athena sql
```sql
select date_parse(dt, '%Y-%m-%d %H') as dt from table
--
select CAST(DATE_ADD('day', -5, DATE(NOW())) as varchar) as dt from table
--
select *,
rank() OVER (PARTITION BY device_id ORDER BY timestamp DESC) AS rnk
from table
--
select date_parse(ts,'%Y-%m-%dT%H:%i:%s.%fZ') as ts,
       split_part(split_part(thread,'.',1),'[',1) as details
--
select date(date_parse(CAST(ts as varchar), '%Y-%m-%dT%H:%i:%s.%fZ')) as run_date from table
--
select json_extract_scalar(json, '$["@timestamp"]') AS timestamp from table
--
with tbl as (
  select *,
  rank() OVER (PARTITION BY device_id ORDER BY dt DESC) AS rnk
  from table
  where type = 'prod_type'
  and dt > to_iso8601(current_date - interval '48' hour)
  and id in (4,9) 
)

select * from tbl
where tbl.rnk in (1,2,3,4,5,6,7,8,9,10)
order by id
--
select CAST(regexp_extract(json_extract_scalar("json",'$.data.some_num'), '[-+]?\d*\.\d+|\d+') AS decimal) as num from table
--
select
      DATE_FORMAT(from_unixtime(cast(json_extract_scalar(logs.json, '$["context"]["eventOccurredTsMs"]') as bigint)/1000) , '%Y-%m-%d') as timestamp_date
--
```

#### numpy
```python
a = np.array([0, 0, 0, 0, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1], dtype=np.bool)
# or
b = ~a
b = np.logical_not(a)
```
#### pandas 
```python
df = pd.DataFrame(np.random.randint(0,2,size=(100, 3)), columns=list('ABC'))
s1 = pd.Series(['Mouse', 'dog', 'house and parrot', '23', np.NaN])
s1.str.contains('og', regex=False)
df[df.a < df.a.quantile(.95)]
```
#### python FAQ
```python
files = filter(os.path.isfile, os.listdir( os.curdir ) )  # files only
```
```
>>> print(", ".join(["ha" if i else "Ha" for i in range(3)]) + "!")
Ha, ha, ha!
merged.columns = [x.replace('_x','_montana') if x.endswith('_x') else x.replace('_y','_bravo') if x.endswith('_y') else x for x in merged.columns]
```

```bash
export PYTHONPATH="${PYTHONPATH}:/my/other/path"
```
```python
# f-string float
f'{a:.2}'
```
```python
# multiprocessing

import pandas as pd
import numpy as np

from multiprocessing import Pool
from functools import partial

def get_report(df):
    res = []
    c = 0
    ids = df.device_id.unique()
    for i in ids:
        c += 1
        if c%1000==0:
            print(c)
        res.append((i, rules(df[df.device_id == i])))
    return np.array(res)


def prepare_file(df):
    index_first = int(df.index[0])
    temp_results = get_report(df)
    pd.DataFrame(temp_results).to_csv(
        f'~/part_{index_first}.csv',
        index=False)
    print(f'part {index_first} was proceeded')



all_df = pd.read_csv('~/Downloads/f.csv', dtype=str)

n = 100000  # chunk row size
list_df = [all_df[i:i+n] for i in range(0, all_df.shape[0], n)][6:13]

with Pool(processes=4) as pool:
    process_file = partial(prepare_file)
    pool.map(process_file, list_df)

```

#### matplotlib
```python
from matplotlib import pyplot as plt
import matplotlib
font = {'family' : 'normal',
        'weight' : 'bold',
        'size'   : 15}

matplotlib.rc('font', **font)
plt.figure(figsize=(20,20))
plt.title('Packets per second')
plt.xticks((0,1,10,50,10,250),size = 12)
plt.hist(exp.packets_per_second, bins=[0,1,10,20,50], facecolor='g', edgecolor='black', alpha=0.75)
```
#### docker file sample

```yml
# Pyhon version
FROM python:3.6

# Working directory
WORKDIR /app

# Copy current directory contents onto the container
COPY . /app
# Installation of requirements
RUN apt-get update && apt-get install -y default-jre nano
RUN pip install --no-cache-dir -r requirements.txt

# Define environment var
ENV NAME "docker"

# Make 8000 port
EXPOSE 80
# CMD command for run
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "wsgi", "--access-logfile", "/app_volume/gncrn_log.log", "--log-level", "debug"]
```

#### git commands
```bash
git add .
git status
git commit -m 'what was changed'
git push
git pull
git checkout -b new_branch # create new branch and checkout to it 
git reset 
git reset --hard
git reset --hard HEAD~1 rollback to one commit 
git push --force 
git diff
git branch
git push --set-upstream origin house_web_app
git merge hotfix
```

#### to change tz settings
```bash
dpkg-reconfigure tzdata
```

#### tar file save unpack
```bash
tar -czvf name-of-archive.tar.gz /path/to/folder-or-file   
tar -czvf files.tar.gz /usr/local/myfiles  
```
#### posgres config user for web app
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
 ALTER TABLE table_name RENAME TO new_table_name;
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
                    
from pyathena import connect
import pandas as pd

conn = connect(s3_staging_dir='s3://s3_dir')
df = pd.read_sql("SELECT * FROM schema.table where dt > '2019-03-10 00' LIMIT 100", conn)
print(df.head())

```

#### Mask algo 
```python
def genarate_mask(dha_report)
    states = np.ma.array(dha_report.columns[3:8])
    res = []
    state = []
    for i in dha_report.iloc[:,3:8].values:
        d = np.array([2 ** x  for x in range(len(i), 0,-1)])
        state.append(states[i.astype(bool)].data)
        res.append(sum(d*i))
    dha_report['verbal'] = state
    dha_report['state'] = res
return dha report
dha_report[(dha_report.state < 32) & (dha_report.state > 16) & (dha_report.type == 'doorbell_v4')].iloc[:,[0,1,8,9]]
```

#### RedShift connection
```python
def get_con_redshift():
    import psycopg2
    import configparser
    import os
    rs_cnfg = configparser.ConfigParser()
    rs_cnfg.read(f'{os.path.expanduser("~")}/.redshift/config')
    
    con = psycopg2.connect(dbname=rs_cnfg["DEFAULT"]['dbname'],
                       host=rs_cnfg["DEFAULT"]['host'], 
                       port=rs_cnfg["DEFAULT"]['port'], 
                       user=rs_cnfg["DEFAULT"]['user'], 
                       password=rs_cnfg["DEFAULT"]['password'])
    return con
    ```
