<template>
<div id="pokemon">
    <router-view/>
</div>

  
</template>

<style>
@font-face {
  font-family: DMSans;
  src: url('./assets/fonts/DMSans-Regular.ttf');
}
body{
  margin:0;
  padding: 0;
  
}
#app{
   font-family: DMSans;
}
#pokemon{

  background: url('./assets/images/pokebackg.png') no-repeat center center fixed;
 margin: 0;
 padding: 0;
 overflow: scroll;
 height: 100vh;
 background-size: cover;
}





</style>
