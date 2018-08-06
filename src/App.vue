<template>
  <div id="app">
    <img src="./assets/logo.png">
    <h1>{{ msg }}</h1>
      <h2>To add a contact, please enter the contact details and click  '+'</h2>
      <h2>To remove, just click  '-' next to the contact</h2>
      
      <input type="text" v-model="newContactName" placeholder="First Name"/>
      <input type="text" v-model="newContactLastName" placeholder="Last Name"/>
      <input type="tel" name="phone"
            placeholder="123-456-7890"
            pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}" maxlength="10" 
            v-model="newContactNumber"
            @keydown="keymonitor"
            required/>
      <button v-on:click="addNumber">+</button>
      
      <ul>
        <li v-for="(contact, index) in contacts" v-bind:key="contact.id"> 
          <div>{{ contact.name }} {{ contact.lastName }} - {{ contact.number }} </div><button v-on:click='removeNumber( index )'>-</button> 
        </li>
      </ul>
      <span v-if="invalidNumber" class="error">Please enter atleast first name and a valid phone number</span>
  </div>
</template>

<script>

let contactsFromLocalStorage = JSON.parse(localStorage.getItem('contacts'));
 
 



export default {
  name: 'App',
  data () {
    return {
      newContactName: null,
      newContactNumber: null,
      newContactLastName: null,
      msg: 'Welcome to my Phone book App created in Vue.js  :)',
      invalidNumber: false,
      contacts: contactsFromLocalStorage
    }
  }, methods: {
    addNumber: function () {
      if (this.newContactName && this.newContactNumber ) {
        this.contacts.push({
          name: this.newContactName, number: this.newContactNumber, lastName: this.newContactLastName
        });
        
        localStorage.setItem('contacts', JSON.stringify( this.contacts));

         this.newContactName = '';
        this.newContactNumber = '';
        this.newContactLastName = '';
      } else {
        this.invalidNumber = true;
      }
      
    },
    removeNumber: function (index){
      this.contacts.splice(index, 1);
      
        localStorage.setItem('contacts', JSON.stringify( this.contacts));
    },
    keymonitor(e) {
      var key   = e.keyCode ? e.keyCode : e.which;
      this.invalidNumber = false;
      if (!( [8, 9, 13, 27, 46, 110, 190].indexOf(key) !== -1 ||
         (key == 65 && ( e.ctrlKey || e.metaKey  ) ) || 
         (key >= 35 && key <= 40) ||
         (key >= 48 && key <= 57 && !(e.shiftKey || e.altKey)) ||
         (key >= 96 && key <= 105) 
       )) e.preventDefault();
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li { 
  margin: 0 10px;
}
li div {
  min-width: 200px;
  display: inline-block;
  text-align: left;
}
a {
  color: #42b983;
}
button{
  background-color: #42B883; /* Green */
  border: none;
  color: white;
  padding: 10px 16px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  border-radius: 40px;
}
.error{
  color: red;
}
</style>
