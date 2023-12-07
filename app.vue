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
    <input type="button" value="(" @click="updateFormula('(')" class="btn symbol_btn">
    <input type="button" value=")" @click="updateFormula(')')" class="btn symbol_btn">
    <input type="button" value="%" @click="updateFormula('%')" class="btn symbol_btn">
    <input type="button" value="CE" @click="updateFormula('CE')" class="btn symbol_btn">
    <input type="button" value=7 @click="updateFormula(7)" class="btn num_btn">
    <input type="button" value=8 @click="updateFormula(8)" class="btn num_btn">
    <input type="button" value=9 @click="updateFormula(9)" class="btn num_btn">
    <input type="button" value="÷" @click="updateFormula('÷')" class="btn symbol_btn">
    <input type="button" value=4 @click="updateFormula(4)" class="btn num_btn">
    <input type="button" value=5 @click="updateFormula(5)" class="btn num_btn">
    <input type="button" value=6 @click="updateFormula(6)" class="btn num_btn">
    <input type="button" value="×" @click="updateFormula('×')" class="btn symbol_btn">
    <input type="button" value=1 @click="updateFormula(2)" class="btn num_btn">
    <input type="button" value=2 @click="updateFormula(2)" class="btn num_btn">
    <input type="button" value=3 @click="updateFormula(3)" class="btn num_btn">
    <input type="button" value="-" @click="updateFormula('-')" class="btn symbol_btn">
    <input type="button" value=0 @click="updateFormula(0)" class="btn num_btn">
    <input type="button" value="." @click="updateFormula('.')" class="btn num_btn">
    <input type="button" value="=" @click="updateFormula('=')" class="btn equal_btn">
    <input type="button" value="+" @click="updateFormula('+')" class="btn symbol_btn">
  </div>
</template>

<script>
export default {
  data() {
    return {
      formula: null,
      formulaArray: [],
      error: '',
      result: null
    };
  },
  methods: {
    updateFormula(value) {
      this.formulaArray.push(value);
      console.log(this.formulaArray);
      this.formula = '';

      for (let i = 0; i < this.formulaArray.length; i++) {
        if(typeof this.formulaArray[i] === 'number' || (this.formulaArray[i] === '.')) {
          this.formula += this.formulaArray[i].toString();
        }
        else {
          this.formula += ' ' + this.formulaArray[i] + ' ';
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
  }

  .container_inputResult,
  .container_btn {
    max-width: 500px;
    width: 80%;
    margin: 60px auto;
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
    justify-content: start;
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
    background: red;
    color: #FFFFFF;
  }
</style>
