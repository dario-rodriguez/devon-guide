Spring Boot Admin is application to manage and monitor your Spring Boot Applications. The applications register with our Spring Boot Admin Client (via HTTP) or are discovered using Spring Cloud (e.g. Eureka). The UI is just an AngularJs application on top of the Spring Boot Actuator endpoints. It 

 ### Setup the Spring boot Admin server
  To run the spring boot admin. First, you need to setup admin server. To do this create the spring.io project and follow the below steps.  

1. Add Spring Boot Admin Server and the UI dependency to the pom.xml file. 
````Xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-server</artifactId>
    <version>1.5.3</version>
</dependency>
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-server-ui</artifactId>
    <version>1.5.3</version>
</dependency>
````
  

2. Add the Spring Boot Admin Server configuration via adding @EnableAdminServer to your spring boot class.

````java

@Configuration
@EnableAutoConfiguration
@EnableAdminServer
public class SpringBootApp{
    public static void main(String[] args) {
        SpringApplication.run(SpringBootAdminApplication.class, args);
    }
}
```` 
3. If you want the login for the sever , then add the login depedency . 

````Xml
<dependency>
    <groupId>de.codecentric</groupId>
    <artifactId>spring-boot-admin-server-ui-login</artifactId>
    <version>1.5.3</version>
</dependency>

````

4. Add the properties to the application.properties file. 
 
````java
spring.application.name=Admin-Application
server.port=1111

management.security.enabled=false
security.user.name=admin
security.user.password=admin123
````
### Register the client app.

Spring boot admin gives the monitoring status of multiple spring.io application.These applications are registered as the client application to spring boot admin server. 

###Register with spring-boot-admin-starter-client  

````XML
<dependency>
      <groupId>de.codecentric</groupId>
      <artifactId>spring-boot-admin-starter-client</artifactId>
      <version>1.5.3</version>
</dependency>

````
2. Add the annotation @EnableEurekaClient to spring boot class

````java
@EnableEurekaClient
@SpringBootApplication // (exclude = { EndpointAutoConfiguration.class, ErrorMvcAutoConfiguration.class })
@EntityScan(basePackages = { "com.carpool" }, basePackageClasses = { AdvancedRevisionEntity.class })
@EnableGlobalMethodSecurity(jsr250Enabled = true)
public class SpringBootApp {

  /**
   * Entry point for spring-boot based app
   *
   * @param args - arguments
   */
  public static void main(String[] args) {

    SpringApplication.run(SpringBootApp.class, args);

  }

}
````

3. Add the properties to the application.properties file. 

````java
info.version=@project.version@

eureka.client.serviceUrl.defaultZone=${EUREKA_URI:http://localhost:8180/eureka}
spring.boot.admin.url=http://localhost:1111
management.security.enabled=false
spring.boot.admin.username=admin
spring.boot.admin.password=admin123
logging.file=target/${spring.application.name}.log

eureka.instance.hostname=localhost
eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false

health.config.enabled=true 
````
### Loglevel management

1. Add dependency. 

````XML
<dependency>
    <groupId>org.jolokia</groupId>
    <artifactId>jolokia-core</artifactId>
</dependency>
````
2. Add the logback-spring.xml file in resorce folder. 

````XML
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
	<include resource="org/springframework/boot/logging/logback/base.xml"/>
	<jmxConfigurator/>
</configuration>
````


