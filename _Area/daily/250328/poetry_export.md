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

## Check Platforms

### 📦 Installing Python Dependencies for a Target Platform (e.g., AWS Lambda)

To install Python packages for a specific platform and Python version (e.g., `manylinux2014_x86_64` for AWS Lambda with Python 3.11), use the following command:

```bash
pip install \
  --no-cache-dir \
  --platform manylinux2014_x86_64 \
  --target ./python/ \
  --python-version 3.11 \
  --only-binary=:all: \
  -r requirements.txt
```

### 🔍 Option Breakdown

| Option                       | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| `--no-cache-dir`            | Disables pip cache to save space.                                           |
| `--platform manylinux2014_x86_64` | Specifies the target platform (e.g., for Lambda Linux compatibility).     |
| `--target ./python/`        | Installs all packages into the `./python/` directory for bundling.          |
| `--python-version 3.11`     | Downloads packages compatible with Python 3.11.                             |
| `--only-binary=:all:`       | Forces pip to use only pre-built binary wheels (`.whl`), skipping source builds. |
| `-r requirements.txt`       | Installs dependencies listed in your `requirements.txt` file.              |

### 📁 Example: Create a Zip Package for Lambda Deployment

```bash
# Install dependencies into ./python/
pip install \
  --no-cache-dir \
  --platform manylinux2014_x86_64 \
  --target ./python/ \
  --python-version 3.11 \
  --only-binary=:all: \
  -r requirements.txt

# Zip the contents for AWS Lambda deployment
zip -r lambda-package.zip python/
```

> ⚠️ **Note**: Requires `pip >= 21.0`. Ensure your pip version supports `--platform` and `--python-version` flags.
