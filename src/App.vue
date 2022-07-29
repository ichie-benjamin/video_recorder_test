<script setup>
import {  RouterView } from 'vue-router'
</script>

<template>

  <nav class="navbar navbar-expand-lg fixed-top navbar-light bg-white shadow-sm" aria-label="Main navigation">
    <div class="container">
      <a class="navbar-brand" href="#">Adilo</a>
      <button class="navbar-toggler p-0 border-0" type="button" id="navbarSideCollapse" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="navbar-collapse offcanvas-collapse" id="navbarsExampleDefault">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">

          <li class="nav-item">
            <a class="nav-link" href="#">Projects</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Tools & App</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Channels</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Contacts</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Analytics</a>
          </li>
          <li class="nav-item">
            <a class="nav-link" href="#">Settings</a>
          </li>

        </ul>
        <div class="d-flex" role="search">
          <img class="me-3" src="@/assets/profile_pics.png" alt="" width="40" height="40">
          <div class="lh-1">
            <h1 class="h6 mb-0 text-white lh-1">Bootstrap</h1>
            <small>Since 2011</small>
          </div>

        </div>
      </div>
    </div>
  </nav>



  <RouterView />
</template>

<style scoped>

</style>
