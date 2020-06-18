# Keycloak BCrypt

Add a password hash provider to handle BCrypt passwords inside Keycloak.

## Build
```bash
mvn clean package
```

## Test with docker-compose
```bash
./init-keycloak.sh
```

## Install
```
curl https://repo1.maven.org/maven2/org/mindrot/jbcrypt/0.4/jbcrypt-0.4.jar > jbcrypt-0.4.jar
KEYCLOAK_HOME/bin/jboss-cli.sh --command="module add --name=org.mindrot.jbcrypt --resources=jbcrypt-0.4.jar"
curl -L https://github.com/leroyguillaume/keycloak-bcrypt/releases/download/1.4.0/keycloak-bcrypt-1.4.0.jar > KEYCLOAK_HOME/standalone/deployments/keycloak-bcrypt-1.4.0.jar
```
You need to restart Keycloak.

## TODO

One have to make string replacement in verify method to adopt to $2y/$2x scheme.
https://stackoverflow.com/questions/49709857/hash-a-password-with-2y-in-java
```
import org.mindrot.jbcrypt.BCrypt;

public class hello {
	
	public static void main(String[] args){
String password = "welcome";
String candidate = "welcome";
// Hash a password for the first time
String hashed = BCrypt.hashpw(password, BCrypt.gensalt());

// gensalt's log_rounds parameter determines the complexity
// the work factor is 2**log_rounds, and the default is 10

// String hashed = BCrypt.hashpw(password, BCrypt.gensalt(12));

	System.out.println(hashed);

// Check that an unencrypted password matches one that has
// previously been hashed

candidate = "welcome1";
System.out.println(candidate);
hashed = "$2y$10$NmjhjLjmgQARQ0urHIcOoOQq3LYagJEGsyngryWRW9FZ1gZO5uorG";
System.out.println(hashed);
String hashed2 = "$2a" + hashed.substring(3);
System.out.println(hashed2);
if (BCrypt.checkpw(candidate, hashed2))
	System.out.println("It matches");
else
	System.out.println("It does not match");

	}

}


```
