<template>
  <nav class="navbar navbar-expand-lg bg-primary">
    <div class="container-fluid">
      <a class="navbar-brand" href="/">AGENDAS V1</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
        <div class="navbar-nav">
          <router-link to="/" class="nav-link">Home</router-link>
          <router-link to="/agenda" class="nav-link">Agenda</router-link>
          <router-link to="/about" class="nav-link">About</router-link>
        </div>
      </div>
    </div>
  </nav>
  <div class="container">
    <router-view/>
  </div>
</template>

<style  lang="scss">

</style>
