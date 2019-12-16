---
title: 디자인 패턴-팩토리 메소드(Factory Method) 파이썬(python)
date: 2019-11-22
tags: 
    - 파이썬
    - 디자인패턴
categories: 
    - 파이썬 
    - 디자인패턴
---

```
from abc import ABC, abstractmethod


class Factory(ABC):
    @abstractmethod
    def connection(self):
        pass


class Oracle(Factory):
    def connection(self):
        return 'Oracle Database connectioned'


class MariaDB(Factory):
    def connection(self):
        return 'Maria Database connectioned'


class Sqlite3(Factory):
    def connection(self):
        return 'Sql Database connectioned'


class Connection_Factory:
    def get_db_connection(self, db):
        return db.connection()


db_fact = Connection_Factory()

print(db_fact.get_db_connection(Oracle()))
print(db_fact.get_db_connection(MariaDB()))
print(db_fact.get_db_connection(Sqlite3()))
```
