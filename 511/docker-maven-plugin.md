

### 1.docker daemon(192.168.1.140)开放2375端口

` vi /usr/lib/systemd/system/docker.service`



```shell
[Unit]
Description=Docker Application Container Engine
Documentation=http://docs.docker.com
After=network.target
Wants=docker-storage-setup.service
Requires=docker-cleanup.timer

[Service]
Type=notify
NotifyAccess=all
EnvironmentFile=-/etc/sysconfig/docker
EnvironmentFile=-/etc/sysconfig/docker-storage
EnvironmentFile=-/etc/sysconfig/docker-network
Environment=GOTRACEBACK=crash
Environment=DOCKER_HTTP_HOST_COMPAT=1
Environment=PATH=/usr/libexec/docker:/usr/bin:/usr/sbin
ExecStart=/usr/bin/dockerd-current -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock \
```





### 2.私有docker registry(192.168.1.140)

`docker run -d -p 5000:5000 --restart always --name registry registry:2`



### 3 客户端

- maven的pom.xml

  ```Xml
  <plugin>
  				<groupId>com.spotify</groupId>
  				<artifactId>docker-maven-plugin</artifactId>
  				<version>0.4.13</version>
  				<configuration>
  					<imageName>localhost:5000/v2/${project.name}:${project.version}</imageName>
  					<dockerDirectory>${project.basedir}/src/main/docker</dockerDirectory>
  					<skipDockerBuild>false</skipDockerBuild>
  					<pushImage>true</pushImage>
  					<resources>
  						<resource>
  							<directory>${project.build.directory}</directory>
  							<include>${project.build.finalName}.jar</include>
  						</resource>
  					</resources>
  				</configuration>
  			</plugin>
  ```

  ​


- 指定环境变量`export DOCKER_HOST=tcp://192.168.1.140:2375`
- 编译语句`mvn clean package docker:build -DpushImage`