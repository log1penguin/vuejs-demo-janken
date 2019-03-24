<template>
  <div id="app">
    <!--タイトル-->
    <h1>じゃんけんゲーム！</h1>
    <!--結果の表示-->
    <h2>結果は…{{ resultMessage }}！</h2>
    <!--もう一度ボタン-->
    <button class="button button-retry" v-on:click="start" disabled>もういちど</button>
    
    <!--敵の手-->
    <div class="imgArea">
      <img v-bind:src="src">
    </div>
    
    <!--自分の手-->
    <ul>
      <li><button class="button button-myHand" type="button" v-on:click="onSelected" value="0">ぐー</button></li>
      <li><button class="button button-myHand" type="button" v-on:click="onSelected" value="1">ちょき</button></li>
      <li><button class="button button-myHand" type="button" v-on:click="onSelected" value="2">ぱぁ</button></li>
    </ul>
    
  </div>
</template>

<script>
export default {
  name: 'app',
  // アプリケーションで使用するデータ
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
  // 初期化が終わったらこれを実行
  created: function(){
    this.start()
  },
  // 使用するメソッド
  methods: {
    // 敵の手を切り替える
    changeImg() {
      // 0以上1未満の数値*3の数以下の最大の整数
      var num = Math.floor(Math.random() * this.imgList.length)
      this.src = this.imgList[num]
    },
    start() {
      this.reset()
      this.timer = setInterval(() => {
        this.changeImg()
      }, (1000/10))
    },
    // 自分の手ボタンが押されたら走るメソッド
    onSelected(e) {
      // 繰り返し動作のキャンセル
      clearInterval(this.timer)
      
      // イベントを発生させたオブジェクトの参照
      let button = e.target
      let enemyHand = parseInt(this.imgList.indexOf(this.src), 10) 
      let myHand = parseInt(button.value, 10)
      this.resultJanken(myHand, enemyHand)
      
      // ボタン押せないようにしとく
      var btns = document.querySelectorAll('.button-myHand')
      for(let btn of btns){
        btn.setAttribute('disabled', true)
      }
      // リトライボタンを押せるようにする
      var retryBtn = document.querySelector('.button-retry')
      retryBtn.removeAttribute('disabled')
    },
    // 勝敗のメッセージを決めるメソッド
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
    // ボタンを再び押せるようにして、勝敗のメッセージ初期化
    reset() {
      var myHandBtns = document.querySelectorAll('.button-myHand')
      for(let btn of myHandBtns) {
        btn.removeAttribute('disabled')
        btn.classList.remove('is-selected')
      }
      
      var retryBtn = document.querySelector('.button-retry')
      if(retryBtn){
        retryBtn.setAttribute('disabled', true)
      }
      
      this.resultMessage = ''
    }

  }
}
</script>

<style scoped>
  h1,h2 {
    text-align: center;
  }
  .imgArea {
    width: 430px;
    height: 500px;
    margin: 20px auto;
  }
  ul {
    text-align: center;
    padding: 0;
  }
  li {
    list-style: none;
    display: inline-block;
  }
  .button {
    border: none;
    cursor: pointer;
    outline: none;
    padding: 0;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    width: 200px;
    line-height: 50px;
    background-color: #15aaa1;
    color: #fff;
    font-size: 1.5em;
    display: block;
    margin: 0 auto;
  }
  .button:disabled {
    background-color: #bbb;
  }
</style>