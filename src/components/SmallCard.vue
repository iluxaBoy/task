<template>
  <div class="card">
    <div class="arrow"></div>
    <slot></slot>
  </div>
</template>

<style scoped>
    .card {
      position: relative;
      width: 230px;
      height: 180px;
      background-color: rgba(255, 255, 255, 0.95);
      border-radius: 10px;
      border-top-left-radius: 0;
    }

    .arrow{
      position: absolute;
      left: -20px;
      background-color: rgba(255, 255, 255, 0.95);
      border-top-left-radius: 10px;
      border-bottom-left-radius: 100px;
      width: 20px;
      height: 20px;
    }

    .slot{
      font-size: 15px;
    }
</style>