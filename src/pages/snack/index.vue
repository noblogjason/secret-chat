<template>
  <div class='snack-panel'>
    <div class='control-panel'></div>
    <div class='game-panel' :style="{width: sidePix, height: sidePix}">

    </div>
  </div>
</template>
<script>
class Pos {
  static randomPos = (m, n = 0) => {
    return new Pos(Math.floor(Math.random()*(m-n+1)+n), Math.floor(Math.random()*(m-n+1)+n))
  }
  constructor(x = 0, y = 0) {
    this.x = x
    this.y = y
  }
}
export default {
  name: 'index',
  data() {
    return {
      cellPix: '15',
      cellNumber: 18,
      food: [Pos.randomPos(18)]
    }
  },
  mounted() {
    console.log(this.food)
  },
  computed: {
    sidePix() {
      return `${this.cellPix * this.cellNumber}px`
    }
  },
  methods: {
   
  }

};
</script>
<style lang="scss" scoped>
  .game-panel {
    border: 1px solid black
  }
</style>
