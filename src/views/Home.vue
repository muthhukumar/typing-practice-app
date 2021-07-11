<template>
  <div class="home">
    <span v-if="typedSentence" class="typedSentence">{{ typedSentence }}</span>
    <span v-if="typedLetters" class="typedLetters">{{ typedLetters }} </span>
    <span v-if="currentLetter" class="currentLetter">{{ currentLetter }}</span>
    <span v-if="unTypedLetters" class="unTypedLetters">{{
      unTypedLetters
    }}</span>
    <span v-if="unTypedSentence.length > 0" class="unTypedSentence">{{
      unTypedSentence.join(" ")
    }}</span>
    <br />
    <input type="text" v-model="currentWord" />
  </div>
</template>

<script>
export default {
  created() {
    const result = this.parseSentence(
      "In order to return the power to repel evil to your sword".split(/(\s+)/)
    );

    console.log(result);

    this.unTypedSentence = result.unTypedSentence;
    this.unTypedLetters = result.unTypedLetters;
    this.currentLetter = result.currentLetter;
  },
  methods: {
    parseSentence(words) {
      const result = {};

      const firstWord = words.splice(0, 1)[0];

      if (firstWord === " ") {
        result.currentLetter = " ";
        result.unTypedLetters = null;
        result.unTypedSentence = words;

        return result;
      }

      result.currentLetter = firstWord[0];
      result.unTypedLetters = firstWord.substring(1, firstWord.length);
      result.unTypedSentence = words;

      return result;
    },
    setValues({
      typedSentence,
      typedLetters,
      currentLetter,
      unTypedLetters,
      unTypedSentence,
    }) {
      if (this.isValidValue(typedSentence)) this.typedSentence = typedSentence;
      if (this.isValidValue(typedLetters)) this.typedLetters = typedLetters;
      if (this.isValidValue(currentLetter)) this.currentLetter = currentLetter;
      if (this.isValidValue(unTypedLetters))
        this.unTypedLetters = unTypedLetters;
      if (this.isValidValue(unTypedSentence))
        this.unTypedSentence = unTypedSentence;
    },
    isValidValue(value) {
      return value === "" || value || value === null;
    },
  },

  computed: {},
  watch: {
    currentWord(newValue) {
      if (newValue[this.index] === this.currentLetter) {
        if (newValue[this.index] === " ") {
          const {
            currentLetter,
            unTypedLetters,
            unTypedSentence,
          } = this.parseSentence(this.unTypedSentence);

          const typedSentence = (this.typedSentence += this.typedLetters + " ");

          this.setValues({
            currentLetter,
            unTypedLetters,
            unTypedSentence,
            typedSentence,
            typedLetters: "",
          });

          this.currentWord = "";
          this.index = 0;

          return;
        }

        this.typedLetters = newValue;

        this.index++;

        if (this.unTypedLetters.length === 1) {
          this.setValues({
            currentLetter: this.unTypedLetters[0],
            unTypedLetters: "",
          });
          return;
        }

        if (this.unTypedLetters.length === 0) {
          if (this.unTypedSentence.length === 0) {
            this.setValues({
              typedSentence: (this.typedSentence += newValue),
              typedLetters: null,
              currentLetter: null,
              unTypedLetters: null,
              unTypedSentence: [],
            });

            return;
          }

          const {
            currentLetter,
            unTypedLetters,
            unTypedSentence,
          } = this.parseSentence(this.unTypedSentence);

          this.setValues({ currentLetter, unTypedLetters, unTypedSentence });

          return;
        }

        this.setValues({
          currentLetter: this.unTypedLetters[0],
          unTypedLetters: this.unTypedLetters.substring(
            1,
            this.unTypedLetters.length
          ),
        });
      }
    },
  },
  data() {
    return {
      typedSentence: "",
      typedLetters: "",
      currentLetter: "",
      unTypedLetters: "",
      unTypedSentence: [],
      index: 0,
      currentWord: "",
    };
  },
};
</script>

<style scoped>
.home {
  max-width: 40rem;
  font-size: 1.3rem;
  margin: 0 auto;
  font-weight: 500;
}
.typedLetters .typedLetters,
.typedSentence {
  color: red;
}

.currentLetter,
.typedLetters,
.unTypedLetters {
  text-decoration: underline;
}
</style>
