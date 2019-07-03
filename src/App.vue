<template lang="pug">
  #app
    #nav
      router-link(to="/") Home
      span  | 
      router-link(to="/about") About
    router-view
</template>

<style lang="stylus">
@import url('https://fonts.googleapis.com/css?family=Noto+Sans+JP&display=swap&subset=japanese');
#app
  font-family: 'Noto Sans JP', sans-serif;
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50

#nav
  padding 30px
  a
    font-weight bold
    color #2c3e50
    &.router-link-exact-active
      color #42b983
</style>
