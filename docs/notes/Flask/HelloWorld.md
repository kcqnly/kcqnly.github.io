# Flask笔记

## 环境搭建

### 1. 安装pipenv

```shell
sudo -H pip install pipenv
```

### 2. 创建虚拟环境

新建一个文件，在文件夹里打开终端

```shell
pipenv install
```

激活虚拟环境

```shell
pipenv shell
```

vscode设置
在项目中的`.vscode`文件夹中创建`settings.json`文件。

```shell
# 获取当前虚拟环境的地址
pipenv --venv
```

在`settings.json`中加入下面一行设置，里面填上你自己的虚拟环境的地址。

```json
"python.venvPath": "..."，
"python.linting.pylintArgs": [
    "--load-plugins",
    "pylint-flask"
],
"python.formatting.autopep8Path": "autopep8",
"python.linting.pylintPath": "pylint"
```

还有一项`python.path`可以通过左下角选择。

## 3. 安装flask

设置镜像(修改Pipfile)

```text
url = "https://pypi.tuna.tsinghua.edu.cn/simple/"
```

```shell
pipenv install flask
```

## 4. 启动

创建`app.py`

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return '<h1>Hello Flask!</h1>'

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=4555, debug=True)

```

项目根目录创建`.flaskenv`文件

```txt
FLASK_ENV=development
FALSK_APP=app.py
```

使用命令运行即可

```shell
flask run
```
