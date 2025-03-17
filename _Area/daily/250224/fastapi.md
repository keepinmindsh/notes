# Alembic 

- [Link](https://pypi.org/project/alembic/)

```shell 
pip install alembic
```

- 기본 init migrations 세팅 추가 

```shell 
albemic init migrations 
```

- alembic revision 설정 

```shell 
alembic revision --autogenerate -m "add User Table" 
```

- metadata 연결 

```python 
# migraitions/env.py

import database 

target_metadata = database.Base.metadata

```

```python
# databse_models.py 

import user.infra.db_models.user 
```

```python 
# migrations/env.py 

import database 
import database_models 
```

- 최종 Revision 적용하기 

```python
alembic upgrade head
```

- Revision 에 대해서 Downgrade 하기 

```shell 
alembic downgrade -1
```