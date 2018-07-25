<template lang="html">
  <div class="game">
    <div class="game__question">
      <div class="game__question__image">
        <img :src="question.image"/>
      </div>
    </div>
    <div class="game__answers">
      <div class="game__answers__audio"
           v-for="aswr in question.answers"
           :class="{
              right: aswr.correct && question.answered,
              wrong: !aswr.correct && aswr.selected && question.answered
           }"
           @click="selectAnswer(aswr)">
           <audio :src="aswr.audio"/>
           <button type="button">Play</button>
      </div>
    </div>
    <div class="game__options">
      <button class="game__options__btn"
              :disabled="!question.answered"
              @click="setCurrent(exercise.questions)"
              v-if="!ended">
                Next
      </button>
      <button class="game__options__btn" v-else>Finish</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ListenImage',
  data () {
    return {
      question: {
        answers: []
      },
      index: 0,
      ended: false
    }
  },
  props: {
    exercise: Object
  },
  methods: {
    setCurrent (questions) {
      for (const ind in questions) {
        this.index = ind
        if (!questions[ind].answered) {
          this.question = questions[ind]
          break
        } else if (ind == (questions.length - 1)) {
          this.question = questions[ind]
          this.ended = true
          break
        }
      }
    },
    selectAnswer (answer) {
      if (!this.question.answered) {
        this.$set(answer, 'selected', true)
        this.$set(this.question, 'answered', true)
      }
    }
  },
  watch: {
    'question.audio': function () {
      this.$refs['audio'].pause()
      this.$refs['audio'].load()
    },
    // 'exercise.questions': function (questions) {
    //   this.setCurrent(questions)
    // }
  },
  mounted () {
    this.setCurrent(this.exercise.questions)
  }
}
</script>

<style lang="scss">
.game {
  display: flex;
  flex-direction: column;
  height: 100%;
  max-height: 91%;
  &__question {
    border: 2px solid blue;
    padding: 1rem 2rem;
    display: grid;
    grid-template-rows: 5rem 5rem;
    &__audio {
      display: flex;
      justify-content: center;
      & i, input {
        margin: auto 1rem;
      }
      & i {
        background-color: gray;
        padding: .75rem 1.25rem;
        border-radius: 100px;
        &:hover {
          cursor: pointer;
        }
      }
      & input {
        background-color: gray;
        border: 2px solid black;
      }
    }
  }
  &__answers {
    border: 2px solid blue;
    padding: 1rem 2rem;
    display: flex;
    flex-wrap: wrap;
    height: 100%;
    &__image {
      border: .75rem solid white;
      flex-grow: 1;
      border-radius: 2.5rem;
      background-size: cover;
      width: auto;
      max-width: 50rem;
      min-width: 30rem;
      height: auto;
      max-height: 30rem;
      min-height: 20rem;
      &:not(:last-child) {
        margin-right: 2rem;
      }
      &.right {
        border-color: green;
      }
      &.wrong {
        border-color: red;
      }
    }
  }
  &__options {
    border: 2px solid blue;
    padding: 1rem 2rem;
    display: flex;
    justify-content: flex-end;
    &__btn {
      align-self: center;
      padding: 1rem 3rem;
      margin-right: 5px;
      border-radius: 100px;
      background-color: #3bbfd4;
      font-weight: 600;
      font-size: 1.4rem;
      color: #fff;
      letter-spacing: 1.5px;
      &:hover,
      &:focus {
        background-color: #28a1b4;
        cursor: pointer;
        outline: none;
      }
      &:disabled {
        filter: grayscale(90%);
      }
    }
  }
}
</style>
