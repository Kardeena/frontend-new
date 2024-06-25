<template>
  <div class="hello">
    <h1>Kardenas App</h1>
    <h2>What I'm doing</h2>	
    <h3>Eine Kalender-App</h3>
    <form @submit.prevent="createEvent" class="event-form">
      <input type="text" v-model="event.title" placeholder="Titel">
      <input type="datetime-local" v-model="event.start" placeholder="Startzeit">
      <input type="datetime-local" v-model="event.end" placeholder="Endzeit">
      <button type="submit">Event erstellen</button>
    </form>
    <h2>Existing Events</h2>
    <ul>
      <li v-for="existingEvent in events" :key="existingEvent.id">
        {{ existingEvent.title }}: {{ formatDate(existingEvent.start) }} - {{ formatDate(existingEvent.end) }}
        <button @click="deleteEvent(existingEvent.id)">Löschen</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      event: {
        title: '',
        start: '',
        end: ''
      },
      events: []
    }
  },
  methods: {
    createEvent() {
      axios.post('http://localhost:8080/events', this.event)
        .then(response => {
          console.log(response.data);
          this.event = { title: '', start: '', end: '' }; // Reset form
          this.getEvents(); // Refresh the list of events
        })
        .catch(error => {
          console.error(error);
        });
    },
    deleteEvent(id) {
      axios.delete(`http://localhost:8080/events/${id}`)
        .then(() => {
          this.getEvents(); // Refresh the list of events
        })
        .catch(error => {
          console.error(error);
        });
    },
    getEvents() {
      axios.get('http://localhost:8080/events')
        .then(response => {
          this.events = response.data;
        })
        .catch(error => {
          console.error(error);
        });
    },
    formatDate(dateString) {
      const options = { year: 'numeric', month: 'long', day: 'numeric', hour: '2-digit', minute: '2-digit' };
      return new Date(dateString).toLocaleString('de-DE', options);
    }
  },
  created() {
    this.getEvents(); // Get the list of events when the component is created
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2, h3 {
  color: #333;
}
.event-form {
  margin-bottom: 20px;
}
.event-form input {
  margin-right: 10px;
}
.event-form button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 5px 10px;
  cursor: pointer;
}
.event-form button:hover {
  background-color: #2a8f66;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: block;
  margin: 10px 0;
}
</style>