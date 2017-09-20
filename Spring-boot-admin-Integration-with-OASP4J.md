  Spring boot admin is monitoring Application to manage multiple Spring application.We can monitor application status, metrics and also the notification with email or with other chat application. We can monitor with help of Spring admin client or discovered using Spring Cloud (e.g. Eureka). The UI is just an AngularJs application on top of the Spring Boot Actuator endpoints.

## Configure the Spring boot admin for the OASP4J App.  

* Setup Admin server
* Configure the client app.
* Add the log file.
* Setup the email notification.
* Setup Slack notification.

 ### Setup Admin server
 To create Admin server create the Spring.io Application and follow the below steps. 
1. Add the code in spring boot class.

````java
spring.autoconfigure.exclude=org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration, org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration
```` 

 

