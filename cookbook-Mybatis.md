 :toc: macro
toc::[]

= Integrate Mybatis module

== Introduction

http://www.mybatis.org/mybatis-3/[MyBatis]  is a persistence framework with support for custom SQL, stored procedures and advanced mappings. MyBatis eliminates almost all of the JDBC code and manual setting of parameters and retrieval of results. MyBatis can use simple XML or Annotations for configuration and map primitives, Map interfaces and Java POJOs (Plain Old Java Objects) to database records.

== Include MyBatis in a Devon project

The module provides you a handle to a mybatis framework, and a bunch of predefined operations for managing the objects inside the service, for your Devon applications.
To implement the module in a Devon project, you must follow these steps:

=== Step 1: Adding the dependency in your project

Include the module dependency in your pom.xml, verifying that the _version_ matches the last available version of the module
[source,xml]
----
<dependency>
  <groupId>com.capgemini.devonfw.modules</groupId>
  <artifactId>devonfw-mybatis</artifactId>
  <version>2.2.1-SNAPSHOT</version>
</dependency>
----

[WARNING]
====
The IP modules of Devonfw are stored in https://www.jfrog.com/artifactory/[Artifactory]. In case, you do not have access to that repository, as the modules are included in the Devonfw distribution, you can install them manually. To do so, open a Devonfw command line (_console.bat_), go to `Devon-dist\workspaces\examples\devon\modules` and execute the command `mvn install`.
If the project is already imported in Eclipse then update project: Right click on _project_ > _Maven_ > _Update Project_ > check the _Force update of Snapshot/Releases_ checkbox > _Ok_
====

=== Step 2: Properties configuration

In order to use the module for managing the cache service, it is necessary to define some connection parameters in the application.properties file in the project.
[source,xml]
----
# Compose for Redis module params
devon.redis.service.name = compose-for-redis
devon.redis.uri = redis://admin:RFAAVWWDZSXXXXX@sl-eu-lon-2-portal.1.dblayer.com:16552
devon.redis.runTests.enable = false
----

- The parameter `devon.redis.service.name` is used to lookup the service in a cloud environment from the environment variable information `VCAP_SERVICES`. Default value `compose-for-redis`.
- The parameter `devon.redis.uri` is used to indicate the service's uri in case we need to connect to the service outside of a cloud environment. There is no default value.
- The parameter `devon.redis.runTests.enable` is used for enabling the test execution. By default this execution is disabled. 

[NOTE]
====
- When the parameter `devon.redis.service.name` is indicated, and the application is running on a cloud environment, the parameter `devon.redis.uri` will be never used. 
- The paramenter `devon.redis.uri` is used when there is no value for the paramenter `devon.redis.service.name` or when  
 the application is not running on a cloud environment.
- In case the test execution is enabled (true) the service's uri (`devon.redis.uri`) must be indicated because the test execution context will be outside a cloud environment.
====

== Basic implementation
////
First and foremost, you need to add the Mapper scanner for dependency injection. To do so, you must add the following annotations in the _SpringBoot_ main class:

[source,java]
----
@MapperScan({ "io.oasp.application.mtsj.<component name>.mapper", "io.oasp.application.mtsj.<component name>.mapper" })
public class MyBootApp {

    [...]
}
----

Remember to include the package of the module in the _basePackages_ attribute of the `@ComponentScan` annotation alongside the packages for the rest of the relevant Spring Boot components.

[source,java]
----
@ComponentScan(basePackages = { "com.capgemini.devonfw.module.composeredis" , "my.other.component.location.package" })
----

As you can see, the _basePackages_ of the _@ComponentScan_ points to the Composeredis module package. Now, you can start using the module.
////
=== The injection of the module

To access the module functionalities, We need to create DAO - <Component>MybatisDao and extends AbstractGenericMybatisDao, Also Autowired Mapper and add constructor

[source,java]
----
@Service
public class <Component>MybatisDao<T, PK> extends AbstractGenericMybatisDao<T, PK> {

    @Autowired
  private <Component>Mapper mapper;
  
   @Autowired
  public <Component>MybatisDao(<Component>Mapper myBatisMapper) {
    super(myBatisMapper);
  }
  
  Add your methods here.

    [...]

}
----

Create a (Interface)Mapper, and extends GenericMybatisMapper
[source,java]
----
public interface <Component>Mapper extends GenericMybatisMapper<<Component>MybatisEntity, Long> {

    @Options(useGeneratedKeys = true)
  @Insert("Insert Query")
  void insert(<Component>MybatisEntity <Component>MybatisEntity);
  
  @Delete("delete query")
  void delete(long id);
 
 //This search query should be placed in mapper.xml
List<<component>MybatisEntity> fetch(@Param("searchCriteria") SearchCriteria searchCriteria, RowBounds rowBounds); 
  
  Add other methods here.

    [...]

}
----
Accessing <Component>MybatisDao in logic(<component>Impl)

[source,java]
----
public class <Component>Impl {

	//Autowired Dao
    @Autowired
  private <Component>MybatisDao <Component>MybatisDao;

  
  use the dao in the methods.

    [...]

}
----
