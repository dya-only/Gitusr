<script lang="ts">
  import Router from 'svelte-spa-router'

  import Navbar from './lib/components/Navbar.svelte'
  import Main from './lib/pages/Main.svelte'
  import User from './lib/pages/User.svelte'

  const routes = {
    '/': Main,
    '/user/:id': User
  }
</script>

<main>
  <Navbar />
  <Router { routes } />
</main>

<style>
</style>