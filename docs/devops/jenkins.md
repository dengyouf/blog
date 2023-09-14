#  Jenkins安装

## 添加apt源

```shell
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
```
```shell
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
```

## 安装 Jenkins

```shell
sudo apt-get update
sudo apt-get install fontconfig openjdk-11-jre
sudo apt-get install jenkins
```

## 访问 Jenkins UI

访问地址：`http://192.168.33.10:8080`

- 解锁安装
![](./imgs/01-jenkins.png)

- 安装推荐插件
![](./imgs/01-jenkins2.png)

- 创建管理员账号
![](./imgs/01-jenkins3.png)

