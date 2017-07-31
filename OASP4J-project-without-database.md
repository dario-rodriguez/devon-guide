OASP4J application template is made up with database , Restful webservice and security package. If someone don't want database in his oasp4j template application , he can the below steps to run the oasp4j application without database and not having the any error.

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

Another option is disable the database , remove the database package and it's implementation class from OASP4J template application.
   
 
 



   