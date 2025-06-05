# UV基础详解


[toc]



## 一、介绍

python环境管理工具

官网：https://docs.astral.sh/uv/

**官方文档，值得一读。**

【翻译版】：https://hellowac.github.io/uv-zh-cn/



## 二、安装

参考：https://docs.astral.sh/uv/getting-started/installation/



### 2.1、shell方式安装（推荐）

Linux or Mac：

```shell
# curl方式安装
curl -LsSf https://astral.sh/uv/install.sh | sh
# wget方式安装
wget -qO- https://astral.sh/uv/install.sh | sh
# 指定版本安装
curl -LsSf https://astral.sh/uv/0.7.10/install.sh | sh
```



Windows：

```shell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
# 指定版本安装
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/0.7.10/install.ps1 | iex"
```



### 2.2、pip方式安装

```shell
pip install uv
pipx install uv
```



### 2.3、brew方式安装

```shell
brew install uv
```



### 2.4、离线安装

https://github.com/astral-sh/uv/tags

github上下载对应的包，放入path中即可。



### 2.5、版本验证

```
uv --version
```



### 2.6、uv升级

```
uv self update
```





## 三、常用命令

### 3.1、python版本管理

```shell
# 安装具体版本（推荐）
uv python install 3.11.12

# 指定小版本，则会安装最新的版本，如下：则会安装3.11的最新版本
uv python install 3.11

# Pin the current project to use a specific Python version.
uv python pin 3.11.12
```

> [!NOTE]
>
> 如果出现安装超时，参考4.1小节 网络加速方法。



### 3.2、虚拟(venv)环境管理

**创建虚拟环境：**

```shell
# 在当前目录下创建一个名为 .venv 的虚拟环境，使用系统默认的 Python 版本
uv venv

# 在当前目录下创建一个名为 my-project-env 的虚拟环境，使用指定的 Python 3.11 版本
uv venv -p python3.11 my-project-env
uv venv -p 3.11 my-project-env
uv venv --python 3.11 my-project-env

# 在指定路径创建一个名为 custom-env 的虚拟环境，使用 Python 3.10
uv venv -p python3.10 /path/to/my/project/custom-env
```



**删除虚拟环境：**

```shell
# 删除当前目录下的名为 .venv 的虚拟环境
uv venv --remove .venv

# 删除指定路径的虚拟环境
uv venv --remove /path/to/my/project/my-project-env
```



### 3.3、依赖包管理

确保你已经激活了虚拟环境 

```
Windows (CMD): .\venv\Scripts\activate
macOS/Linux (Bash/Zsh): source venv/bin/activate
```



> [!IMPORTANT]
>
> **推荐使用`uv add` 方式安装，这样会自动更新 `pyproject.toml` `uv.lock`文件。**



#### 3.3.1、uv pip方式管理依赖

**安装依赖包：**

```shell
# 确保你已经激活了虚拟环境

# 安装 requests 包
uv pip install requests

# 安装指定版本的 Flask 包
uv pip install Flask==2.2.2
uv pip install "django>=4.0,<5.0"

# 从 requirements.txt 文件安装所有依赖
uv pip install -r requirements.txt

# 安装开发依赖 (通常在 requirements-dev.txt 中)
uv pip install -r requirements-dev.txt -d dev
```

```shell
# uv pip安装依赖包
# 确保你已经激活了虚拟环境
# 安装项目依赖（从 pyproject.toml）
uv pip install -e .

# 安装开发依赖
uv pip install -e ".[dev]"

# 安装所有可选依赖
uv pip install -e ".[dev,docs,web]"

# 生成锁定文件
uv pip freeze > requirements.lock

# 同步依赖（确保环境与 requirements.txt 完全一致）
uv pip sync requirements.txt
```



**卸载依赖包：**

```shell
# 确保你已经激活了虚拟环境

# 卸载 requests 包
uv pip uninstall requests

# 卸载多个包
uv pip uninstall Flask Werkzeug
```



**列出已安装的包：**

```shell
# 确保你已经激活了虚拟环境
uv pip list
```



**导出requirements文件：**

```shell
# 确保你已经激活了虚拟环境

# 导出到默认的 requirements.txt 文件
uv pip freeze > requirements.txt

# 导出到指定的文件
uv pip freeze > my_dependencies.txt
```



**同步依赖：**

**功能:** 根据锁定的 requirements.txt 文件同步当前虚拟环境中的依赖。如果虚拟环境中的包与锁定文件不一致，uv 会安装、升级或卸载包以使其与锁定文件匹配。

