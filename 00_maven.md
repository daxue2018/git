

# 修改MAVEN_OPTS

在系统的环境变量中，设置MAVEN_OPTS，用以存放JVM的参数，具体设置的步骤，参数示例如下：

　　MAVEN_OPTS=-Xms256m -Xmx768m -XX:PermSize=128m -XX:MaxPermSize=256M  

　　或者临时设置 export MAVEN_OPTS=-Xms256m -Xmx768m -XX:PermSize=128m -XX:MaxPermSize=256M

在mvn中添加MAVEN_OPTS

   找到Maven的安装目录，在bin目录下，编辑mvn.bat(linux下，mvn)

　　set MAVEN_OPTS=-Xms256m -Xmx768m -XX:PermSize=128m -XX:MaxPermSize=256M  


maven的setting配置文件放到用户目录下，防止覆盖

maven命令创建工程：mvn archetype:generate -DgroupId=com.daxue.maven -DartifactId=maven-test-app -DarchetypeArtfactId=maven-archetype-quickstart -DinteractiveMode=false



1. 创建第一个项目
GroupId com.daxue.maven
ArtifactId maven-helloword
Version 1.0.0-SNAPSHOT

mvn dependency:tree 查看依赖树

在settings.xml配置文件中，为当前机器统一配置使用的私服仓库地址，而且一般都是直接用私服中的仓库组，在settings.xml中用profiles即可。
<profiles>
	<profile>
		<id>nexus</id>
      	<repositories>
        		<repository>
          			<!-- <id>nexus</id> -->
          			<!-- id使用central可以覆盖maven的配置 -->
          			<id>central</id>
          			<name>Nexus</name>
      	  		    <!-- <url>http://localhost:8081/nexus/content/groups/public</url> -->
                    <!-- 其实是没意义的一个url，因为此时在需要从远程仓库下载依赖或者插件的时候，会从两个自己配置的central仓库去走，
                        然后看release或者snapshot是否支持，如果支持，那么就会找镜像配置，由于我们的镜像匹配所有请求，
                        所以所有请求都会走镜像，而镜像配置的是私服地址，所以相当于所有的请求都会走私服了。 -->
                    <url>http://central</url>
          			<releases><enabled>true</enabled></releases>
          			<snapshots><enabled>true</enabled></snapshots>
        		</repository>
      	</repositories>
      	<pluginRepositories>
        		<pluginRepository>
        		    <!-- <id>nexus</id> -->
                    <!-- id使用central可以覆盖maven的配置 -->
          			<id>central</id>
          			<name>Nexus Plugin Repository</name>
      			    <!-- <url>http://localhost:8081/nexus/content/groups/public</url> -->
      			    <!-- 其实是没意义的一个url，因为此时在需要从远程仓库下载依赖或者插件的时候，会从两个自己配置的central仓库去走，
      			        然后看release或者snapshot是否支持，如果支持，那么就会找镜像配置，由于我们的镜像匹配所有请求，
      			        所以所有请求都会走镜像，而镜像配置的是私服地址，所以相当于所有的请求都会走私服了。 -->
      			    <url>http://central</url>
          			<releases><enabled>true</enabled></releases>
          			<snapshots><enabled>true</enabled></snapshots>
        		</pluginRepository>
      	</pluginRepositories>
	</profile>
</profiles>

<!-- 激活对应的profile，在项目执行构建的时候，就会将profile中的仓库配置应用到每个项目中去 -->
<activeProfiles>
	<activeProfile>nexus</activeProfile>
</activeProfiles>

<mirrors>
    <!-- 设置镜像代理所有请求 -->
	<mirror>
		<id>nexus</id>
		<mirrorOf>*</mirrorOf>
		<url>http://localhost:8081/nexus/content/groups/public</url>
	</mirror>
</mirros>


将项目发布包部署到哪个仓库中，是需要用下面的pom.xml中的配置来设置的：

<distributionManagement>
	<repository>
		<id> nexus-releases</id>
		<name> Nexus Release Repository</name>
		<url>http://localhost:8081/nexus/content/repositories/releases/</url>
	</repository>
	<snapshotRepository>
		<id> nexus-snapshots</id>
		<name> Nexus Snapshot Repository</name>
		<url>http://localhost:8081/nexus/content/repositories/snapshots/</url>
	</snapshotRepository>
</distributionManagement>

需要在settings中配置：

<servers>
	<server>
		<id>nexus-releases</id>
		<username>deployment</username>
		<password>deployment123</password>
	</server>
	<server>
		<id>nexus-snapshots</id>
		<username>deployment</username>
		<password>deployment123</password>
	</server>
</servers>


jar包手动上传

有些第三方厂商的jar包，比如特殊数据库厂商的jdbc驱动，或者某个厂商的支付相关的api jar包，是不提供公网下载的，只能从他们那里拿到一个jar包，此时就需要手动将其上传到3rd party仓库里面去。

新版本里面，其实主要是建议用命令行手动上传的方式，就不用界面上上传的方式了：

选择3rd party，选择Artifact Upload，上传即可

	<server>
		<id>nexus-3rd-party</id>
		<username>deployment</username>
		<password>deployment123</password>
	</server>


mvn deploy:deploy-file -DgroupId=com.csource -DartifactId=fastdfs-client-java -Dversion=1.24 -Dpackaging=jar -Dfile=F:\DevelopmentKit\fastdfs_client_v1.24.jar -Durl=http://localhost:8081/repository/3rd-party/ -DrepositoryId=nexus-3rd-party 
















