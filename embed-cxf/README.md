# Embedding Apache CXF within webapp

I decided to write blog in order to improve my English as well as writing skills. And also I think it will help me
save what I have learned into my "Hard Disk Drive" 

## 1. Overview 
So in my first blog I will share how to embed Apache-CXF in your webapp.

## 2. Creating webapp

First, we need to create our webapp using [Maven](http://maven.apache.org).

```shell script
mvn archetype:generate -DgroupId=dev.ubcode.blogposts -DartifactId=embed-cxf -DarchetypeArtifactId=maven-archetype-webapp -DarchetypeVersion=1.4 -DinteractiveMode=false
```

The above code will us create directory name *embed-cxf* and this directory will look like below.

![direcory structure](https://github.com/uuganbold/blogposts/blob/embed-cxf/embed-cxf/images/dirstructure.png?raw=true=200px)

## 3. Importing Apache CXF

To import Apache CXF, we just need to add maven dependencies in **pom.xml** file.

```xml
    <!--CXF -->
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-frontend-jaxrs</artifactId>
      <version>3.3.3</version>
    </dependency>
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-transports-http</artifactId>
      <version>3.3.3</version>
    </dependency>
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-transports-http-jetty</artifactId>
      <version>3.3.3</version>
    </dependency>
```
If you developing web service client you also need to import client library.
```xml
    <dependency>
      <groupId>org.apache.cxf</groupId>
      <artifactId>cxf-rt-rs-client</artifactId>
      <version>3.3.3</version>
    </dependency>
```

## 4. Jackson
And also because we are going to use [Jackson](https://github.com/FasterXML/jackson-jaxrs-providers) to request and response objects, 
we need import it.

```xml
    <dependency>
      <groupId>com.fasterxml.jackson.jaxrs</groupId>
      <artifactId>jackson-jaxrs-json-provider</artifactId>
      <version>2.9.9</version>
    </dependency>
      <dependency>
         <groupId>com.fasterxml.jackson.dataformat</groupId>
         <artifactId>jackson-dataformat-xml</artifactId>
         <version>2.9.9</version>
      </dependency>
```
## 5. Spring
We can use [Springframework](https://spring.io/) to configure Apache CXF,
so we will import Springframework as well.

```xml
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.1.9.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.1.9.RELEASE</version>
    </dependency>
```

### 6. Logging
Programming would be nightmare without nicer log. We are importing [Logback](http://logback.qos.ch/)
to enable Spring's logs.

```xml
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.3</version>
    </dependency>
```

We are almost finishing now. Only thing we have to do is implementing web services.

### 7. Implementing WS

I will just use codes Professor Berhane Zewdie gave us  on the Web Services Programming course at [the university](https://www.luc.edu/)
to make it simple. I hope he won't mind.

First, we need to declare CXFServlet which delegates http request to our Rest Resource.
So web.xml file should looks like this.

```xml
<web-app>
  <display-name>RESTful Employee Services</display-name>
  <context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>WEB-INF/cxf.xml</param-value>
    <!-- param-value>classpath:com/company/hr/service/cxf.xml</param-value -->
  </context-param>
  <listener>
    <listener-class>
      org.springframework.web.context.ContextLoaderListener
    </listener-class>
  </listener>
  <servlet>
    <servlet-name>CXFServlet</servlet-name>
    <servlet-class>
      org.apache.cxf.transport.servlet.CXFServlet
    </servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>CXFServlet</servlet-name>
    <url-pattern>/*</url-pattern>
  </servlet-mapping>
</web-app>
```
It is saying CXFServlet will use Spring's WebApplicationContext from WEB-INF/cxf.xml file. 

Then we need create cxf.xml file and java codes, but it is not a thing I am intending to write. 
However, you can see the full project from the my [Github.](https://github.com/uuganbold/blogposts/tree/embed-cxf/embed-cxf).

After you created all things, your directory structure should look like below (excluding README and embed-cxf.iml)

![after structure](https://github.com/uuganbold/blogposts/blob/embed-cxf/embed-cxf/images/after.png?raw=true=200px)

### Building

I think any java IDE will detect maven's pom and configure you building. But if you want you can use Maven's goal to build the project.

```shell script
mvn package
```

This command will create you embed-cxf.war which can be deployed to Servlet Container (Tomcat).

