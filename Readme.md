### How to configure the Spring boot REST API with Swagger? ###
Even though REST services are document itself by using hetoas. We can use Swagger to visualizing API.
- We need to add springfox swagger and swagger ui dependencies to out project
```XML
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.6.1</version>
    <scope>compile</scope>
</dependency>
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.6.1</version>
    <scope>compile</scope>
</dependency>
```
- Configure the swagger2 to identify the application api's
```JAVA
@Configuration
@EnableSwagger2
public class SwaggerConfig {
    @Bean
    public Docket productApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .select()                 
                .apis(RequestHandlerSelectors.basePackage("com.fuppino.controllers"))
                .paths(regex("/product.*"))
                .build();
    }
}
```

### What are different MicroServices patterns? ###
[ref](https://www.youtube.com/watch?v=iJVW7v8O9BU)
- Aggregator Pattern
	- Individual services with separate cache and DB
	- All independent services are called by a Aggregator component to combine the services result
	- Aggregator component can be Load balance, the public exposer is Aggregator component
	- Individual services are not exposed to outside
![Aggregator Pattern](https://github.com/dvinay/learning/blob/master/micorservice%20pattern%20images/Aggregator_Pattern.png)

- Proxy Pattern
	- Individual services with separate cache and DB
	- Instead of Aggregator component, It might have Proxy layer
	- the major difference between Aggregator component and Proxy Pattern, Aggregator combines the result. Where as Proxy Component do data convertion like binary or format convertion like JSON format etc.
	- Proxy component can be Load balance, the public exposer is Proxy component
![Proxy Pattern](Proxy_Pattern.png)

- Chained Pattern
	- It's like calling one service to other service
	- Client sees only the one public service, but internally it may call multiple services to prepare the data
	- dis.adv: if you have chained multiple services then it increases the client waiting time ot if one node got failed to generate the data, the remaining node behaviour changes.
![Chained Pattern](Chained_Pattern.png)



