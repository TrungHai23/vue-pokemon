<template>
  <div>
    <h1>Heloo</h1>
    <MainScreen
      v-if="statusMatch === 'default'"
      @onStart="onHandleBeforeStar($event)"
    ></MainScreen>
    <InteractScreen
      v-if="statusMatch === 'match'"
      :cardsContext="settings.cardsContext"
      @onFinish="onGetResult"
    ></InteractScreen>
    <ResultScreen
      v-if="statusMatch === 'result'"
      :timer="timer"
      @onStartAgain="statusMatch = 'default'"
    ></ResultScreen>
  </div>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffled } from "./assets/utils/array";
export default {
  name: "App",
  data() {
    return {
      settings: {
        totalOfBlock: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  methods: {
    onHandleBeforeStar(config) {
      console.log("running handle befoer start", config);
      this.settings.totalOfBlock = config.totalOfBlock;
      const firstCards = Array.from(
        { length: this.settings.totalOfBlock / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      console.log(cards);
      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      );
      console.log(this.settings.cardsContext);
      this.settings.startedAt = new Date().getTime();
      // data ready
      this.statusMatch = "match";
    },
    onGetResult() {
      // get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      //switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>

<style></style>
