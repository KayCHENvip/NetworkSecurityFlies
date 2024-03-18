# GitLab 配置 SSH 密钥



### 一 下载并安装Git

 

### 二 在文件夹空白处右击，打开Git Bash窗口

 

### 三 生成密钥

 

输入命令，并一路回车。

```bash
# -C是用来识别这个密钥的注释



ssh-keygen -t rsa -C "bread@food.com"
```

 

### 四 用记事本打开公钥，并复制

 

默认路径为C:\Users\用户名\.ssh\id_rsa.pub

 

### 五 添加公钥

 

登录[GitLab](https://so.csdn.net/so/search?q=GitLab&spm=1001.2101.3001.7020)网站，右上角头像 -> Settings -> SSH Keys中，添加复制的文本。

 

### 六 下载项目

 

使用SSH方式下载

```bash
git clone ssh://git@192.168.111.222:5678/tom/demo.git
```

下载过程中，出现如下提示时，需要输入yes。

```bash
ECDSA key fingerprint is SHA256:XXXXXX.



Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
```

###  

### 七 配置名称和邮箱

 

若不配置，在VSCode中提交代码会报错。

```bash
git config --global user.name "Your Name"



git config --global user.email "you@example.com"
```

 

## Linux系统

- 检查本机是否存在 `密钥`，如果存在 `id_rsa（私钥）`、`id_rsa.pub（公钥）` 文件则说明已经创建过了，直接拷贝即可。

  ```shell
  shell
  复制代码$ ls ~/.ssh
  ```

  如果不需要这份可以删除，注意这份密钥没有在使用，移除之后就无法恢复了，之前所使用的地方也需要使用新的：

  ```shell
  shell复制代码$ rm -rf ~/.ssh/id_rsa
  $ rm -rf ~/.ssh/id_rsa.pub
  ```

- 创建密钥（存在密钥的可以跳过）

  ```shell
  shell
  复制代码$ ssh-keygen -t rsa -C "youremail@example.com"
  ```

  ```shell
  shell复制代码# 执行命令，将邮箱换成自己的
  $ ssh-keygen -t rsa -C "youremail@example.com"
  
  # 指定保存文件夹，默认是这个 /Users/dengzemiao/.ssh/id_rsa
  Enter file in which to save the key (/Users/dengzemiao/.ssh/id_rsa): 
  
  # 输入验证密码，如果不想每次都输入验证密码，则直接回车，不进行输入
  Enter passphrase (empty for no passphrase):
  Enter same passphrase again:
  ```

- 拷贝 `公钥`

  ```shell
  shell
  复制代码$ cat ~/.ssh/id_rsa.pub
  ```

  ![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e7f29f0cded24225b48998876155dae5~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

- 打开 `GitLab` 进行配置 `SSH`，配置好之后，就可以通过 `SSH` 进行拉取更新代码了！

  ![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1bfca1eb9ac34469ae8b9e9b2d328d07~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

  ![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/03e34484a3b1473e8447c6a72fc95260~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)