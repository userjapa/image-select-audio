<template lang="html">
  <div class="game">
    <div class="game__question">
      <div class="game__question__image">
        <img :src="question.img"/>
      </div>
    </div>
    <div class="game__answers-item">playing
      <div class="game__answers-item__audio"
           v-for="(aswr, key) in question.answers">
           <!-- @click="selectAnswer(aswr)"> -->
           <div class="game__answers-item__audio__player">
             <audio :src="aswr.audio"
               :ref="`audio-aswr-${key}`"
               @play="$set(playing, key, true)"
               @pause="$set(playing, key, false)"/>
             <button type="button"
                     class="game__answers-item__audio__player__btn"
                     @click="playing[key]?$refs[`audio-aswr-${key}`][0].pause():$refs[`audio-aswr-${key}`][0].play()">
                  <!-- <i v-if="!playing[key]">Play</i>
                  <i v-else>Pause</i> -->
                  <font-awesome-icon icon="play" v-if="!playing[key]"/>
                  <font-awesome-icon icon="pause" v-else />
             </button>
             <input class="game__answers-item__audio__player__progress" type="range"/>
             <!-- <div class="game__answers-item__audio__player__status">
               <img :src="require('@/assets/img/correct-icon.png')">
             </div> -->
           </div>
           <div class="game__answers-item__audio__select" @click="selectAnswer(aswr)"
                v-if="!question.answered || (!aswr.selected && !aswr.correct)">?</div>
           <img class="game__answers-item__audio__select"
                :src="require('@/assets/img/correct-icon.png')"
                v-if="aswr.correct && question.answered">
                <!-- :style="{ visibility: (!question.answered)?'hidden':'' }" -->
           <img class="game__answers-item__audio__select"
                :src="require('@/assets/img/wrong-icon.png')"
                v-if ="!aswr.correct && aswr.selected && question.answered">
                <!-- :style="{ visibility: (!question.answered || !aswr.selected)?'hidden':''}" -->
      </div>
      <div class="game__answers-item__options">
        <button class="game__answers-item__options__btn"
                :disabled="!question.answered"
                @click="setCurrent(exercise.questions)"
                v-if="!ended">
          Next
        </button>
        <button class="game__answers-item__options__btn" v-else>Finish</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'ListenImage',
  data () {
    return {
      playing: [],
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
  flex-direction: row;
  height: 100%;
  max-height: 91%;
  &__question {
    flex-basis: 50%;
    border: 2px solid blue;
    padding: 1rem 2rem;
    &__image {
      width: 100%;
      max-height: 100%;
      height: auto;
      & img {
        height: auto;
        max-width: 100%;
        max-height: 100%;
      }
    }
  }
  &__answers-item {
    width: 50%;
    border: 2px solid blue;
    padding: 1rem 2rem;
    display: flex;
    justify-content: flex-start;
    align-content: center;
    flex-direction: column;
    position: relative;
    &__audio {
      border-radius: 2.5rem;
      background-size: cover;
      align-self: center;
      width: 100%;
      display: flex;
      justify-content: center;
      &:first-child {
        padding-top: 5px;
      }
      &:not(:last-child) {
        padding-bottom: 10px;
      }
      &__player {
        border: 2px solid #ddd;
        margin: auto 0;
        // margin-left: 7.5%;
        margin-right: 10px;
        display: flex;
        justify-content: space-between;
        height: 2.5em;
        padding: 5px;
        border-radius: 100px;
        &__btn {
          padding: 10px 10px;
          border-radius: 100px;
          font-size: 20px;
          text-align: center;
        }
        &__progress {
          width: 65%;
          margin-right: 15px;
          &:active,
          &:focus {
            outline: none
          }
        }
        &__status {
          height: 100%;
          & img {
            height: 100%;
          }
        }
      }
      &__select {
        background-color: #ddd;
        width: 54px;
        height: 54px;
        border: 2px;
        border-radius: 150px;
        font-size: 30px;
        font-weight: 600;
        text-align: center;
        vertical-align: middle;
        line-height: 55px;
      }
    }
    &__options {
      position: absolute;
      top: calc(100% - 57px);
      left: calc(100% - 143px);
      border: 2px solid blue;
      width: 101px;
      height: 35px;
      padding: 1rem 2rem;
      display: flex;
      flex-direction: column;
      justify-content: flex-end;
      &__btn {
        align-self: flex-end;
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
}
</style>
