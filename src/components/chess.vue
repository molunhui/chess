<template>
  <div class="chess">
    <template v-for="(p,y) in pieces.len">
      <template v-for="(p,x) in pieces.len">
        <i class="" @click="tick(x,y)"></i>
        <br v-if="x == pieces.len-1">
      </template>
    </template>
  </div>
</template>

<script>
  export default {
    name: 'chess',
    data () {
      return {
        gameOver: false,  // 游戏结束
        pieces: {},       // 棋
        boardWidth: 11,    // 棋盘大小
        last: '',         // 上一步
        pieceClass: { b: 'b', w: 'w' },
        player: 0 // 1:player1 class==b  0:player2 lass==w
      }
    },
    created () {
      this.init()
    },

    methods: {
      // 初始化棋盘
      init () {
        this.gameOver = false
        // const center = Math.floor(this.boardWidth / 2)
        let pieces = {
          len: this.boardWidth
        }
        // 初始化棋盘坐标
        // for (let x = 0; x < pieces.len; x++) {
        //   for (let y = 0; y < pieces.len; y++) {
        //     let player = ''
        //     // 设定中间棋子为黑棋
        //     if (x === center && y === center) player = this.pieceClass.b
        //     pieces[this._cover(x) + this._cover(y)] = player
        //   }
        // }
        this.pieces = pieces
      },
      tick (x, y) {
        console.log(x, y)
      }
    }
  }
</script>

<style scoped>
  .chess {
    width: 440px;
    height: 440px;
    margin: 30px auto;
    font-size: 0;
  }
  i {
    position: relative;
    display: inline-block;
    width: 40px;
    height: 40px;
  }
  i:before {
    content: '';
    position: absolute;
    width: 1px;
    height: 40px;
    left: 50%;
    top: 0;
    background-color: #111;
  }
  i:after {
    content: '';
    position: absolute;
    width: 40px;
    height: 1px;
    top: 50%;
    left: 0;
    background-color: #111;
  }
</style>
