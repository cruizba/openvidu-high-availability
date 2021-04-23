# multi-master-sample-app

This project exemplifies the reconnection capabilities of an application making use of OpenVidu Pro environment, whenever a node crashes.

This is a SpringBoot application. Prerequisites:

| Dependency    | Check version   | Install                                 |
| ------------- | --------------- |---------------------------------------- |
| Java 11 JDK    | `java -version` | `sudo apt-get install -y openjdk-11-jdk` |
| Maven         | `mvn -v`        | `sudo apt-get install -y maven`         |

To compile and run the app:

```
git clone https://github.com/OpenVidu/multi-master-sample-app.git
cd multi-master-sample-app
mvn clean package
java -jar target/multi-master-sample-app-*.jar
```

The application by default listens on port 5000 with a self-signed SSL certificate that must be accepted on the browser.
The application offers the following endpoint to receive OpenVidu Webhook events:

```
https://DOMAIN_OR_IP:PORT/api/webhook
```

This endpoint must be configured in OpenVidu Pro with property `OPENVIDU_WEBHOOK_ENDPOINT`:

```
OPENVIDU_WEBHOOK=TRUE
OPENVIDU_WEBHOOK_ENDPOINT=https://DOMAIN_OR_IP:PORT/api/webhook
```