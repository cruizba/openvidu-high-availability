# openvidu-high-availability

This project exemplifies the reconnection capabilities of an application making use of OpenVidu Enterprise environment, whenever a node crashes.

## Compile and run the app

This is a SpringBoot application. Prerequisites:

| Dependency    | Check version   | Install                                 |
| ------------- | --------------- |---------------------------------------- |
| Java 11 JDK    | `java -version` | `sudo apt-get install -y openjdk-11-jdk` |
| Maven         | `mvn -v`        | `sudo apt-get install -y maven`         |

To compile and run the app:

```
git clone https://github.com/OpenVidu/openvidu-high-availability.git
cd openvidu-high-availability
mvn clean package
java -jar target/openvidu-high-availability-*.jar --openvidu.url=OPENVIDU_PRO_DOMAIN --openvidu.secret=OPENVIDU_SECRET
```

### Example
- `OPENVIDU_PRO_DOMAIN` = `https://example-openvidu.io`
- `OPENVIDU_SECRET` = `MY_SECRET`
```
git clone https://github.com/OpenVidu/openvidu-high-availability.git
cd openvidu-high-availability
mvn clean package
java -jar target/openvidu-high-availability-*.jar --openvidu.url=https://example-openvidu.io --openvidu.secret=MY_SECRET
```

## Test the reconnection capabilities

A session hosted in a Media Node suffering a crash will be automatically re-created and re-located in a different Media Node, without intervention of the final user. For this to work, the OpenVidu Enterprise cluster must have at least 2 running Media Nodes. To test the reconnection capabilities of the application:

1. Make sure your OpenVidu Enterprise cluster has at least 2 different Media Nodes.
2. Connect 2 different users to the same session. They should both send and receive each other's video.
3. Find out in which Media Node the session was located. You can call REST API method [GET Media Nodes](https://docs.openvidu.io/en/latest/reference-docs/REST-API/#get-openviduapimedia-nodes) to do so.
4. Terminate the machine hosting the session.
5. After 3~4 seconds both users should automatically re-join the same session, successfully re-establishing the video streams.
