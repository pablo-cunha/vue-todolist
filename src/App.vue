<template>
  <div id="app">
    <b-navbar  class="navbar" type="light" toggleable="lg">
    <b-navbar-brand href="#">To-Do List</b-navbar-brand>

    <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

    <b-collapse id="nav-collapse" is-nav>
      <b-navbar-nav>
        <b-nav-item class="nav-link" to="/">Tarefas</b-nav-item>
        <b-nav-item class="nav-link" to="/form">Formulário</b-nav-item>
      </b-navbar-nav>
    </b-collapse>
  </b-navbar>
  
    <router-view/>
  </div>
</template>

<style scoped>
.navbar {
  border-bottom: 3px solid #7F00FF
  }

</style>
