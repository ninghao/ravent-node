# Ravent 应用实战项目

这是宁皓网的一个应用开发实战课程的配套项目，项目基于 [xb2-node](https://github.com/ninghao/xb2-node) 这个项目创建。这个 xb2-node 是我们在一个[应用开发课程](https://ninghao.net/package/xb2-node)里一起开发的项目，在这个项目的基础上我们会根据实际需求再去做一些改进。

## 本地开发

### **1：把项目克隆到本地**

```
git clone https://github.com/ninghao/ravent-node
```

### **2：创建开发分支**

你克隆了这个项目之后，会在本地创建一个 master 分支，在这个 master 分支上的项目应该是开发好的项目。如果你打算跟着课程一步一步改进这个项目，你可以先在本地创建一个开发分支。

```
git checkout -b develop remotes/origin/starting-point
```

### **2：安装依赖**

```
cd ravent-node
npm install
```

### **3：环境变量**

项目要运行起来需要用到一个环境变量文件，复制一份 _assets/example.env_ 这个文件，把复制的文件放在项目的根目录的下面，并且把文件名改成 _.env_。然后根据自己的需要修改在 _.env_ 这个环境变量文件里的内容。

```
# 应用配置
APP_PORT=3000

# 内容配置
POSTS_LIMIT=30

# 数据仓库配置
MYSQL_HOST=localhost
MYSQL_PORT=3306
MYSQL_USER=root
MYSQL_PASSWORD=PASSWORD
MYSQL_DATABASE=ravent

# 密钥
PRIVATE_KEY=LS0tLS1CR
PUBLIC_KEY=LS0tLS1CRUd
```

在这个环境变量里的 PRIVATE_KEY 与 PUBLIC_KEY 是签发验证用户令牌的时候需要用的密钥与公钥。这里需要的是经过 Base64 编码之后的密钥与公钥，这样做主要是为了可以把密钥与公钥内容转换成一行，这样我们才能把它放在这个环境变量文件里使用。

在 _assets/example.env_ 文件里已经包含了 PRIVATE_KEY 与 PUBLIC_KEY 的值，这对密钥是我自己签发的，你在本地开发的时候可以直接使用，但是你的应用要在生产环境上运行的时候，你要准备自己的密钥与公钥，然后替换掉在环境变量里的 PRIVATE_KEY 与 PUBLIC_KEY 的值。

## 准备数据

在 [ravent-node-demo](https://github.com/ninghao/ravent-node-demo) 这个项目里包含了这个项目的演示数据，你可以把这个项目下载到本地，如果你觉得慢，也可以申请加入宁皓网的 QQ 群：240746680，然后在群文件里下载。

![数据仓库](https://raw.githubusercontent.com/ninghao/ravent-node/master/assets/images/Snip20200714_3.png)

### 1：新建一个数据仓库

在本地你需要安装 MySQL，然后给应用创建一个数据仓库，名字可以叫 _ravent_。

### 2：导入演示数据

在之前创建的 ravent 数据仓库里，导入在 _ravent-node-demo_ 这个项目里 _database/ravent.sql_ 这个文件。这样这个 ravent 数据仓库里会出现应用需要的数据表，数据表里面会有一些演示数据。

### 3：复制上传文件

在 _ravent-node-demo_ 项目里，找到 _uploads_ 目录，把这个目录复制一份放到我们的 _ravent-node_ 这个项目的根目录下面。这个 _uploads_ 里面的东西就是上传的图片与用户头像的原始文件，以及它们的调整尺寸之后的文件。

## 接口测试

测试应用接口的时候，可以使用 Insomnia 这个客户端软件。在测试的时候你需要创建一些对接口的请求，然后要做一些配置，比如请求的地址，请求里带的数据等等。你可以在 Insomnia 里面导入在 _ravent-node-demo_ 这个项目里的 _insomnia-workspace/ravent.yaml_ 这个文件，这样会为你创建好测试应用接口的时候需要的请求。

![Insomnia](https://raw.githubusercontent.com/ninghao/ravent-node/master/assets/images/Snip20200714_5.png)

## 生成密钥

在生产环境上运行这个应用的时候，你需要准备自己的密钥与公钥。

### 1：生成密钥与公钥文件

```

cd config
openssl
genrsa -out private.key 4096
rsa -in private.key -pubout -out public.key
exit

```

### 2：转换密钥与公钥格式

在项目下面，执行：

```
node config/convert.key.js
```

你会看到转换成 Base64 格式的 Private Key 与 Public Key，把得到的内容交给 .env 文件里的 PRIVATE_KEY 与 PUBLIC_KEY。
