<template>
  <div id="app">
    <h1>じゃんけんゲーム！</h1>
    <h2>{{ resultMessage }}</h2>
    <button v-on:click="start">もういちど</button>
    
    <div class="imgArea">
      <img v-bind:src="src">
    </div>
    <ul>
      <li><button class="button" type="button" v-on:click="onSelected" value="0">ぐー</button></li>
      <li><button class="button" type="button" v-on:click="onSelected" value="1">ちょき</button></li>
      <li><button class="button" type="button" v-on:click="onSelected" value="2">ぱぁ</button></li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'app',
  data: function() {
    return {
      src: 'dist/imgs/choki.png',
      imgList: [
        'dist/imgs/gu.png',
        'dist/imgs/choki.png',
        'dist/imgs/par.png'
        ],
        timer: null,
        resultMessage: ''
    }
  },
  created: function(){
    this.start()
  },
  methods: {
    changeImg(number) {
      if(number && Math.abs(number) <= this.imgList.length){
        this.src = this.imgList(number)
      } else {
        var num = Math.floor(Math.random() * this.imgList.length)
        this.src = this.imgList[num]
      }
    },
    start() {
      this.reset()
      this.timer = setInterval(() => {
        this.changeImg()
      }, (1000/100))
    },
    onSelected(e) {
      clearInterval(this.timer)
      
      let button = e.target
      let enemyHand = parseInt(this.imgList.indexOf(this.src), 10)
      let myHand = parseInt(button.value, 10)
      this.resultJanken(myHand, enemyHand)
      
      var btns = document.querySelectorAll('.button')
      for(let btn of btns){
        btn.setAttribute('disabled', true)
      }
    },
    resultJanken(myHand, enemyHand) {
      switch( (myHand-enemyHand+3)%3 ){
        case 0:
          this.resultMessage = 'あいこ'
          break
        case 1:
          this.resultMessage = 'まけ〜'
          break
        case 2:
          this.resultMessage = '勝ち！'
          break
      }
    },
    reset() {
      var btns = document.querySelectorAll('.button')
      for(let btn of btns) {
        btn.removeAttribute('disabled')
        btn.classList.remove('is-selected')
      }
      this.resultMessage = ''
    }

  }
}
</script>

<style scoped>
  h1,h2 {
    margin: 0 auto;
  }
  .imgArea {
    height: 500px;
  }
  li {
    list-style: none;
    display: inline-block;
    width: 200px;
    line-height: 70px;
    background-color: #15aaa1;
    color: #fff;
  }
</style>