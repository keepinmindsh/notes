# Poetry Export 

If you want export dependany files from your poetry, you can use a follow way of many ways. 

```
#  Makefile  
poetry_export:
	poetry export -f requirements.txt --without-hashes > requirements.txt
```

- Use a Virtual Environment (Recommended)
To avoid dependency conflicts, it's best to use a virtual environment:

```shell 
python -m venv venv
source venv/bin/activate   # On Windows: venv\Scripts\activate
pip install numpy==1.24.4 gensim numba contourpy gradio
```

So, It can be downloaded on your virtual environment. 

```shell 
(lines-auth-human-resources-py3.11) (base)  lines@SEUNGHWAui-MacBookPro  ~/sources/00_company/lines_auth_human_resources   master ±✚
 pip install --no-cache-dir --target  ./python/ --python-version 3.11 --only-binary=:all: -r requirements.txt
```
