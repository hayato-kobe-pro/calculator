<template>
  <div class="container_inputResult">
    <div class="formula">
      <p>{{ formula }}</p>
    </div>
    <div class="result">
      <p>{{ result }}</p>
    </div>
  </div>
  <div class="container_btn">
    <input type="button" value="(" @click="checkButton('(')" class="btn symbol_btn">
    <input type="button" value=")" @click="checkButton(')')" class="btn symbol_btn">
    <input type="button" value="%" @click="checkButton('%')" class="btn symbol_btn">
    <input type="button" value="CE" @click="checkButton('CE')" class="btn symbol_btn CE_btn">
    <input type="button" value="AC" @click="checkButton('AC')" class="btn symbol_btn AC_btn hide_btn">
    <input type="button" value=7 @click="checkButton(7)" class="btn num_btn">
    <input type="button" value=8 @click="checkButton(8)" class="btn num_btn">
    <input type="button" value=9 @click="checkButton(9)" class="btn num_btn">
    <input type="button" value="÷" @click="checkButton('/')" class="btn symbol_btn">
    <input type="button" value=4 @click="checkButton(4)" class="btn num_btn">
    <input type="button" value=5 @click="checkButton(5)" class="btn num_btn">
    <input type="button" value=6 @click="checkButton(6)" class="btn num_btn">
    <input type="button" value="×" @click="checkButton('*')" class="btn symbol_btn">
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
      formula: null,
      formulaArray: [],
      calculateArray: [],
      error: '',
      result: null
    };
  },
  methods: {
    checkButton(value){
      switch(value){
        case '=':
          this.calculate(this.formula);
          document.querySelector('.CE_btn').classList.add('hide_btn');
          document.querySelector('.AC_btn').classList.remove('hide_btn');
          break;
        case 'CE':
          console.log('CE');
          // this.formulaArray.pop();
          break;
        case 'AC':
          this.formula = null;
          this.formulaArray = [];
          this.result = null;
          break;
        default :
          console.log(value);
          this.updateFormula(value); 
      }
    },
    updateFormula(checkedValue) {
      this.formulaArray.push(checkedValue);
      this.formula = '';

      for (let i = 0; i < this.formulaArray.length; i++) {
        if(typeof this.formulaArray[i] === 'number' || (this.formulaArray[i] === '.')) {
          this.formula += this.formulaArray[i].toString();
        } else {
          this.formula += ' ' + this.formulaArray[i] + ' ';
        }
      }
    },
    calculate(checkFormula) {
      // サニタイジング
      checkFormula = this.sanitizeFormula(checkFormula);

      this.result = '= ' + Math.round(eval(checkFormula) * 10000000000) / 10000000000;
    },
    sanitizeFormula(checkFormula) {
      checkFormula = checkFormula.replace(/</g, "&lt;");
      checkFormula = checkFormula.replace(/>/g, "&gt;");
      checkFormula = checkFormula.replace(/&/g, "&amp;");
      checkFormula = checkFormula.replace(/"/g, "&quot;");
      checkFormula = checkFormula.replace(/'/g, "&#39;");
      checkFormula = checkFormula.replace(/@/g, "%40");
      checkFormula = checkFormula.replace(/:/g, "%3A");
      checkFormula = checkFormula.replace(/;/g, "%3B;");
      return checkFormula;
    }
  }
};
</script>

<style scoped>
  * {
    font-size: 24px;
    color: #000000;
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