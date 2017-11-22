# Cloud IoT Core Java HTTP example

This sample app publishes data to Cloud Pub/Sub using the HTTP bridge provided
as part of Google Cloud IoT Core.

Note that before you can run the sample, you must configure a Google Cloud
PubSub topic for Cloud IoT Core and register a device as described in the
[parent README](../README.md).

## Setup

Run the following command to install the dependencies using Maven:

    mvn clean compile

## Running the sample

The following command summarizes the sample usage:

```
    mvn exec:java \
        -Dexec.mainClass="com.google.cloud.iot.examples.HttpExample" \
        -Dexec.args="-project_id=<your-iot-project> \
                     -registry_id=<your-registry-id> \
                     -device_id=<device-id> \
                     -private_key_file=<path-to-keyfile> \
                     -message_type=<event|state> \
                     -algorithm=<RS256|ES256>"
```

For example, if your project ID is `blue-jet-123`, your service account
credentials are stored in your home folder in creds.json and you have generated
your credentials using the [`generate_keys.sh`](../generate_keys.sh) script
provided in the parent folder, you can run the sample as:

```
    mvn exec:java \
        -Dexec.mainClass="com.google.cloud.iot.examples.HttpExample" \
        -Dexec.args="-project_id=blue-jet-123 \
                     -registry_id=my-registry \
                     -device_id=my-java-device \
                     -private_key_file=../rsa_private_pkcs8 \
                     -algorithm=RS256"
```

To publish state messages, run the sample as follows:

```
    mvn exec:java \
        -Dexec.mainClass="com.google.cloud.iot.examples.HttpExample" \
        -Dexec.args="-project_id=blue-jet-123 \
                     -registry_id=my-registry \
                     -device_id=my-java-device \
                     -private_key_file=../rsa_private_pkcs8 \
                     -message_type=state \
                     -algorithm=RS256"
```


## Reading the messages written by the sample client

1. Create a subscription to your topic.

```
    gcloud beta pubsub subscriptions create \
        projects/your-project-id/subscriptions/my-subscription \
        --topic device-events
```

2. Read messages published to the topic

```
    gcloud beta pubsub subscriptions pull --auto-ack \
        projects/my-iot-project/subscriptions/my-subscription
```