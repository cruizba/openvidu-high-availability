# multi-master-sample-app

This project exemplifies the reconnection capabilities of an application making use of OpenVidu Pro environment, whenever a node crashes.

## Compile and run the app

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
https://SAMPLE_APP_DOMAIN_OR_IP:PORT/api/webhook
```

This endpoint must be configured in OpenVidu Pro with property `OPENVIDU_WEBHOOK_ENDPOINT`:

```
OPENVIDU_WEBHOOK=TRUE
OPENVIDU_WEBHOOK_ENDPOINT=https://SAMPLE_APP_DOMAIN_OR_IP:PORT/api/webhook
```

To configure this in your multiple OpenVidu Pro Master nodes, you just need to do a `POST` to OpenVidu PRO with these parameters specified:
```
curl -XPOST -u OPENVIDUAPP:OPENVIDU_SECRET -d '{
    "OPENVIDU_WEBHOOK": true
    "OPENVIDU_WEBHOOK_ENDPOINT": "https://SAMPLE_APP_DOMAIN_OR_IP:PORT/api/webhook"
}' 'https://OPENVIDU_PRO_DOMAIN/openvidu/api/restart'
```

## Test the reconnection capabilities

A session hosted in a Media Node suffering a crash will be automatically re-created and re-located in a different Media Node, without intervention of the final user. For this to work, the OpenVidu Pro cluster must have at least 2 running Media Nodes. To test the reconnection capabilities of the application:

1. Make sure your OpenVidu Pro cluster has at least 2 different Media Nodes.
2. Connect 2 different users to the same session. They should both send and receive each other's video.
3. Find out in which Media Node the session was located. You can call REST API method [GET Media Nodes](https://docs.openvidu.io/en/latest/reference-docs/REST-API/#get-openviduapimedia-nodes) to do so.
4. Terminate the machine hosting the session.
5. After 3~4 seconds both users should automatically re-join the same session, successfully re-establishing the video streams.
