<template>
  <div class='snack-panel'>
    <div class='control-panel'></div>
    <div class='game-panel' :style="{width: realSidePix, height: realSidePix}">
      <div class='snacks'>
        <div 
          v-for="item in snacks" 
          :key="`${item.x}&${item.y}`" 
          :style="{width: realCellPix, height: realCellPix, top: getRealPos(item.y), left: getRealPos(item.x)}"
          class="snack">
        </div>
      </div>
      <div class='foods'>
        <div class='food' :style="{width: realCellPix, height: realCellPix, top: getRealPos(food.y), left: getRealPos(food.x)}"></div>
      </div>
    </div>
  </div>
</template>
<script>
class Pos {
  static randomPos = (m, n = 0) => {
    return new Pos(Math.floor(Math.random()*(m-n)+n), Math.floor(Math.random()*(m-n)+n))
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
      cellPix: 25,
      cellNumber: 20,
      food: this.refreshFood(),
      snacks: [new Pos()],
      direction: 'right', // 方向
      speed: 300, // 速度
      timed: null, // 定时器
      state: 'wait',
    }
  },
  mounted() {
    console.log(this.food)
    document.addEventListener('keydown', this.handleKeyDown)
    this.state = 'start'
  },
  computed: {
    realSidePix() {
      return `${this.cellPix * this.cellNumber}px`
    },
    realCellPix() {
      return `${this.cellPix}px`
    }
  },
  watch: {
    state(v) {
      if (v === 'start') {
        this.gameStart()
      } else if (v === 'fail') {
        this.gameFail()
      }
    }
  },
  methods: {
    getRealPos(num) {
      return `${num * this.cellPix}px`
    },
    refreshFood() {
      const newPos = Pos.randomPos(18)
      return newPos
    },
    checkOutRigion(pos) { // 是否出去游戏区域
      return pos.x >= this.cellNumber || pos.y >= this.cellNumber
    },
    checkGetFood(pos) {
      return pos.x === this.food.x && pos.y === this.food.y
    },
    checkReturnDirection(pos) { // 是否撞击了自己
      return this.snacks.some( item => {
        return item.x === pos.x && item.y === pos.y
      })
    },
    gameStart() {
      this.timed = setInterval(this.move, this.speed)
      console.log('开始啦')
    },
    gameFail() {
      this.timed = null
      alert('gameover')
    },
    move() {
      const snackStart = this.snacks[0]
      let newPos
      switch(this.direction) {
        case 'right' :
          newPos = new Pos(snackStart.x+1, snackStart.y)
          break;
        case 'left' :
          newPos = new Pos(snackStart.x-1, snackStart.y)
          break;
        case 'top' :
          newPos = new Pos(snackStart.x, snackStart.y-1)
          break;
        case 'down' :
          newPos = new Pos(snackStart.x, snackStart.y+1)
          break;
      }
      if (this.checkReturnDirection(newPos)) {
        return 
      }
      if (this.checkOutRigion(newPos)) {
        this.state = 'fail'
      } else {
        this.snacks.unshift(newPos)
        if (this.checkGetFood(newPos)) {
          this.food = this.refreshFood()
        } else {
          this.snacks.pop()
        }
      }
    },
    handleKeyDown(e) {
      const { keyCode } = e
      switch (keyCode) {
        case 38:
          this.direction != 'down' && (this.direction = 'top')
          break
        case 40:
          this.direction != 'top' && (this.direction = 'down')
          break
        case 37:
          this.direction != 'right' && (this.direction = 'left')
          break
        case 39:
          this.direction != 'left' && (this.direction = 'right')
          break
      }
    }
  }
};
</script>
<style lang="scss" scoped>
  .game-panel {
    border: 1px solid black;
    position: relative;
  }
  .snack {
    background: gray;
    position: absolute;
  }
  .food {
    background: red;
    position: absolute;
  }
</style>
