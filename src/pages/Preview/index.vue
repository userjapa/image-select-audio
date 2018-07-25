<template lang="html">
  <ListenSelectAudio :exercise="exercise"/>
</template>

<script>
import ListenSelectAudio from '@/components/ListenSelectAudio'

export default {
  name: 'Preview',
  computed: {
    exercise () {
      return this.$store.getters['getExercise']
    }
  },
  components: {
    ListenSelectAudio
  }
}
</script>

<style lang="scss">
</style>
