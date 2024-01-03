<template>
  <div>
    <h1>MQTT Vue 3 Example</h1>
    <div>
      <p v-for="(message, index) in receivedMessages" :key="index">
        Received message: {{ message }}
      </p>
    </div>
  </div>
</template>

<script setup>
import mqtt from "mqtt";

const brokerUrl = "103.176.179.177";
const options = {
  clientId: "mqtt-subscriber",
  clean: true,
};

const subscribedTopics = ref(["room_1"]);
const receivedMessages = ref([]);
let client = null;

const connectToBroker = () => {
  client = mqtt.connect(brokerUrl, options);

  client.on("connect", () => {
    console.log("Connected to Mosquitto broker");

    // Subscribe to topics
    subscribedTopics.value.forEach((topic) => {
      client.subscribe(topic);
    });

    // Publish a message
    const topicToPublish = "room_1_control";
    const messageToPublish = "Hello, MQTT!";
    client.publish(topicToPublish, messageToPublish);
  });

  client.on("message", (topic, message) => {
    console.log(`Received message on topic '${topic}': ${message.toString()}`);
    receivedMessages.value.push(message.toString());
  });

  client.on("offline", () => {
    console.log("Disconnected from Mosquitto broker");
  });

  client.on("error", (error) => {
    console.error("Error:", error);
  });

  // Handle connection close
  client.on("close", () => {
    console.log("Connection closed");
  });
};

onMounted(() => {
  connectToBroker();
});

onBeforeUnmount(() => {
  if (client) {
    client.end();
  }
});
</script>
