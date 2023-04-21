<template>
  <div class="complete-test__timer" v-if="isMounted">
    Звоните скорее, запись доступна всего <span>{{ timer }}</span
    >МИНУТ
  </div>
</template>

<script>
export default {
  data() {
    return {
      isMounted: false,
      timer: '10:00',
      timerFinished: false
    }
  },
  mounted() {
    this.isMounted = true // устанавливаем флаг, когда компонент отображается на экране
  },
  created() {
    this.startTimerComplete()
  },
  methods: {
    startTimerComplete() {
      let minutes = 10
      let seconds = 0
      let interval = setInterval(() => {
        if (minutes === 0 && seconds === 0) {
          clearInterval(interval)
          this.$emit('timer-finished')
        } else {
          if (seconds === 0) {
            minutes--
            seconds = 59
          } else {
            seconds--
          }
          this.timer =
            (minutes < 10 ? '0' + minutes : minutes) +
            ':' +
            (seconds < 10 ? '0' + seconds : seconds)
        }
      }, 1000)
    }
  }
}
</script>

<style scoped>
.complete-test__timer {
  width: 240px;
  font-weight: 400;
  font-size: 11px;
  line-height: 30px;
  text-align: center;
  letter-spacing: 0.1em;
  color: #3bde7c;
  margin-bottom: 5px;
}
.complete-test__timer span {
  font-size: 20px;
  margin-right: 10px;
}
</style>
