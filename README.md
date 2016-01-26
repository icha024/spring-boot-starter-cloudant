# Cloudant Auto-Configuration for Spring Boot
Provides convenient Cloudant dependency and configuration helper.
## Installing with Maven
```
<dependency>
  <groupId>com.clianz</groupId>
  <artifactId>spring-boot-starter-cloudant</artifactId>
  <version>0.0.1</version>
</dependency>
```
## Usage
### Using Cloudant client
Just inject the client:
```
@Autowired
CloudantClient cloudant
```
Then do some client API function, or just create database from it and start using it:
```
Database db = cloudant.database("mydb", true);
db.save(data);
```
### Using database directly
Alternatively, you may inject a database instance directly:
```
@Bean
public Database mydb(CloudantClient cloudant) {
	return cloudant.database("mydb", true);
}
```
Then start using it:
```
@Autowired
Database mydb
```
```
mydb.save(data);
```
## Configuration
### Spring Boot Configuration
Configurations can be place in the Application.properties (or yml, or json) as usual. The username and password is mandatory.
```
spring.data.cloudant.account=myAccountName   #Default to username will be user if blank.
spring.data.cloudant.username=myUserName     #Mandatory. The username as assigned by Cloudant.
spring.data.cloudant.password=myPasswd       #Mandatory. The password as assigned by Cloudant.
spring.data.cloudant.url=Cloudant URL        #Defaults to official server.
spring.data.cloudant.proxyURL=http://...     #URL to proxy server
spring.data.cloudant.proxyUser=myUserName    #Proxy username
spring.data.cloudant.proxyPassword=myPasswd  #Proxy password.
spring.data.cloudant.connectTimeout=300      #Connect timeout in seconds. Default to 300 seconds (5 minutes).
spring.data.cloudant.readTimeout=300         #Read timeout in seconds. Default to 300 seconds (5 minutes).
spring.data.cloudant.maxConnections=5        #Default to 6.
spring.data.cloudant.disableSSLAuthentication=false   #Defaults to false.
```
### Bluemix (CloudFoundry) Configuration
When using Bluemix (CloudFoundry), will client will automatically use the Cloudant service binded to the app instead of the Spring configuration.

Bluemix's VCAP_SERVICES environment variable containing a Cloudant service will always take precedence over any Spring configuration. This is useful - Local development will use the Spring configuration properties, and promoting it to Bluemix will automatically use the environment configured instance. If Spring's configuration is desired, just remove the Cloudant service binding from your Bluemix app.

## Requirements
- Java 1.6+
- Official Cloudant client 2.x