<template>
  <div class="home">
    <span
      v-for="(letter, index) in letters.split('')"
      :key="letter + Math.random()"
      :class="{ highLight: letters[index] === typed[index] }"
      >{{ letter }}
    </span>
    <br />
    <input type="text" v-model="currentWord" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      letters:
        "In order to return the power to repel evil to your sword, you must find another to take my stead in this temple and ask the gods for their assistance",
      typedSentence: "",
      currentWord: "",
      currentIndex: 0,
      wordIndex: 0
    };
  },
  computed: {
    typed() {
      return this.typedSentence + this.currentWord;
    },
  },
  watch: {
    currentWord() {
      if (this.currentWord[this.currentWord.length - 1] === " ") {
        // console.log("Current index = ", this.currentIndex);
        // console.log("Current word length = ", this.currentWord.length);
        console.log(
          "Sub String = ",
          this.letters.substring(
            this.currentIndex,
            this.currentWord.length - 1 + this.currentIndex
          )
        );
        console.log("Current word = ", this.currentWord);
        if (
          // this.letters.substring(
          //   this.currentIndex,
          //   this.currentWord.length - 1 + this.currentIndex
          // ) === this.currentWord.trim())
          this.letters.split(" ")[this.wordIndex] === this.currentWord.trim())
           {
          this.typedSentence += this.currentWord;
          this.currentIndex += this.currentWord.length;
          this.currentWord = "";
          this.wordIndex += 1;
        }
        // console.log("Typed sentence = ", this.typedSentence);
        // console.log("Current index = ", this.currentIndex);
      }
    },
  },
};
</script>

<style scoped>
.home {
  max-width: 40rem;
  margin: 0 auto;
}
.highLight {
  color: red;
}
</style>
