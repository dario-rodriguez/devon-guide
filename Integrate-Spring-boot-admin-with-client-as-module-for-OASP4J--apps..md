Spring boot admin is an application to monitor and manage the different Spring.io applications.There are the different way to configure spring boot admin with spring.io apps. As many features available in it, you can configure as per your requirement. The documents you can refer [[ Spring boot admin Integration with OASP4J|https://github.com/devonfw/devon-guide/wiki/Spring-boot-admin-Integration-with-OASP4J]]  or  [[ Spring Boot Admin Reference Guide|http://codecentric.github.io/spring-boot-admin/1.5.3/#getting-started]].  We can also configure the spring boot admin with OASP4J modules, please follow the below steps. 

### Spring boot Admin server

You can directly check out the Spring boot Admin server from the [[ repo |https://github.com/Himanshu122798/spring-bootadmin-server]] or you can create own using the above reference document. 

###  Configure OASP4J app as a client. 
  
  **1. Add the module dependency.  
  
```xml
  <dependency>
      <groupId>com.capgemini.devonfw.modules</groupId>
      <artifactId>devonfw-springbootadminclient</artifactId>
      <version>2.2.0</version>
  </dependency>
``` 
 **2. Add the below property and change the values as per server configuration like admin.url , username, password 

```xml
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
```

 

 