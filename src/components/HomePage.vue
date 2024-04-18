<script setup>
import { ref } from 'vue';

const isLoading = ref(true);
const events = ref([]);

const getEvents = () =>{
  fetch(`https://ui-projekt-default-rtdb.europe-west1.firebasedatabase.app/events.json`, {
    method: 'GET',
  })
  .then((rawData) => {
    return rawData.json();
  })
  .then((data) => {
    // Tilgå events inden i en Nested Structure
    events.value = Object.values(data)[0].events;
    console.log(events.value);
  })
  .finally(() => {
    isLoading.value = false;
  });
};

getEvents();

/* DELTAGER KODE */
const attendEvent = async (eventId) => {
    try {
        const response = await fetch('https://ui-projekt-default-rtdb.europe-west1.firebasedatabase.app/events.json');
        const data = await response.json();

        // Accessing events from the nested property
        const eventsData = Object.values(data)[0].events;

        if (!eventsData || eventsData.length === 0) {
        throw new Error('Ingen events fundet');
        }

        const eventIndex = eventsData.findIndex((e) => e.id === eventId);
        if (eventIndex === -1) {
        throw new Error('Event ikke fundet');
        }

        const event = eventsData[eventIndex];
        event.deltager += 1;
        
        const updatedEventsData = [...eventsData]; /* Spread Operator - alle elementer fra det eksisterende array eventsData og "sprede" dem ud som individuelle elementer i et nyt array.*/
        updatedEventsData[eventIndex] = event;

        await fetch('https://ui-projekt-default-rtdb.europe-west1.firebasedatabase.app/events/-Nucr-C5ANJidfw12Nba.json', {
        method: 'PATCH',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({ events: updatedEventsData }),
        });

        alert('Tak fordi du deltager til eventet!');
    } catch (error) {
        console.error('Fejl ved ja:', error.message);
        alert('Fejl ved ja: ' + error.message);
    }
};

/* DELTAGER IKKE KODE */
/* DELTAGER KODE */
const noToEvent = async (eventId) => {
    try {
        const response = await fetch('https://ui-projekt-default-rtdb.europe-west1.firebasedatabase.app/events.json');
        const data = await response.json();

        const eventsData = Object.values(data)[0].events;

        if (!eventsData || eventsData.length === 0) {
        throw new Error('Ingen events fundet');
        }

        const eventIndex = eventsData.findIndex((e) => e.id === eventId);
        if (eventIndex === -1) {
        throw new Error('Event ikke fundet');
        }

        const event = eventsData[eventIndex];
        event.deltager -= 1;
        
        const updatedEventsData = [...eventsData];
        updatedEventsData[eventIndex] = event;

        await fetch('https://ui-projekt-default-rtdb.europe-west1.firebasedatabase.app/events/-Nucr-C5ANJidfw12Nba.json', {
        method: 'PATCH',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({ events: updatedEventsData }),
        });

        alert('Vi håber på at se dig til andre events!');
    } catch (error) {
        console.error('Fejl ved nej:', error.message);
        alert('Fejl ved fjernelse af nej: ' + error.message);
    }
};
</script>

<template>
  <div id="main-section">
    <p v-if="isLoading">Loading...</p>
    <div v-else>
      <ul>
        <li v-for="e in events" :key="e.id">
          <div class="newevent">
            <div class="text-overlay">
              <h3>{{ e.title }}</h3>
              <p>{{ e.date }}</p>
              <div class="deltager">
                <h4>Deltager?</h4>
                <button @click="attendEvent(e.id)" class="buttonja">JA</button>
                <button @click="noToEvent(e.id)" class="buttonnej">NEJ</button>
              </div>
            </div>
              <img :src="e.billede" class="newimg" alt="Event image" />
              
          </div>
          
        </li>
      </ul>
    </div>
  </div>
</template>
