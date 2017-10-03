       Spring boot admin is monitoring Application to manage multiple Spring application.We can monitor application status, metrics and also the notification with email or with other chat application. We can monitor with help of Spring admin client or discovered using Spring Cloud (e.g. Eureka). The UI is just an AngularJs application on top of the Spring Boot Actuator endpoints.

## Configure the Spring boot admin for the OASP4J App.  

* Setup Admin server
* Configure the client app.
* Loglevel management.
* Setup the email notification.
* Setup Slack notification.

 ### Setup Admin server
 To create Admin server create the Spring.io Application and follow the below steps. 
1. Add the code in spring boot class.

````java
@EnableAdminServer
@Configuration
@EnableDiscoveryClient
@SpringBootApplication
public class SpringBootApp{

  public static void main(String[] args) {

    SpringApplication.run(BootadminApplication.class, args);
  }

  @Configuration
  public static class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {

      // Page with login form is served as /login.html and does a POST on /login
      http.formLogin().loginPage("/login.html").loginProcessingUrl("/login").permitAll();
      // The UI does a POST on /logout on logout
      http.logout().logoutUrl("/logout");
      // The ui currently doesn't support csrf
      http.csrf().disable();

      // Requests for the login page and the static assets are allowed
      http.authorizeRequests().antMatchers("/login.html", "/**/*.css", "/img/**", "/third-party/**").permitAll();
      // ... and any other request needs to be authorized
      http.authorizeRequests().antMatchers("/**").authenticated();

      // Enable so that the clients can authenticate via HTTP basic for registering
      http.httpBasic();
    }
  }

}
```` 
2. Add the dependency.
 ````XML

 <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-security</artifactId>
 </dependency>

<dependency>
      <groupId>de.codecentric</groupId>
      <artifactId>spring-boot-admin-server-ui-login</artifactId>
      <version>1.5.3</version>
</dependency>

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
3. Add the properties to the application.properties file. 
 
````java
spring.application.name=Admin-Application
server.port=1111

management.security.enabled=false
security.user.name=admin
security.user.password=admin123
````
### Configure the client app.
To configure the multiple application to Admin server add the below dependency and code to each app.

1. Add the dependency
````XML
<dependency>
      <groupId>de.codecentric</groupId>
      <artifactId>spring-boot-admin-starter-client</artifactId>
      <version>1.5.1</version>
</dependency>

<dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
    
 <dependency>
     <groupId>org.springframework.cloud</groupId>
     <artifactId>spring-cloud-starter-eureka</artifactId>
     <version>1.3.1.RELEASE</version>
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
### Setup the email notification.
1. Add the dependency

````XML
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
</dependency>
````
2 . Add the add the class NotifierConfiguration

````java
@Configuration
@EnableScheduling
public class NotifierConfiguration {
  @Autowired
  private Notifier delegate;

  @Bean
  public FilteringNotifier filteringNotifier() { 
    return new FilteringNotifier(delegate);
  }

  @Bean
  @Primary
  public RemindingNotifier remindingNotifier() { 
    RemindingNotifier notifier = new RemindingNotifier(filteringNotifier());
    notifier.setReminderPeriod(TimeUnit.SECONDS.toMillis(10));
    return notifier;
  }

  @Scheduled(fixedRate = 1_000L)
  public void remind() {
    remindingNotifier().sendReminders();
  }
}
````
3. Add the properties to the application.properties file. 

````java
spring.mail.host=smtp.gmail.com
spring.boot.admin.notify.mail.to=admin@gmail.com
spring.boot.admin.notify.mail.enabled=true
spring.boot.admin.notify.mail.to=user@gmail.com


````
### Setup Slack notification.
To enable Slack notifications you need to add a incoming Webhook under custom integrations on your Slack account and configure it appropriately.

1. Add the properties to the application.properties file. 

````java
spring.boot.admin.notify.slack.enabled=true
spring.boot.admin.notify.slack.username=user123
spring.boot.admin.notify.slack.channel=general
spring.boot.admin.notify.slack.webhook-url=https://hooks.slack.com/services/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
spring.boot.admin.notify.slack.message="#{application.name} (#{application.id}) is #{to.status}"

````


