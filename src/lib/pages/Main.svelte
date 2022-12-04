<script lang="ts">
  import { push } from 'svelte-spa-router'
</script>

<main class="h-screen w-screen flex flex-col justify-center items-center">
  <div class=flex>
    <input class="search outline-0 p-5 font-bold rounded-l-full w-[300px] h-[50px] bg-[#21262d]" />
    <button class="btn rounded-r-full w-[100px] h-[50px] font-bold bg-gradient-to-r from-purple-500 to-cyan-600 transition duration-300 hover:brightness-125" on:click={ () => { push("/user/dia") } }>Search</button>
  </div>
  <!-- window.location.href="/user" -->
</main>

<style>
</style>