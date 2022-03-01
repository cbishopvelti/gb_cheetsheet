
list users
```
dscl . list /Users | grep -v "^_" 
```
delet Users: 
```
dscl . -delete "/Users/[username]"
```


### Create user
```
CREATE USER airflow WITH ENCRYPTED PASSWORD 'airflow';
GRANT ALL PRIVILEGES ON DATABASE airflow TO airflow;
```