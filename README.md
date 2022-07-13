# hyperf-dockerfiles
使用docker-compose 编排hyperf运行环境支持一键启动hyperf的项目环境


## 必须操作
* 文件需放在项目根目录下
## 使用方式
~~~
cd dockerfiles && docker-compose up -d
~~~

## ssh
~~~
#docker-compose.yml

environment:
      SSHPASSWD: "test" #默认密码为13456你可以通过environment自定义密码
~~~