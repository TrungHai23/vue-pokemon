<template>
  <div class="screen">
    <h1>Win</h1>
    <p>{{ Math.round(timer / 1000) }} second</p>
    <button @click="onStartAgain">Start Again</button>
  </div>
</template>

<script>
export default {
  props: {
    timer: {
      type: Number,
      required: true,
    },
  },
  methods: {
    onStartAgain() {
      this.$emit("onStartAgain");
    },
  },
};
</script>
