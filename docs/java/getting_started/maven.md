# Maven Settings文件设置

将公司以来仓库添加到你的maven settings.xml文件中

```xml
<profile>
    <id>ics</id>
    <repositories>
        <repository>
            <id>releases</id>
            <url>
                https://devrepo.devcloud.huaweicloud.com/02/nexus/content/repositories/0f816cca7c2d460a95500553c42d7cef_1_0/
            </url>
            <releases>
                <enabled>true</enabled>
            </releases>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
        <repository>
            <id>snapshots</id>
            <url>
                https://devrepo.devcloud.huaweicloud.com/02/nexus/content/repositories/0f816cca7c2d460a95500553c42d7cef_2_0/
            </url>
            <releases>
                <enabled>false</enabled>
            </releases>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>
</profile>
```

> 相关的账号密码找各自的负责人要

### 添加阿里云加速镜像

```xml
<mirrors>
    <mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>        
    </mirror>
</mirrors>
```