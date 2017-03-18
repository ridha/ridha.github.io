---
layout: single
title:  "Accessing PostgreSQL with Python and Psycopg2"
date:   2008-08-09 22:28:00
categories: python
---
Psycopg2 is a DB API 2.0-compliant PostgreSQL driver.
You can download psycopg from here http://pypi.python.org/pypi/psycopg2/2.0.4.

First verify that the psycopg2 module is installed.
You can check this by running Python in interactive mode from the command line prompt.

{% highlight bash %}
$ python
Python 2.5.1 (r251:54863, Mar  7 2008, 04:10:12)
[GCC 4.1.3 20070929 (prerelease) (Ubuntu 4.1.2-16ubuntu2)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import psycopg2
Traceback (most recent call last):
File , line 1, in ?
ImportError: No module named psycopg2
{% endhighlight %}

Scripts that access PostgreSQL through DB-API using psycopg2 generally perform the following steps:
* Import the psycopg2 module
* Open a connection to the PostgreSQL server
* Issue statements and retrieve their results
* Close the server connection

### Writing the scripts

For the script examples I created a database `test` and a table `fruits`;

{% highlight bash %}
postgres=# select * from fruits;
 id |  name
----+--------
  1 | apple
  2 | orange
  3 | grape
(3 rows)
{% endhighlight %}

Use a text editor to create a file named fruits.py that contains the following script.
This script uses psycopg2 to interact with the PostgreSQL server, selects all the records in the table and prints them.

{% highlight python %}
import psycopg2
conn = psycopg2.connect ("host='localhost' user='user_name' password='password' dbname='postgres'")
cursor = conn.cursor()
cursor.execute("SELECT id,name from fruits")
while 1:
    row = cursor.fetchone()
    if not row:
        break
    print "%s, %s" % (row[0], row[1])
cursor.close()
conn.close()
{% endhighlight %}

The result is:

{% highlight python %}
akm@tmp$ python2.5 fruits.py
1, apple
2, orange
3, grape
{% endhighlight %}


fetchone() is used for retrieving one row at a time. fetchone() returns the next row of the result set as a tuple, or the value None if no more rows are available.
Another way to retrieve the result is to use fetchall().
It returns the entire result set all at once as a tuple of tuples, or as an empty tuple if the result set is empty.

{% highlight python %}
cursor.execute("SELECT id, name FROM fruits")
rows = cursor.fetchall()
for row in rows:
    print "%s, %s" % (row[0], row[1])
{% endhighlight %}

If you want to insert data into the table use:
{% highlight python %}
cursor.execute("""INSERT INTO fruits (id, name)
                       VALUES
                       ('4', 'banana'),
                       ('5', 'cherry'),
                       """)
{% endhighlight %}

After issuing the insert statement, commits the changes, and disconnects from the server:

{% highlight python %}
cursor.close()
conn.commit()
conn.close()
{% endhighlight %}

The commit() method commits any outstanding changes in the current transaction to make them permanent in the database.
In DB-API, connections begin with autocommit mode disabled, so you must call commit() before disconnecting or
changes may be lost.
