# sqlite
import sqlite3
import os
# انشاء مجلد لحفظ الداتابيز داخله
newpath = 'C:\ProgramData\MelkiData2'
if not os.path.exists(newpath):
    os.makedirs(newpath)
# انشاء الداتابيز
conn = sqlite3.connect(newpath+'\example.db')
c = conn.cursor()

# إنشاء جدول
c.execute('CREATE TABLE if not exists stocks(date text, trans text, symbol text, qty real, price real)')

# إدراج صف من البيانات

#c.execute("INSERT INTO stocks VALUES ('2006-01-05','BUY','RHAT',100,35.6)")

for i in range(3):
    a=i
    c.executemany('INSERT INTO stocks VALUES (?,?,?,?,?)', [('2006-03-'+str(i), 'BUY', 'IBM'+str(i), 1000, a)])
# حذف بعض البيانات
c.execute('DELETE FROM stocks WHERE price >=5')

# حفظ التعديلات
conn.commit()
#طباعة بيانات من الجدول

t = ('IBM1',)
c.execute('SELECT date, symbol FROM stocks WHERE symbol=?', t )
print(c.fetchone())

for row in c.execute('SELECT * FROM stocks ORDER BY price'):
        print(row)
# يمكن كذلك إغلاق الاتصال بعد الانتهاء
# ولكن يجب التأكد من حفظ جميع التعديلات تجنبًا لفقدانها كلّها
conn.close()
