pom模板

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>socket-learn</artifactId>
    <version>1.0-SNAPSHOT</version>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.7.RELEASE</version>
        <relativePath />
    </parent>

    <properties>
        <java.version>1.8</java.version>
        <spring.boot.version>1.5.10.RELEASE</spring.boot.version>
        <spring.cloud.version>Edgware.SR3</spring.cloud.version>
        <mybatis.version>1.2.0</mybatis.version>
        <mybatis.plugin.version>1.3.4</mybatis.plugin.version>
        <druid.version>1.1.3</druid.version>
        <lombok.version>1.16.12</lombok.version>
        <fastjson.version>1.2.70</fastjson.version>
        <jjwt.version>0.7.0</jjwt.version>
        <swagger.version>2.7.0</swagger.version>
        <pingyin.version>2.5.1</pingyin.version>
        <commons.lang.version>2.6</commons.lang.version>
        <commons.lang3.version>3.5</commons.lang3.version>
        <commons.io.version>1.3.2</commons.io.version>
        <apache.commons.version>1.10</apache.commons.version>
        <google.guava.version>20.0</google.guava.version>
        <pagehelper.version>1.2.3</pagehelper.version>
        <apache.poi.version>3.15</apache.poi.version>
        <apache.maven.version>2.3.2</apache.maven.version>
        <apache.maven.war.version>3.0.0</apache.maven.war.version>
        <jsoup.version>1.11.2</jsoup.version>
        <tk.mybatis.version>3.4.0</tk.mybatis.version>
        <fastdfs.client.version>1.25.2-RELEASE</fastdfs.client.version>
        <jpush.version>3.3.9</jpush.version>
        <mina.version>2.0.9</mina.version>
        <oss.version>2.7.0</oss.version>
        <logback.version>1.2.3</logback.version>
        <maven.test.skip>true</maven.test.skip>
    </properties>

    <dependencies>
        <!-- springboot start -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>2.9.3</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.session</groupId>
            <artifactId>spring-session-data-redis</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.tomcat</groupId>
                    <artifactId>tomcat-jdbc</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <!--邮件模块依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>

        <!-- springboot end -->


        <!-- mybatis start -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.0.0</version>
        </dependency>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.10</version>
        </dependency>
        <!-- mybatis end -->
        
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>RELEASE</version>
            <scope>compile</scope>
        </dependency>


        <!-- excel start -->
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi</artifactId>
            <version>${apache.poi.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>${apache.poi.version}</version>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.xmlbeans</groupId>
                    <artifactId>xmlbeans</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
        <dependency>
            <groupId>org.apache.xmlbeans</groupId>
            <artifactId>xmlbeans</artifactId>
            <version>3.1.0</version>
        </dependency>
        <!-- excel end -->

        <!-- tools start -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>${fastjson.version}</version>
        </dependency>
        <dependency>
            <groupId>com.belerweb</groupId>
            <artifactId>pinyin4j</artifactId>
            <version>${pingyin.version}</version>
        </dependency>
        <dependency>
            <groupId>commons-lang</groupId>
            <artifactId>commons-lang</artifactId>
            <version>${commons.lang.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>${commons.lang3.version}</version>
        </dependency>
        <dependency>
            <groupId>commons-io</groupId>
            <artifactId>commons-io</artifactId>
            <version>${commons.lang.version}</version>
        </dependency>
        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>${google.guava.version}</version>
        </dependency>
        <dependency>
            <groupId>org.jsoup</groupId>
            <artifactId>jsoup</artifactId>
            <version>${jsoup.version}</version>
        </dependency>
        <dependency>
            <groupId>com.aliyun.oss</groupId>
            <artifactId>aliyun-sdk-oss</artifactId>
            <version>${oss.version}</version>
        </dependency>
        <dependency>
            <groupId>com.github.penggle</groupId>
            <artifactId>kaptcha</artifactId>
            <version>2.3.2</version>
        </dependency>
        <dependency>
            <groupId>cz.mallat.uasparser</groupId>
            <artifactId>uasparser</artifactId>
            <version>0.6.0</version>
        </dependency>
        <dependency>
            <groupId>com.github.ua-parser</groupId>
            <artifactId>uap-java</artifactId>
            <version>1.4.3</version>
        </dependency>
        <dependency>
            <groupId>net.sourceforge.javacsv</groupId>
            <artifactId>javacsv</artifactId>
            <version>2.0</version>
        </dependency>
        <!-- tools end -->

        <dependency>
            <groupId>ws.schild</groupId>
            <artifactId>jave-core</artifactId>
            <version>2.4.5</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/ws.schild/jave-native-osx64 -->
        <dependency>
            <groupId>ws.schild</groupId>
            <artifactId>jave-native-osx64</artifactId>
            <version>2.4.5</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/ws.schild/jave-native-linux64 -->
        <dependency>
            <groupId>ws.schild</groupId>
            <artifactId>jave-native-linux64</artifactId>
            <version>2.4.5</version>
        </dependency>
        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.4</version>
        </dependency>

        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>${logback.version}</version>
        </dependency>

        <!--配置文件加密机制-->
        <dependency>
            <groupId>com.github.ulisesbocchio</groupId>
            <artifactId>jasypt-spring-boot-starter</artifactId>
            <version>3.0.3</version>
        </dependency>
        <dependency>
            <groupId>com.github.librepdf</groupId>
            <artifactId>openpdf</artifactId>
            <version>1.3.28.1</version>
        </dependency>
    </dependencies>

</project>
```