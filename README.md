# Flask-MySQLPooled

Flask-MySQLPooled is a Flask extension that allows you to access a MySQL database by connection pool.

## Installation

Flask-MySQLPooled can be installed in the usual way:
```
python setup.py install
```    

It is even easier to download and install the package in one go using pip:
```
pip install Flask-MySQLPooled
```

## Configuration

Flask-MySQLPooled provides these settings:

| name                        | default value | remarks |
| --------------------------- | ------------- | ------- |
| MySQLPooled_DATABASE_HOST     | ‘localhost’   |         |
| MySQLPooled_DATABASE_PORT     | 3306          |         |
| MySQLPooled_DATABASE_USER     | None          |         |
| MySQLPooled_DATABASE_PASSWORD | None          |         |
| MySQLPooled_DATABASE_DB       | None          |         |
| MySQLPooled_DATABASE_CHARSET  | 'utf8'        |         |
| MySQLPooled_USE_UNICODE       | True          |         |
| MySQLPooled_DATABASE_SOCKET   | None          |         |
| MySQLPooled_SQL_MODE          | None          |         |
| MySQLPooled_MINCACHED         | 0             |         |
| MySQLPooled_MAXCACHED         | 1             |         |
| MySQLPooled_MAXCONNECTIONS    | 1             |         |
| MySQLPooled_BLOCKING          | False         |         |
| MySQLPooled_MAXUSAGE          | None          |         |
| MySQLPooled_SETSESSION        | None          |         |
| MySQLPooled_RESET             | True          |         |
| MySQLPooled_FAILURES          | None          |         |
| MySQLPooled_PING              | 1             |         |

## Usage

Initialize the extension:
```python
from flask_mysqlpooled import MySQLPooled
    
mysql_pool = MySQLPooled()
mysql_pool.init_app(app)
# or
mysql_pool = MySQLPooled(app)
```

Obtain a cursor:
```python
cursor = mysql_pool.get_db().cursor()
```


Multiple connection example:
```python
import pymysql
from flask import Flask
from flask_mysqlpooled import MySQLPooled

app = Flask(__name__)

mysql_pool_1 = MySQLPooled(app,
                        prefix='MySQLPooled1',
                        host='host1',
                        user='user1',
                        password='password1',
                        db='db1',
                        autocommit=True,
                        cursorclass=pymysql.cursors.DictCursor,
                        mincached=0,
                        maxcached=3)
mysql_pool_2 = MySQLPooled(app,
                        prefix='MySQLPooled2',
                        host='host2',
                        user='user2',
                        password='password2',
                        db='db2',
                        autocommit=True,
                        cursorclass=pymysql.cursors.DictCursor,
                        mincached=0,
                        maxcached=3)

@app.route('/')
def index():
    cursor1 = mysql_pool_1.get_db().cursor()
    cursor2 = mysql_pool_2.get_db().cursor()
    # ...
```