```shell
# 确保你已经激活了虚拟环境，并且有一个 requirements.txt 文件

# 根据 requirements.txt 同步虚拟环境
uv pip sync requirements.txt

# 根据 pylock.toml 同步虚拟环境
uv pip sync pylock.toml

# 同步指定的锁定文件
uv pip sync locked_dependencies.txt
```



#### 3.3.2、uv add方式安装依赖（推荐）

**添加依赖包：**

```shell
# 确保你已经激活了虚拟环境

# 安装 requests 包，默认生产依赖
uv add requests

# 安装指定版本的 Flask 包
uv add Flask==2.2.2
uv add "django>=4.0,<5.0"

# 添加开发依赖
uv add --dev pytest black flake8
uv add --dev "pytest>=7.0"

# 添加可选依赖组
uv add --group docs sphinx sphinx-rtd-theme
uv add --group web fastapi uvicorn
```



**添加依赖包（依赖包分组）：**

在` uv` 包管理工具中，`--dev` 和 `--group` 是用于将依赖项分类到不同组别的参数，类似于其他包管理器（如 pip 或 poetry）中的开发依赖和可选依赖。

##### --dev参数

- `--dev` 表示将包标记为**开发依赖**（Development Dependency），这类依赖通常仅在开发环境中需要（如测试框架、代码格式化工具等），而不会出现在生产环境中。
- 这些依赖会被记录在 `pyproject.toml` 或类似配置文件的 `dev-dependencies` 部分（取决于工具的具体实现）。

**使用示例：**

```
# 安装 pytest、black 和 mypy，并将它们标记为开发依赖
uv add --dev pytest black mypy
```



##### --group参数

- `--group` 允许将依赖项划分到**自定义的依赖组**中，用于更灵活地管理不同环境的依赖（如测试、文档、可选功能等）。
- 类似于 `poetry` 的 `extras` 或 `pip` 的可选依赖组。

**使用示例：**

```
# 安装 mkdocs 和 mkdocstrings 并将其分配到 docs 组，安装 pytest 和 coverage 并分配到 tests 组
uv add --group docs mkdocs mkdocstrings
uv add --group tests pytest coverage
```

> 在 `pyproject.toml` 中，依赖组可能会显示为：
>
> ```
> [tool.uv.dependencies]
> # 主依赖
> requests = "^2.31.0"
> 
> [tool.uv.group.docs.dependencies]
> mkdocs = "^1.5.0"
> mkdocstrings = "^0.24.0"
> 
> [tool.uv.group.tests.dependencies]
> pytest = "^8.0.0"
> coverage = "^7.0.0"
> ```

安装特定组（group）的依赖

```
uv install --group docs  # 仅安装 docs 组的依赖
uv install --all-groups  # 安装所有组的依赖

适用场景：
- 文档生成（如 sphinx、mkdocs）。
- 测试环境（如 pytest、coverage）。
- 可选功能（如 GPU 加速包 torch 的特定版本）。
```



##### --dev --group关联关系和区别

在 `uv` 包管理工具中，依赖分组（dependency groups）的默认配置目前主要围绕 **开发依赖（dev）** 和 **主依赖（默认分组）**，但具体支持的默认分组可能因工具版本或设计有所不同。以下是已知的常见默认分组及其用途：

- 默认分组（Main/Default）

```
uv add requests  # 添加到主依赖
```

> 配置文件 (`pyproject.toml`)：
>
> ```
> [tool.uv.dependencies]
> requests = "^2.31.0"
> ```



- 开发分组（Development）

```
uv add --dev pytest
# 或
uv add --group dev pytest
```

> 配置文件：
>
> ```
> [tool.uv.dev-dependencies]
> pytest = "^7.0.0"
> 
> # 或（取决于实现）
> [tool.uv.group.dev.dependencies]
> pytest = "^7.0.0"
> ```



- 其他分组（group）

```
uv add --group tests pytest-coverage
uv add --group docs mkdocs
uv add --group optional redis
```



```
# 自定义分组
uv add --group benchmark pandas
```

> 配置文件会生成：
>
> ```
> [tool.uv.group.benchmark.dependencies]
> pandas = "^2.0.0"
> ```



##### 分组总结

| 分组类型       | 参数/名称                | 用途                       |
| :------------- | :----------------------- | :------------------------- |
| **主依赖**     | 无（默认）               | 生产环境必需依赖           |
| **开发依赖**   | `--dev` 或 `--group dev` | 开发工具（测试、格式化等） |
| **自定义分组** | `--group <name>`         | 按需划分（如 docs、tests） |



#### 3.3.3、卸载依赖包

