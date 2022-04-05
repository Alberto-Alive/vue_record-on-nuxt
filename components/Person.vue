<template>
  <div class="hello">
    <div class="person">
      {{ p.name }}
      <input type="text" v-model="p.age" />
    </div>
  </div>
</template>

<script>
export default {
  name: "Person",
  props: {
    person: Object,
  },
  data() {
    return {
      p: {},
    };
  },
  watch: {
    p: {
      handler(newValue) {
        console.log(newValue.id);
        console.log(newValue.age);
      },
      deep: true,
    },
  },
  mounted() {
    this.p = { ...this.person };
  },
};
</script>