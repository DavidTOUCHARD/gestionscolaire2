# gestionscolairespringboot
Bonjour à tous


Prérequis : 
A la racine du projet git doit se trouver le POM.XML

Dans le POM.XML
<dependency>
		  <groupId>org.springframework.boot</groupId>
		  <artifactId>spring-boot-starter-jdbc</artifactId>
		</dependency>
		<dependency>
		  <groupId>org.postgresql</groupId>
		  <artifactId>postgresql</artifactId>
		</dependency>


<plugin>
        			<groupId>com.heroku.sdk</groupId>
				<artifactId>heroku-maven-plugin</artifactId>
				<version>3.0.2</version>
				<configuration>
					<appName>gestionscolaire2</appName>
					<processTypes>
				     		<web>java $JAVA_OPTS -cp target/classes:target/dependency/* Main</web>
				  	</processTypes>
				</configuration>
      			</plugin>


application.properties

spring.datasource.url=jdbc:postgres://krvivfjukgugqa:ee2c0ceaa87317c2c2a193413b38d6c10c47b200ecd585c923a37bc84c2a50c7@ec2-52-202-146-43.compute-1.amazonaws.com:5432/d870geaj4nc9q
spring.datasource.username=krvivfjukgugqa
spring.datasource.password=ee2c0ceaa87317c2c2a193413b38d6c10c47b200ecd585c923a37bc84c2a50c7
spring.datasource.port=5432
spring.jpa.hibernate.ddl-auto=create

spring.datasource.initialization-mode=always

spring.datasource.driverClassName=org.postgresql.Driver
spring.datasource.maxActive=10
spring.datasource.maxIdle=5
spring.datasource.minIdle=2
spring.datasource.initialSize=5
spring.datasource.removeAbandoned=true


Créer une classe DatabaseConfig
import com.zaxxer.hikari.*;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.*;
import javax.sql.DataSource;

@Configuration
public class DatabaseConfig {

  @Value("${spring.datasource.url}")
  private String dbUrl;

  @Bean
  public DataSource dataSource() {
      HikariConfig config = new HikariConfig();
      config.setJdbcUrl(dbUrl);
      return new HikariDataSource(config);
  }
}

Ajouter un fichier Procfile (fichier sans extension) pour deployer l'application
web: java -Dserver.port=$PORT $JAVA_OPTS -jar target/gestionscolairespringboot-0.0.1-SNAPSHOT.war

data.sql
Supprimer les ` car Postgre ne les accepte pas

Heroku : 
git pull
heroku create  (pour creer l'application heroku)
git push heroku master (push sur heroku)
heroku ps:scale web=1  (lecture du fichier Procfile)
heroku open (ouvre l'application)
heroku logs --tail (consulter les logs)