```shell
# 确保你已经激活了虚拟环境

# 卸载 requests 包
uv remove requests

# 卸载多个包
uv remove Flask Werkzeug

# 从 docs 组移除 mkdocs
uv remove --group docs mkdocs
```



### 3.4、创建项目

```
uv init my_project
uv init my_project -p 3.11.12
```



### 3.5、uv cache

```shell
# 查看缓存路径
uv cache dir
# 从缓存目录中删除所有缓存条目，将其彻底清除。
uv cache clean
# 删除所有未使用的缓存条目
uv cache purge
```



### 3.6、uv lock

`uv lock` 用于 **锁定当前项目的依赖版本**，生成或更新 `uv.lock` 文件（类似于 `pip` 的 `requirements.txt` 或 `poetry` 的 `poetry.lock`）。

```shell
# 根据 pyproject.toml 生成 uv.lock 文件（如果不存在）
# 如果已有 uv.lock，会更新它以匹配当前依赖声明
uv lock
```



```shell
# 如果项目使用 requirements.txt 而非 pyproject.toml
uv lock -r requirements.txt
```



```shell
# 忽略现有 uv.lock，重新计算依赖关系（适用于手动修改 pyproject.toml 后）
uv lock --update
```



### 3.7、uv export

`uv export` 用于 **将当前项目的依赖导出为其他格式**（如 `requirements.txt`），通常用于兼容传统工具或部署场景。

```
uv export > requirements.txt

# 忽略哈希校验（某些旧版 pip 需要）
uv export --without-hashes > requirements.txt

# 导出指定分组的依赖
# 导出 pyproject.toml 中 [tool.uv.group.dev] 下的依赖。
uv export --group dev > requirements-dev.txt
uv export --group prod > requirements-prod.txt

# 排除开发依赖
uv export --without dev > requirements-prod.txt

# 导出json格式
uv export --format json > requirements.json

# 结合 uv lock 确保一致性
uv lock              # 先锁定依赖版本
uv export --locked   # 从锁文件导出
```





## 四、使用优化

### 4.1、uv安装python，网络加速

参考：https://developer.aliyun.com/article/1660997

描述：

```
uv安装python版本，因为网络隔离原因，不能直接下载包。需要离线安装方式。
```



**1、下载离线安装包**

在执行`uv python install 3.11.12`时，uv 会去`https://github.com/astral-sh/python-build-standalone/releases`上拉取对应版本（最新日期）。

如果知道你需要的版本包，可以直接从`github`地址上下载。如果不知道，先执行`uv python install 3.11.12`命令，在超时时，会显示包名，按照包名去找到对应的包，然后下载包。

示例：

如：

包名为：

**2、设置环境变量**

本地建一个目录，比如`/mnt/workspace/uv_python_install_mirror/20250409`，然后将上述压缩包放入这个目录，然后将环境变量`UV_PYTHON_INSTALL_MIRROR`设置成这个目录，这样就uv就会去这个目录里面找压缩包，然后快速安装python了。比如`export UV_PYTHON_INSTALL_MIRROR=file:///mnt/workspace/uv_python_install_mirror/20250409`

> [!IMPORTANT]
>
> 目录的日期是一定要创建的，否则会找不到文件。

**3、安装python**

uv python install 3.11.12



### 4.2、docker中使用uv

参考：https://docs.astral.sh/uv/guides/integration/docker/



### 4.3、迁移现有requirements.txt

运行 `uv pip compile requirements.txt -o pyproject.toml --format pyproject` 即可转换为 pyproject.toml 格式。





## 五、pip依赖包源

```
阿里云： http://mirrors.aliyun.com/pypi/simple/
清华大学： https://pypi.tuna.tsinghua.edu.cn/simple/
中国科技大学： https://pypi.mirrors.ustc.edu.cn/simple/
中国科学技术大学： http://pypi.mirrors.ustc.edu.cn/simple/
```



**全局环境变量设置包源地址：**

```
export UV_INDEX_URL=https://mirrors.aliyun.com/pypi/simple/
```



**项目设置包源地址：**

在`pyproject.toml`中，最后添加一下内容：

```
[[tool.uv.index]]
url = "https://mirrors.aliyun.com/pypi/simple/"
default = true
```



**安装包时设置源：**

```
uv pip install <package_name> --index-url https://mirrors.aliyun.com/pypi/simple/
uv pip install <package_name> -i https://mirrors.aliyun.com/pypi/simple/
# ssl异常时
uv pip install <package_name> -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com
```





## 参考

https://www.cnblogs.com/java-six/p/18872752

uv add和uv pip区别：https://mp.weixin.qq.com/s/8tyD87RMARPRqiV8i-saOg

