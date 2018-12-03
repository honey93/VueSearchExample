<template>
  <div id="app">
    <!-- <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div> -->
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

}

*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.pad-15-hor{
  padding-left:15px;
  padding-right:15px;
}

.pad-15-ver{
  padding-top:15px;
  padding-bottom: 15px; 
}

.green{
  color: green;
}

@media screen and (max-width:550px) {

  .min-width-160{
  min-width: 160px;
}

.max-width-160{
  max-width:160px;
  overflow: hidden;
  text-overflow: ellipsis;
  word-break: break-all;
  text-align:right;
}
  
}


</style>
