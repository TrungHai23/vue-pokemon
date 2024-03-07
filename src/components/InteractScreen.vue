<template>
  <div class="screen">
    <h1>Interact Screen here ...</h1>
    <CardFlip
      v-for="(card, index) in cardsContext"
      :key="index"
      :ref="`card-${index}`"
      :imgBackFaceUrl="`images/${card}.png`"
      :card="{ index: index, value: card }"
      @onFlip="checkRule($event)"
    ></CardFlip>
  </div>
</template>

<script>
import CardFlip from "./CardFlip.vue";

export default {
  data() {
    return {
      rules: [],
    };
  },
  components: {
    CardFlip,
  },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        console.log("Right...");
        if (
          this.$refs[`card-${this.rules[0].index}`] &&
          this.$refs[`card-${this.rules[1].index}`]
        ) {
          this.$refs[`card-${this.rules[0].index}`][0].onEnabledDisableMode();
          this.$refs[`card-${this.rules[1].index}`][0].onEnabledDisableMode();
          this.rules = [];

          const disabledElement = document.querySelectorAll(
            ".screen .card.disabled"
          );
          console.log(disabledElement);
          if (
            disabledElement &&
            disabledElement.length === this.cardsContext.length - 2
          ) {
            setTimeout(() => {
              this.$emit("onFinish");
            }, 1000);
          }
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        console.log("Wrong...");
        console.log(this.$refs);
        // close two card
        setTimeout(() => {
          if (
            this.$refs[`card-${this.rules[0].index}`] &&
            this.$refs[`card-${this.rules[1].index}`]
          ) {
            this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard();
            this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard();
          }
          // reset rules to []
          this.rules = [];
        }, 800);
      } else return false;
    },
  },
};
</script>
