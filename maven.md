![image-20240623003422121](C:\Users\32596\AppData\Roaming\Typora\typora-user-images\image-20240623003422121.png)

```xml
    <!--maven工程的坐标-->
    <groupId>com.hyx.maven</groupId>
    <artifactId>maven_java</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--maven工程的打包方式-->
    <packaging>jar</packaging>
```



在没有在 maven 指定文件夹下创建文件，打包时需要指定路径

```xml
 <build>
        <!--自定义打包名称-->
        <finalName>maven_java-1.0-jar</finalName>
        <resources>
            <resource>
                <!-- 设置资源所在目录 -->
                <directory>src/main/java</directory>
                <includes>
                    <!-- 设置包含的资源类型 -->
                    <include>**/*.xml</include>
                </includes>
            </resource>
        </resources>
    </build>
```

在build可以直接插入Tomcat插件









maven 依赖特性

依赖传递：

只有<scope>compile</scope>才可以进行传递

终止依赖传递：

![image-20240623073753278](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623073753278.png)



![image-20240623073813862](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623073813862.png)







![image-20240623074019371](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623074019371.png)

![image-20240623074042294](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623074042294.png)

手动解决依赖冲突：

![image-20240623075620323](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623075620323.png)





创建父工程和子工程

父工程

```xml
    <!--  由于父工程不需要参与打包，因此打包方式pom  -->
    <packaging>pom</packaging>
```

子工程

```xml
<!--    设置父工程的坐标-->
    <parent>
        <groupId>com.hyx.maven</groupId>
        <artifactId>maven_parent</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>

    <artifactId>maven_son</artifactId>
```

如果用<dependencies></dependencies>，则子类则会无条件继承父类的依赖

如果用    <dependencyManagement></dependencyManagement>，则在子类中需要调用父类依赖，不用配置版本号

```xml
父类   
<dependencyManagement>
        <dependencies>
            <!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
            <dependency>
                <groupId>com.alibaba</groupId>
                <artifactId>druid</artifactId>
                <version>1.2.8</version>
            </dependency>
            <!-- https://mvnrepository.com/artifact/org.junit.jupiter/junit-jupiter-api -->
            <dependency>
                <groupId>org.junit.jupiter</groupId>
                <artifactId>junit-jupiter-api</artifactId>
                <version>5.8.2</version>
                <scope>test</scope>
            </dependency>
        </dependencies>
  </dependencyManagement>
子类
    <dependencies>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
        </dependency>
    </dependencies>
```



一键管理  一键构建

```xml
    <modules>
        <module>maven_son</module> //maven_son：这个是子工程的路径
    </modules>
```





##### maven 私服

![image-20240623081832108](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623081832108.png)

![image-20240623081912124](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623081912124.png)

![image-20240623081952080](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623081952080.png)

![image-20240623082145577](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623082145577.png)



启动nexus，需要在window power shell 以管理员身份运行：

```
./nexus /run
```

![image-20240623083147986](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623083147986.png)

![image-20240623083515545](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623083515545.png)



![image-20240623083809932](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623083809932.png)

```xml
	 <mirror>
		<id>neuxs-mine</id>
		<mirrorOf>central</mirrorOf>
		<name>Nexus mine</name>
		<url>http://localhost:8081/repository/maven-public/</url>
	</mirror>
```



![image-20240623084330941](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623084330941.png)

```xml
  <servers>
	<server>
      <id>neuxs-mine</id>
      <username>admin</username>
      <password>123456</password>
    </server>
```

![image-20240623084654702](C:/Users/32596/AppData/Roaming/Typora/typora-user-images/image-20240623084654702.png)

修改后自动导入新私服