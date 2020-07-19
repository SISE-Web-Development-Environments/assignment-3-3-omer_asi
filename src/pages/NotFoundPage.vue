<template>
  <div class="container">
    <p><br></p>
    <b-jumbotron class="title w3-center w3-padding-16" >
      <template v-slot:header>404</template>
      <p>No Page To Show On This Path...</p>    
      <router-link to="/" exact>ET Go Home</router-link>
    </b-jumbotron>   
  </div>
</template>
