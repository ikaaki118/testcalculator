<template>
  <div class="container">
    <h1 class="title is-1">testcalc</h1>

        <ul>
          <li v-for="h in histry" :key="k" class="button is-link is-light" @click="click_number(k)">
              {{h}}
          </li>
        </ul>
        <br>
        numberA:{{numberA}}<br>
        numberB:{{numberB}}<br>
        result:{{result}}<br>
        symbol:{{symbol}}<br>
        calculating:{{calculating}}


    <div class="field">
      <div class="control">
        <input class="input" type="text" placeholder="数値を入力"  v-model="box">
      </div>
    </div>


    <div class="field is-grouped">
      <div class="control">
        <!-- 数字キーの羅列 -->
        <ul>
          <li v-for="k in keypad" :key="k" class="button is-link is-light" @click="click_number(k)">
              {{k}}
          </li>
        </ul>
        <!-- 数字キーの羅列ココマデ -->
        <!--四則演算キーの羅列-->
        <ul>
          <li v-for="k in symbolpad" :key="k" class="button is-link is-light" @click="click_symbol(k)">
              {{k}}
          </li>
        </ul>
        <!--四則演算キーの羅列ココマデ-->
        <button class="button is-link is-light"  @click="click_equal()">=</button>
        <button class="button is-link is-light" @click="clear()">クリア</button>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  data () {
    return {

      box: null,
      numberA: null,
      numberB: null,
      result: null,
      symbol: null,
      calculating: false,
      histry: [],

      //↓定数
      keypad: [1,2,3,4,5,6,7,8,9,0],
      symbolpad: ["＋","－","×","÷",]
    }
},

    methods: {

    click_number(n) {
      //"計算中でない" or "数字が未入力" or "数字が0" or "既に計算結果がある" なら数値を代入
      //数字の前に空白を入れることで文字列として認識される
      if(this.calculating == false ||  this.box == null || this.box == 0 || this.result != null){
        this.result = null
        this.box = "" + n
        this.calculating = true
        //"計算処理中" かつ "数字が入力済み" かつ "数字が0でない"なら、今ある数字の後ろに数値を追加する
      }else{
        this.box += "" + n
      }
    },

    click_symbol(s) {
      //"既に数値が登録されている" かつ "記号がクリックされている" かつ "別の記号がクリックされた"ときは記号の登録を変更する
      if(this.numberA != null && this.symbol != s && this.symbol != null && this.box ==null){
        this.symbol = s
      }
      else if(this.numberA == null && this.box ==null){
      }
      else{
        this.register(s)
        this.calculating = true
        this.box = this.result
        this.register(s)
      }
    },

    click_equal(){
      //登録された数字があり、boxが空白でなく、計算が進行中なら、入力されている数字で計算をする
      if(this.numberA != null && this.box != null && this.calculating){
        this.register()
      }
    },

    register (s) {
      //numberAが空でかつboxが空でないとき、boxの中身をAに代入
      if((this.numberA == null) && (this.box != null)){
        this.numberA = parseFloat(this.box)
        this.histry.push(this.box)

        this.box= null
        this.result = null
        this.symbol = s
        this.calculating = true
      }
      else if(this.box != null) {
        this.numberB = parseFloat(this.box)
        this.culculate()
        this.symbol = s
      }

    },

    culculate() {
      switch(this.symbol) {
        case "＋" :
          this.result = this.numberA + this.numberB
        break;

        case "－" :
          this.result = this.numberA - this.numberB
        break;

        case "×" :
          this.result = this.numberA * this.numberB
        break;

        case "÷" :
          this.result = this.numberA / this.numberB
        break;
      }
      this.numberA = null
      this.numberB = null
      this.box = this.result
      this.calculating = false
      },

    clear() {
      this.box = null
      this.numberA = null
      this.numberB = null
      this.result = null
      this.symbol = null
      this.calculating = false
      this.histry = []
    }
  }

}
</script>