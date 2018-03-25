## 使用 Python 从数据库获取图像数据

> 需安装 MySQLdb 第三方Python库

``` python
#!/usr/bin/python

# -*- coding: utf-8 -*-

import MySQLdb as mdb 
import sys 
import os

try:
    conn = mdb.connect(host='127.0.0.1', user='root',
                       passwd='123', db='database_A')
    cursor = conn.cursor()
    cursor.execute("SELECT facePic FROM list_A")
    result = cursor.fetchall()
    n = len(result)
    print(n)
    for i in range(n):
        filename = '/home/zhi/images/image_%08d.jpg' % (i+1)
        print(filename)
        fout = open(filename, 'wb')
        fout.write("".join(result[i]))
        fout.close()
    cursor.close()
    conn.close()


except IOError, e:
    print("Errpr %d: %s" % (e.args[0], e.args[1]))
    sys.exit(1)
```