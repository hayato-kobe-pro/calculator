<template>
  <div class="container_inputResult">
    <div class="formula">
      <p>{{ viewFormula }}</p>
    </div>
    <div class="result">
      <p>{{ result }}</p>
    </div>
  </div>
  <div class="container_btn">
    <input type="button" value="(" @click="checkButton('(')" class="btn symbol_btn">
    <input type="button" value=")" @click="checkButton(')')" class="btn symbol_btn">
    <input type="button" value="%" @click="checkButton('%')" class="btn symbol_btn">
    <input type="button" value="CE" @click="checkButton('CE')" class="btn symbol_btn CE_btn" v-if="showCEButton">
    <input type="button" value="AC" @click="checkButton('AC')" class="btn symbol_btn AC_btn" v-else>
    <input type="button" value=7 @click="checkButton(7)" class="btn num_btn">
    <input type="button" value=8 @click="checkButton(8)" class="btn num_btn">
    <input type="button" value=9 @click="checkButton(9)" class="btn num_btn">
    <input type="button" value="÷" @click="checkButton('÷')" class="btn symbol_btn">
    <input type="button" value=4 @click="checkButton(4)" class="btn num_btn">
    <input type="button" value=5 @click="checkButton(5)" class="btn num_btn">
    <input type="button" value=6 @click="checkButton(6)" class="btn num_btn">
    <input type="button" value="×" @click="checkButton('×')" class="btn symbol_btn">
    <input type="button" value=1 @click="checkButton(1)" class="btn num_btn">
    <input type="button" value=2 @click="checkButton(2)" class="btn num_btn">
    <input type="button" value=3 @click="checkButton(3)" class="btn num_btn">
    <input type="button" value="-" @click="checkButton('-')" class="btn symbol_btn">
    <input type="button" value=0 @click="checkButton(0)" class="btn num_btn">
    <input type="button" value="." @click="checkButton('.')" class="btn num_btn">
    <input type="button" value="=" @click="checkButton('=')" class="btn equal_btn">
    <input type="button" value="+" @click="checkButton('+')" class="btn symbol_btn">
  </div>
</template>

