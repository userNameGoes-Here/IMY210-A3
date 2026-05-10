<script setup>

</script>

<template>
  <div>
    <nav class="navPanel">
      <NuxtLink to="/" ><img src="https://img.icons8.com/?size=100&id=86527&format=png&color=000000" title="home"/></NuxtLink>
      <NuxtLink to="/search" ><img src="https://img.icons8.com/?size=100&id=2sWrwEXiaegS&format=png&color=000000" title="search post"/></NuxtLink>
      <NuxtLink to="/create"><img src="https://img.icons8.com/?size=100&id=38113&format=png&color=000000" title="add post"/></NuxtLink>
    </nav>  
  </div>

  <div>
      <NuxtPage />
  </div>
</template>

<style scoped>
  * {
    display: flex;
    justify-content: center;
    /*align-content: center;*/
  }

  .navPanel a {
  color: rgb(236, 236, 236);
  margin-left: 25px;
  text-decoration: none;
  font-weight: bold;
}

.navPanel a:hover {
  color: #93c5fd;
}
</style>
