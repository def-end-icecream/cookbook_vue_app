<template>
  <div class="messages-index">
    <h1>New Message:</h1>
    <form v-on:submit.prevent="submit()">
      <textarea v-model="newMessageBody" id="" cols="30" rows="10"></textarea
      ><br />
      <input type="submit" />
    </form>
    <h1>All Messages:</h1>
    <div v-for="message in messages" :key="message.id">
      <p>
        {{ message.user_name }} {{ relativeDate(message.created_at) }}:
        {{ message.body }}
      </p>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import moment from "moment";
import ActionCable from "actioncable";

export default {
  data: function() {
    return {
      messages: [],
      newMessageBody: ""
    };
  },
  created: function() {
    axios.get("/api/messages").then((response) => {
      console.log(response.data);
      this.messages = response.data;
    });
    var cable = ActionCable.createConsumer("ws://localhost:3000/cable");
    cable.subscriptions.create("MessagesChannel", {
      connected: () => {
        // Called when the subscription is ready for use on the server
        console.log("Connected to MessagesChannel");
      },
      disconnected: () => {
        // Called when the subscription has been terminated by the server
      },
      received: (data) => {
        // Called when there's incoming data on the websocket for this channel
        console.log("Data from MessagesChannel:", data);
        this.messages.unshift(data); // update the messages in real time
      }
    });
  },
  methods: {
    submit: function() {
      var params = {
        body: this.newMessageBody
      };
      axios.post("/api/messages", params).then((response) => {
        console.log("Server has recieved post request!");
      });
    },
    relativeDate: function(date) {
      return moment(date).fromNow();
    }
  }
};
</script>
