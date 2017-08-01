OASP4J application template is made up with database , Restful webservice and security package. If someone don't want database in his oasp4j template application , he can follow the below steps to run the oasp4j application without database and without having any error.

### Add Property  
src->main->resources-> application.properties 

````java
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration, org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration
````
### Remove annotation 

Remove the @Configuration Annotation from the file com.carpool.general.batch.impl.config -> BeansBatchConfig

````java
@Configuration
public class BeansBatchConfig {

  private JobRepositoryFactoryBean jobRepository;
````

#### Remove @name annotation
  
Remove @name from Dao package class , which are related with dao class and from it's implementation class.

#### Remove dataaccess package

Another option is to disable the database , remove the database package and it's implementation class from OASP4J template application.
   
 #### Remove the dependences
 Remove the all dependency from the pom.xml file ,  that related with database . 

````java
    
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
    </dependency>

    Hibernate EntityManager for JPA (implementation)
    <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-entitymanager</artifactId>
    </dependency>

    Database
    <dependency>
      <groupId>com.h2database</groupId>
      <artifactId>h2</artifactId>
    </dependency>

    <dependency>
      <groupId>org.flywaydb</groupId>
      <artifactId>flyway-core</artifactId>
    </dependency>

    hibernate
    <dependency>
      <groupId>org.hibernate.javax.persistence</groupId>
      <artifactId>hibernate-jpa-2.1-api</artifactId>
    </dependency>
    <dependency>
      <groupId>cglib</groupId>
      <artifactId>cglib</artifactId>
    </dependency>
    <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-validator</artifactId>
    </dependency>
```` 
 
 



   