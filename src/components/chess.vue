<template>
  <div class="chess">
    <template v-for="(p,y) in pieces.len">
      <template v-for="(p,x) in pieces.len">
        <i :class="pieces[_cover(x)+_cover(y)]" @click="tick(x,y)"></i>
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
        const center = Math.floor(this.boardWidth / 2)
        let pieces = {
          len: this.boardWidth
        }
        // 初始化棋盘坐标
        for (let x = 0; x < pieces.len; x++) {
          for (let y = 0; y < pieces.len; y++) {
            let player = ''
            // 设定中间棋子为黑棋
            if (x === center && y === center) player = this.pieceClass.b
            pieces[this._cover(x) + this._cover(y)] = player
          }
        }
        this.pieces = pieces
      },
      _cover (m) {
        return m < 10 ? `0${m}` : '' + m
      },
      tick (x, y) {
        const coordinate = this._cover(x) + this._cover(y)
        if (!this.pieces[coordinate] && !this.gameOver) {
          this.pieces[coordinate] = this.player ? this.pieceClass.b : this.pieceClass.w
          this.last = coordinate

          // 如果尚未决出胜负，换手
          if (!this.check(x, y)) {
            this.player = !this.player
          }
        }
      },

      // 判断胜负关系
      check (x, y) {
        const cx = this._cover(x)
        const cy = this._cover(y)
        // 记录当前子的颜色
        const curPiece = this.pieces[cx + cy]
        let count = 0

        // 横向
        for (let i = 0; i < this.boardWidth; i++) {
          if (this.pieces[this._cover(i) + cy] === curPiece) {
            count++
          } else {
            count = 0
          }
          if (count === 5) {
            if (curPiece === 'b') {
              console.log('黑子赢')
            } else {
              console.log('白子赢')
            }
            this.gameOver = true
            return false
          }
        }

        // 纵向
        for (let i = 0; i < this.boardWidth; i++) {
          if (this.pieces[cx + this._cover(i)] === curPiece) {
            count++
          } else {
            count = 0
          }
          if (count === 5) {
            if (curPiece === 'b') {
              console.log('黑子赢')
            } else {
              console.log('白子赢')
            }
            this.gameOver = true
            return false
          }
        }

        //  \方向
        let sub = Math.min(x, y)
        sub = sub > 5 ? 5 : sub
        console.log(sub)
        let _x = x - sub
        let _y = y - sub
        for (let i = 0; i < this.boardWidth - Math.abs(x - y); i++) {
          if (this.pieces[this._cover(_x) + this._cover(_y)] === curPiece) {
            count++
          } else {
            count = 0
          }
          _x++
          _y++
          if (count === 5) {
            if (curPiece === 'b') {
              console.log('黑子赢')
            } else {
              console.log('白子赢')
            }
            this.gameOver = true
            return false
          }
        }

        // /方向：想象成\方向的镜像
        _x = this.boardWidth - x - 1
        _y = y
        sub = Math.min(_x, _y)
        sub = sub > 5 ? 5 : sub
        _y = _y - sub
        _x = _x - sub
        for (let i = 0; i < this.boardWidth - Math.abs(_x - _y); i++) {
          if (this.pieces[this._cover(_y)] + this._cover(this.boardWidth - _x - 1)) {
            count++
          } else {
            count = 0
          }
          if (count === 5) {
            if (curPiece === 'b') {
              console.log('黑子赢')
            } else {
              console.log('白子赢')
            }
            this.gameOver = true
            return false
          }
        }
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
    background-color: #ccc;
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

  /*黑棋*/
  i.b {
    background-color: #111;
    border-radius: 50%;
  }

  /*白棋*/
  i.w {
    background-color: #fff;
    border-radius: 50%;
  }
  i.w:after, i.w:before {
    content: "";
    display: none;;
  }
</style>
