# Содержание
- [ЛР № 5–6](#лабораторная-работа--56)
- [ИСР № 2](#инвариативная-самостоятельная-работа--2)
    - [1 задание]
    - [2 задание]
    - [3 задание]
- [ВСР № 2](#вариативная-самостоятельная-работа--2)

# [Лабораторная работа № 5–6](https://repl.it/@Rakleed/programming4-lab5-6)
```python
import sqlite3
import functools


def once(func):
    @functools.wraps(func)
    def inner(*args, **kwargs):
        print(func.__name__, func.__doc__, args, kwargs)
        if not inner.called:
            conn = func(*args, **kwargs)
            inner.called = True
            print("Called is changed.")
            return conn
    print("Init")
    inner.called = False
    return inner


@once
def connect_to_db(path_to_db):
    connection = None
    if path_to_db:
        try:
            connection = sqlite3.connect(":memory:")
        except:
            return None
        else:
            c = connection.cursor()
            return {"conn": connection, "cursor": c}
    return connection


def wrapper(conn, table):
    auth_stat = False
    login = str(input("Введите логин: "))
    password = str(input("Введите пароль: "))
    user_query = "SELECT * FROM " + str(table) + " WHERE login=" + "'" + login + "'"
    cursor = conn.cursor()
    res = cursor.execute(user_query)
    user_info = res.fetchall()
    if not user_info:
        print("Нет такого пользователя.")
        return auth_stat
    print(user_info)
    role = ''
    if user_info[0][1] == password:
        print(private_zone_area())
    else:
        print("Пользователя с таким паролем не существует.")
        return auth_stat
    if (user_info[0][0]) == "root":
        print("Вы зашли как root.")
        role = "0"
    elif (user_info[0][0]) == "admin":
        print("Вы зашли как admin.")
        role = "1"
    else:
        print("Вы зашли как обычный пользователь.")
        role = "2"
    return role


def private_zone_area():
    return "private_zone_area"


def get_users_from_table(conn, table):
    sql_query = "SELECT * FROM " + str(table)
    cursor = conn.cursor()
    res = cursor.execute(sql_query)
    users_list = res.fetchall()
    return users_list


def main():
    conn_dict = connect_to_db("file.db")
    conn, cur = conn_dict["conn"], conn_dict["cursor"]
    try:
        sql_query = '''CREATE TABLE users
            (login text, pass text, role int)'''
        cur.execute(sql_query)
    except sqlite3.OperationalError as e:
        e_string = str(e)
        if "already exists" in e_string:
            print(f' NOTICE: {e}. CONTINUE ')
    else:
        sql_query = '''INSERT INTO users VALUES (?, ?, ?)'''
        users_list = [("root", "123", 0), ("admin", "789", 1),
                      ("user", "qwe", 2)]
    try:
        cur.executemany(sql_query, users_list)
        conn.commit()
    except sqlite3.Error as e:
        print(f'Error with adding users to db. {e}')
    users = get_users_from_table(conn, "users")
    print(users)
    user_role = False
    while user_role == False:
        user_role = wrapper(conn, "users")
        if user_role == False:
            print("Выйти?")
            if str(input("Да/Нет: ")) == "Да":
                conn.close()
                cur, conn = None, None
                return 0
    conn.close()
    cur, conn = None, None


main()
```
[file.db](src/file.db)

![Result of lab5-6](src/programming4-lab5-6-result.png)

# Инвариативная самостоятельная работа № 2
### [2.1. ](https://repl.it/@Rakleed/programming4-indepworkinvar2-1)
```python

```
![Result of indepworkinvar2-1](src/programming4-indepworkinvar2-1-result.png)

### [2.2. ](https://repl.it/@Rakleed/programming4-indepworkinvar2-2)
```python

```
![Result of indepworkinvar2-2](src/programming4-indepworkinvar2-2-result.png)

### [2.3. ](https://repl.it/@Rakleed/programming4-indepworkinvar2-3)
```python

```
![Result of indepworkinvar2-3](src/programming4-indepworkinvar2-3-result.png)

# [Вариативная самостоятельная работа № 2](https://repl.it/@Rakleed/programming4-indepworkvar2)
```python

```
![Result of indepworkvar2](src/programming4-indepworkvar2-result.png)
