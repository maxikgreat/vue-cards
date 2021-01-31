<template>
  <teleport to="#modal">
    <AddUpdateForm
      v-if="showForm"
      @close-modal="showForm = !showForm"
      @add-new-event="addEvent($event)"
      @update-event="updateEvent($event)"
      :currentEvent="currentEvent"
    />
  </teleport>

  <div class="options">
    <button @click="showPastEvents = !showPastEvents">Show past events</button>
    <button @click="setForm(null)">Show form</button>
  </div>
  <ul>
    <li v-for="event in orderedEvents" :key="event.id" @click="setForm(event.id)">
      <Event
        :event="event"
        :daysLeft="daysLeft(event)"
        :showPastEvents="showPastEvents"
        @remove-event="removeEvent($event)"
      />
    </li>
  </ul>
</template>

<script>
const events = [
  {
    id: 1,
    name: "Graduation",
    details: "wooohoo!!!",
    date: "2020-09-25",
    background: "#F34949",
  },
  {
    id: 2,
    name: "Holidays",
    details: "wooohoo!!!",
    date: "2021-12-30",
    background: "#f9f970",
  },
  {
    id: 3,
    name: "HolidayYYYY",
    details: "Get me outta here!",
    date: "2020-09-12",
    background: "#7AD3F0",
  },
  {
    id: 4,
    name: "Birthday",
    details: "My birthday party",
    date: "2020-10-02",
    background: "#F07AEC",
  },
  {
    id: 5,
    name: "Christmas",
    details: "ho ho ho",
    date: "2020-03-5",
    background: "#EB9A0F",
  },
  {
    id: 6,
    name: "Conference Talk",
    details: "dont flop",
    date: "2021-02-28",
    background: "#68EE94",
  },
  {
    id: 7,
    name: "Today party",
    details: "dont flop",
    date: "2021-01-31",
    background: "#990303",
  },
];

  import Event from '@/components/Event';
  import AddUpdateForm from '@/components/AddUpdateForm';

  export default {
    name: "App",
    components: {
      Event,
      AddUpdateForm
    },
    data() {
      return {
        events,
        showPastEvents: false,
        showForm: false,
        currentEvent: null,
      }
    },
    methods: {
      addEvent(event) {
        this.events.push({
          id: this.events.length + 1,
          ...event
        });
        this.showForm = false;
      },
      updateEvent(event) {
        const index = this.events.findIndex(ev => ev.id === event.id);
        this.events[index] = event;
        this.showForm = false;
        console.log(this.events);
      },
      removeEvent(id) {
        this.events = this.events.filter(event => event.id !== id);
      },
      setForm(id) {
        if (id) {
          this.currentEvent = this.events.find(event => event.id === id) || {};
        } else {
          this.currentEvent = null;
        }
        this.showForm = true;
      },
      daysLeft(event) {
        const time = Date.parse(event.date) - Date.now();
        return Math.ceil(time / (1000 * 3600 * 24));
      },
    },
    computed: {
      orderedEvents() {
        const ordered = this.events;
        return ordered.sort((a, b) => this.daysLeft(a) > this.daysLeft(b) ? 1 : -1);
      }
    }
  };
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    text-align: center;
    max-width: 600px;
    margin: 0 auto;
  }
  ul {
    padding: 0;
  }
  li {
    list-style: none;
    cursor: pointer;
  }
</style>
