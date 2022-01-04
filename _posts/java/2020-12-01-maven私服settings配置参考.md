# maven打包强制更新依赖
1，打包命令方式，只能更新snapshots包
mvn clean install -U    ## Forces a check for missing releases and updated snapshots on remote repositories
2，setting.xml配置，用于程序部署的打包
```xml
<snapshots>
    <enabled>true</enabled>
    <updatePolicy>always</updatePolicy>
</snapshots>
<releases>
	<enabled>true</enabled>
    <updatePolicy>always</updatePolicy>
</releases>
```

# 配置deploy需注意
1，配置servers 
2，配置profile->properties: altSnapshotDeploymentRepository\altReleaseDeploymentRepository  
3，项目pom.xml中的build依赖maven-deploy-plugin版本一定要2.8以上，否则报错

# settings.xml
```xml
  <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <localRepository/>
    <interactiveMode/>
    <usePluginRegistry/>
    <offline/>
    <pluginGroups/>
    
    <!-- 用于deploy -->
  <servers>
	<server>
	  <id>releases</id>
	  <username>admin</username>
	  <password>xxx</password>
	</server>
	<server>
	  <id>snapshots</id>
	  <username>admin</username>
	  <password>xxx</password>
	</server>
  </servers>

<!-- 私服镜像 -->
    <mirrors>
	 <mirror>
	   <id>mynexus</id>
	   <mirrorOf>*</mirrorOf>
	   <name>Repository Mirror</name>
	   <url>http://maven.jinfeng.hu/repository/maven-public/</url>
	 </mirror>
        <!--mirror>
            <id>aliyunmaven</id>
            <mirrorOf>central</mirrorOf>
            <name>阿里云公共仓库</name>
            <url>https://maven.aliyun.com/repository/central</url>
        </mirror-->
      </mirrors>
    <proxies/>

    <profiles>
	  <profile>
		<id>mynexus</id>
		<repositories>
		  <repository>
			<id>nexus</id>
			<name>Public Repositories</name>
			<url>http://maven.jinfeng.hu/repository/maven-public/</url>
			<releases>
			  <enabled>true</enabled>
			</releases>
		  </repository>
		
		  <repository>
			<id>central</id>
			<name>Central Repositories</name>
			<url>http://maven.jinfeng.hu/repository/maven-central/</url>
			<releases>
			  <enabled>true</enabled>
			</releases>
			<snapshots>
			  <enabled>false</enabled>
			</snapshots>
		  </repository>
		  
		  <repository>
			<id>release</id>
			<name>Release Repositories</name>
			<url>http://maven.jinfeng.hu/repository/maven-releases/</url>
			<releases>
			  <enabled>true</enabled>
			</releases>
			<snapshots>
			  <enabled>false</enabled>
			</snapshots>
		  </repository>
		  
		  <repository>
			<id>snapshots</id>
			<name>Snapshot Repositories</name>
			<url>http://maven.jinfeng.hu/repository/maven-snapshots/</url>
			<releases>
			  <enabled>true</enabled>
			</releases>
			<snapshots>
			  <enabled>true</enabled>
			</snapshots>
		  </repository>
		</repositories>
		
		<pluginRepositories>
		  <pluginRepository>
			<id>plugins</id>
			<name>Plugin Repositories</name>
			<url>http://maven.jinfeng.hu/repository/maven-public/</url>
		  </pluginRepository>
		</pluginRepositories>
	  </profile>

    <!-- deploy配置 -->
     <profile>
        <id>nexus</id>
        <properties>
            <altSnapshotDeploymentRepository>snapshots::default::http://maven.jinfeng.hu/repository/maven-snapshots</altSnapshotDeploymentRepository>
            <altReleaseDeploymentRepository>releases::default::http://maven.jinfeng.hu/repository/maven-releases</altReleaseDeploymentRepository>
        </properties>
    </profile>
  </profiles>
  <activeProfiles>
    <activeProfile>nexus</activeProfile>
  </activeProfiles>
    
</settings>
```

# pom文件的build依赖
```xml
<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-deploy-plugin</artifactId>
                <version>2.8.2</version>
            </plugin>
        </plugins>
    </pluginManagement>
</build>
```
