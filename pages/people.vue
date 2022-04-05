<template>
<v-container>
  <div id="app">
    <Person :person="person" v-for="person in people" :key="person.id"></Person>
  </div>
  <div>
      <button @click="addPerson()"> Add Person</button>
  </div>
</v-container>
</template>

<script>
import Person from "@/components/Person";

export default {
  name: "App",
  components: {
    Person,
  },
  data() {
    return {
      people: [
        { id: 0, name: "Bob", age: 27 },
        { id: 1, name: "Frank", age: 32 },
        { id: 2, name: "Joe", age: 38 },
      ],
    };
  },
  methods: {
      addPerson() {
          this.people.push({id:3, name:"Alberto", age: 25})
      }
  }
};
</script>