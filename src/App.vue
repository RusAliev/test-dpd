<template>
  <router-view/>
</template>

<style lang="stylus">
body {
  margin: 0;
}

.base-wrapper
  font-family Avenir, Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  width: 100%;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  background-color: #ededf5;

.grid__block {
  width: 100%;
}

.grid__block-inner {
  padding: 12px 8px 16px;
  background-color: #fff;
  border-radius: 8px;
}

.grid__block-name {
  margin-bottom: 5px;
  opacity: 0.4;
  color: #1c1b1b;

  font-weight: 700;
  text-transform: uppercase;

  font-size: 12px;
}
</style>
