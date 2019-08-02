<template>
  <div class="home">
    <h1>New Contact</h1>
    <div>
      <p>First Name: <input type="text" v-model="newContactFirstName"></p>
      <p>Middle Name: <input type="text" v-model="newContactMiddleName"></p>
      <p>Last Name: <input type="text" v-model="newContactLastName"></p>
      <p>Email: <input type="text" v-model="newContactEmail"></p>
      <p>Phone: <input type="text" v-model="newContactPhone"></p>
      <p>Bio: <input type="text" v-model="newContactBio"></p>
      <button v-on:click="createContact()">Save Contact</button>
    </div>

    <h1>All Contacts</h1>
    <div v-for="contact in contacts">
      <h2>{{ contact.first_name }} {{ contact.middle_name }} {{ contact.last_name }}</h2>
<!--       <h2>{{ contact.middle_name }}</h2>
      <h2>{{ contact.last_name }}</h2> -->
      <button v-on:click="showInfo(contact)">Show Info</button>
        <div v-if="currentContact === contact">
          <h3>Email: {{ contact.email }}</h3>
          <h3>Phone: {{ contact.phone_number }}</h3>
          <h3>Bio: {{ contact.bio }}</h3>
          <h4>Edit Contact Details Below</h4>
          <div>
              <p>First Name: <input type="text" v-model="contact.first_name"></p>
              <p>Middle Name: <input type="text" v-model="contact.middle_name"></p>
              <p>Last Name: <input type="text" v-model="contact.last_name"></p>
              <p>Email: <input type="text" v-model="contact.email"></p>
              <p>Phone: <input type="text" v-model="contact.phone_number"></p>
              <p>Bio: <input type="text" v-model="contact.bio"></p>
              <button v-on:click="updateContact(contact)">Update Contact</button>
          </div>
          <hr>
          <button v-on:click="deleteContact(contact)">Delete Contact</button>
        </div>
      <hr>
      <hr>
<!--       <img v-bind:src="photo.url"> -->
<!--       <p>Width: {{ photo.width }}</p>
      <p>Height: {{ photo.height }}</p> -->
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      contacts: [],
      currentContact: {},
      newContactFirstName: "",
      newContactMiddleName: "",
      newContactLastName: "",
      newContactEmail: "",
      newContactPhone: "",
      newContactBio: ""
    };
  },
  created: function() {
    axios.get("/api/contacts").then(response => {
      this.contacts = response.data;
      console.log(response.data);
    });
  },
  methods: {
    showInfo: function(contact) {
      if (this.currentContact === contact) {
        this.currentContact = {};
      } else {
        this.currentContact = contact;
      }
    },
    createContact: function() {
      var params = {
        first_name: this.newContactFirstName,
        last_name: this.newContactLastName,
        middle_name: this.newContactMiddleName,
        email: this.newContactEmail,
        phone_number: this.newContactPhone,
        bio: this.newContactBio
      };
      axios.post('/api/contacts', params).then(response => {
        this.contacts.push(response.data);
      });
    },
    deleteContact: function(contact) {
      console.log("deleteContact function says bye bitch!");
      console.log(contact);
      axios.delete('/api/contacts/' + contact.id).then(response => {
        var index = this.contacts.indexOf(contact);
        this.contacts.splice(index, 1);
      });
    },
    updateContact: function(contact) {
      console.log("what's Gucci fromt the updateContact function!");
      console.log(contact);
      var params = {
        first_name: contact.first_name,
        middle_name: contact.middle_name,
        last_name: contact.last_name,
        email: contact.email,
        phone_number: contact.phone_number,
        bio: contact.bio,
      };
    }
  }
};
</script>


