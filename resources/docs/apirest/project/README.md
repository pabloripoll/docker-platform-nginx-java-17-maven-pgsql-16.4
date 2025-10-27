# JAVA SpringBoot

If you want to be able to compile Java code (not just run it), you need the JDK, not just the JRE. The package you need on Alpine Linux is openjdk17 (not just openjdk-jre).

openjdk-jre (or openjdk17-jre): Only allows you to run Java applications.
openjdk17: Installs the full JDK (includes the compiler javac and other dev tools).
You can safely just install openjdk17—it provides the JDK and also includes the JRE.

Example Dockerfile line for Alpine Linux:
Dockerfile
RUN apk add --no-cache openjdk17
You do not need to install openjdk-jre separately if you install openjdk17, since the JDK includes the JRE.

Summary:

For Maven/Gradle builds or compiling Java, use openjdk17.
For running Java apps only, openjdk17-jre is enough.
You do not need both. Just use openjdk17 for development and builds.


```
.
├── src
│   └── main
│       └── java
│           └── com
│               └── example
│                   └── api
│                       ├── controller
│                       ├── entity
│                       ├── repository
│                       └── APIController.java
└── pom.xml
```


## Build and Run

Build the container
```bash
$ make apirest-create
```

Build the app:
```sh
$ make apirest-ssh

/var/www $ mvn clean package

[INFO] Scanning for projects...
Downloading ...
[INFO] Replacing main artifact /var/www/target/api-springboot-0.0.1-SNAPSHOT.jar with repackaged archive, adding nested dependencies in BOOT-INF/.
[INFO] The original artifact has been renamed to /var/www/target/api-springboot-0.0.1-SNAPSHOT.jar.original
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  24.314 s
[INFO] Finished at: 2025-10-25T13:41:49Z
[INFO] ------------------------------------------------------------------------
```

- Run the app:
```sh
$ java -jar target/api-springboot-0.0.1-SNAPSHOT.jar
$ cp target/api-springboot-0.0.1-SNAPSHOT.jar target/app.jar
```

- Test:
 Visit http://localhost:8080/hello

```bash
$ make apirest-info
JAVA SB API: NGINX 1.28 / JAVA 17
Container ID.: 58ea77779179
Name.........: miapxhd-oj-apirest-dev
Image........: miapxhd-oj-apirest-dev:alpine3.22-nginx1.26-java17
Memory.......: 512M
Host.........: 127.0.0.1:4001
Hostname.....: 192.168.1.41:4001
Docker.Host..: 172.18.0.2
NetworkID....: f37218e62f1b7a11be39e69b9cd029e3bf2532d095e5fda60a95ef4afaa00c6f
```

All toghether:
```bash
$ mvn clean package && cp target/api-springboot-0.0.1-SNAPSHOT.jar target/app.jar && supervisorctl -c /etc/supervisor/supervisord.conf reload
```

Test the application
```bash
$ mvn spring-boot:run
```