# cicd-demo
这是在公司的开发的CI/CD项目流程，但因为保密协议，因此我自己在重新制作一个，并且用简单的springboot项目，用于测试java项目使用jenkins、gitlab ci打包部署功能，也可以用在argo cd

# 版本依赖
Openjdk:17.0.7

Maven:3.9.3

Spring Boot:3.1.1


# 打包项目：
# 在项目根目录执行命令
mvn clean package

# 运行项目：
java -jar target/SpringBootDemo-0.0.1-SNAPSHOT.jar

# 访问测试：
➜  SpringBootDemo git:(main) ✗ curl 127.0.0.1:8888/       
Hello SpringBoot Version:v1   
➜  SpringBootDemo git:(main) ✗ curl 127.0.0.1:8888/health
ok

# 构建docker镜像并部署
➜  SpringBootDemo git:(main) ✗ docker build -t springboot_demo:v1 .   
➜  SpringBootDemo git:(main) ✗ docker run -d -p 8888:8888 --name springboot_demo springboot_demo#:v1

# CI/CD 流程：
cicd
├── Dockerfile  # 项目打包成docker镜像的文本文件
├── Dockerfile-maven # 自定义maven镜像，替换国内源
├── deployment-docker.sh # 部署到docker环境脚本
├── deployment-linux.sh # 部署到linux系统环境脚本
├── gitlab-ci # gitlab ci/cd流水线
│   ├── docker.yml # 部署到docker环境完整流水线
│   ├── k8s.yml # 部署到k8s环境完整流水线
│   └── linux.yml # 部署到linux系统环境完整流水线
├── jenkins # jenkins ci/cd流水线
│   ├── Dockerfile-jenkins-slave # 自定义jenkins slave镜像的文本文件
│   ├── Jenkinsfile-docker.groovy # jenkins发布到docker环境的完整流水线
│   ├── Jenkinsfile-k8s.groovy # jenkins发布到k8s环境的完整流水线
│   └── email.html # jenkins发送自定义邮件格式代码
├── jmeter # jmeter接口自动化测试
│   └── demo.jmx # 接口自动化测试配置文件
├── k8s.yaml # 部署到k8s的yaml文件
├── kustomize # k8s多环境部署文件
└── sonar-project.properties # SonarQube代码扫描配置文件

# 自动化测试
# jmeter目录下存放示例接口自动化测试脚本，主要测试内容如下

http://www.baidu.com/
http://demo:8888/
http://demo.local.com/
如果需要自动化测试，在服务部署后使用域名或者主机名+端口方式访问测试

