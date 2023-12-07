<template>
  <div class="container_btn">
    <input type="button" value="(" class="btn symbol_btn">
    <input type="button" value=")" class="btn symbol_btn">
    <input type="button" value="%" class="btn symbol_btn">
    <input type="button" value="CE" class="btn symbol_btn">
    <input type="button" value="7" class="btn num_btn">
    <input type="button" value="8" class="btn num_btn">
    <input type="button" value="9" class="btn num_btn">
    <input type="button" value="÷" class="btn symbol_btn">
    <input type="button" value="4" class="btn num_btn">
    <input type="button" value="5" class="btn num_btn">
    <input type="button" value="6" class="btn num_btn">
    <input type="button" value="×" class="btn symbol_btn">
    <input type="button" value="1" class="btn num_btn">
    <input type="button" value="2" class="btn num_btn">
    <input type="button" value="3" class="btn num_btn">
    <input type="button" value="-" class="btn symbol_btn">
    <input type="button" value="0" class="btn num_btn">
    <input type="button" value="." class="btn num_btn">
    <input type="button" value="=" class="btn equal_btn">
    <input type="button" value="+" class="btn symbol_btn">
  </div>
</template>

<style>
  .container_btn {
    max-width: 500px;
    width: 80%;
    margin: 60px auto;
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
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
    background: #1700FF;
    color: #FFFFFF;
  }
</style>