<script>
export default {
  data() {
    return {
      showCEButton: true,
      viewFormula: null,
      formulaArray: [],
      calculateArray: [],
      MudAndDivCounter: 0,
      error: '',
      result: null
    };
  },
  methods: {
    checkButton(value){
      switch(value){
        case '=':
          this.calculate();
          break;
        case 'CE':
          // this.formulaArray.pop();
          break;
        case 'AC':
          this.viewFormula = null;
          this.formulaArray = [];
          this.result = null;
          this.showCEButton = !this.showCEButton;
          break;
        default :
          this.updateFormula(value);
          this.updateCounter(value);
      }
    },
    updateFormula(checkedValue) {
      
      if (this.formulaArray[this.formulaArray.length - 1] === '%' && typeof checkedValue === "number") {
        this.formulaArray.push('×');
        this.formulaArray.push(checkedValue);
        this.MudAndDivCounter += 1;
        console.log(1);
      }
      else if(this.result && typeof checkedValue === "number") {
        // resultがある状態でnumを入力するとformulaArrayとともにリセット
        this.formulaArray[0] = checkedValue;
        this.result = null;
        console.log(2);        
      }
      else if(typeof checkedValue === "number" 
      && typeof this.formulaArray[this.formulaArray.length - 1] === "number"){
        // 数字が連続で入力された時の計算
        this.formulaArray[this.formulaArray.length - 1] = this.formulaArray[this.formulaArray.length - 1] * 10 + checkedValue;
        console.log(3);
      }
      else if((this.formulaArray.length !== 0 
      || typeof checkedValue === "number") 
      && (typeof checkedValue === "number" 
      || typeof this.formulaArray[this.formulaArray.length - 1] === "number")){
        // 数字や演算子を追加（通常処理）
        this.formulaArray.push(checkedValue);
        this.result = null;
        console.log(4);
      }
      else if (this.formulaArray[this.formulaArray.length - 1] === '%') {
        this.formulaArray.push(checkedValue);
      }
      else {
        // カウンターの数の調整
        this.updateCounter();
        // 演算子が連続で押された時に新しい入力に更新する。
        this.formulaArray[this.formulaArray.length - 1] = checkedValue;
        console.log(5);
      }
      
      // viewFormulaのリセット
      this.viewFormula = '';
      
      // 表示(viewFomula)の更新
      for (let i = 0; i < this.formulaArray.length; i++) {
        // ここswitchに変更して、*/を×と÷にしたい。
        if(typeof this.formulaArray[i] === "number" || (this.formulaArray[i] === '.') || (this.formulaArray[i] === '%')) {
          this.viewFormula += this.formulaArray[i].toString();
          continue;
        } else if (i !== 0) {
          this.viewFormula += ' ' + this.formulaArray[i] + ' ';
        }
      }
    },
    updateCounter(checkedValue){
      // 乗除計算のカウント
      if (checkedValue === '×' || checkedValue === '÷' || checkedValue === '%'){
        if(this.formulaArray[this.formulaArray.length - 1] !== '×' || this.formulaArray[this.formulaArray.length - 1] !== '÷' || this.formulaArray[this.formulaArray.length - 1] !== '%') {
          this.MudAndDivCounter += 1;
        }
      } else {
        if(this.formulaArray[this.formulaArray.length - 1] === '×' || this.formulaArray[this.formulaArray.length - 1] === '÷') {
          this.MudAndDivCounter -= 1;
        } 
      }
      console.log("counter :"  + this.MudAndDivCounter);
    },
    calculate() {
      console.log(this.formulaArray);
      // %の計算
      this.calcPercent();
      // 演算子の置換
      this.replaceOperators();

      // formulaArrayを用いて計算する。
      for (let i = 0; i < this.formulaArray.length; i++){

        if (typeof this.formulaArray[i] === "number" ){
          // 数字の時はスキップ
          continue;
        } else if(this.formulaArray[i] === '*' || this.formulaArray[i] === '/'){
          // *,/の時に乗除カウンターを減らす
          this.MudAndDivCounter -= 1;
        }
        
        // 乗除カウンターが0の時のみ加減計算も可能にする
        if (this.MudAndDivCounter === 0 || this.formulaArray[i] === '*' || this.formulaArray[i] === '/') {
          this.formulaArray[i] = this.calc(this.formulaArray[i - 1], this.formulaArray[i], this.formulaArray[i + 1], i);
          // 計算後にformulaArrayの配列を整理
          if (this.formulaArray.length > 1){
            this.formulaArray.splice(i + 1, 1);
            this.formulaArray.splice(i - 1, 1);
          }
        }

      }
      if (this.formulaArray.length === 1) {
        this.showCEButton = !this.showCEButton;
        // 小数点を丸める。一部の小数値をうまく丸められない誤差を調整。
        this.formulaArray[0] = Math.round(this.formulaArray[0] * 10000000000) / 10000000000;
        this.result = "= " + this.formulaArray[0];
        console.log("counter :"  + this.MudAndDivCounter);
        return this.result;
      } else {
        this.calculate();
      }
    },
    calc(beforeOperands, operator ,afterOperands, index){
      // 演算子で計算を切り替え
      switch (operator) {
        case '*':
          return beforeOperands * afterOperands;
        case '/':
          return beforeOperands / afterOperands;
        case '+':
          return beforeOperands + afterOperands;
        case '-':
          return beforeOperands - afterOperands;
        }
    },
    calcPercent(){
      for (let i = 0; i < this.formulaArray.length; i++){
        if(this.formulaArray[i] === '%'){
            this.formulaArray[i - 1] /= 100 ;
            // %を処理した要素を削除
            this.formulaArray.splice(i, 1);
            // iの数字を調整（削除した％の分）
            i -= 1;
            // MudAndDivCounterを減らす
            this.MudAndDivCounter -= 1;
          }
        }
    },
    replaceOperators(){
      // 演算子の置換
      for (let i = 0; i < this.formulaArray.length; i++){

        switch (this.formulaArray[i]) {
          case '×':
            this.formulaArray[i] = '*';
            continue;
          case '÷':
            this.formulaArray[i] = '/';
            continue;
          case '.':
            this.formulaArray[i - 1] =Number(this.formulaArray[i - 1] + '.' + this.formulaArray[i + 1]);
            this.formulaArray.splice(i, 2);
        }
      }
    }
  }
};
</script>

<style scoped>
  * {
    font-size: 24px;
    color: #000000;
    margin: 0;
  }

  .container_inputResult{
    height: 60px;
  }

  .container_inputResult,
  .container_btn {
    max-width: 500px;
    width: 80%;
    margin: 60px auto;
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    justify-content: left;
  }

  .btn {
    width: calc((100% - 24px) / 4);
    height: auto;
    font-size: 24px;
    line-height: 48px;
    border-radius: 8px;
    border: none;
    font-family: Arial, Helvetica, sans-serif;
  }

  .btn.symbol_btn {
    background: #DDDDDD;
  }
  .btn.num_btn {
    background: #EEEEEE;
  }
  .btn.equal_btn {
    background: blue;
    color: #FFFFFF;
  }

  .btn.hide_btn {
    display: none;
  }
</style>