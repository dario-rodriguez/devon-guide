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

