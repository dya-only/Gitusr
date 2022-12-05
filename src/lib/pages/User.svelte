<script lang="ts">
  import { onMount } from "svelte";
  import Fa from 'svelte-fa'
  import { faBuilding, faCompass, faLocationDot, faUser, faEnvelope } from '@fortawesome/free-solid-svg-icons'

  export let params

  let res: any = {}

  onMount(async () => {
    const resp = await fetch(`https://api.github.com/users/${ params.id }`).then(resp => resp.json())
    res = resp
    console.log(res)

    if (res.company == null) {
      res.company = "__"
    }
    if (res.location == null) {
      res.location = "__"
    }
    if (res.blog == null) {
      res.blog = "__"
    }
  })

</script>

<main class="h-screen w-screen flex justify-center items-center">
  
  <div class="flex justify-center items-end flex-wrap">

    <!-- Profile -->
    <div class="profile p-10 md:m-5 md:rounded-2xl drop-shadow-2xl bg-[#161B22] flex justify-center items-center flex-wrap">
      <img class="avatar rounded-3xl w-64 h-64 md:mr-12 transition ease-in-out  hover:brightness-50" src={res.avatar_url} alt="" />
      <div class="usr">
        <div class="name text-3xl font-bold">{ res.name }</div>
        <a class="sub-name text-2xl font-light underline" href={ res.html_url } alt="">@{ res.login }</a>
      </div>
    </div>
  
    <div>
      <!-- Follow/Following -->
      <div class="flw p-10 mb-5 rounded-2xl drop-shadow-2xl bg-[#161B22] flex place-items-center">
        <div class="flex">
          <Fa class="mr-2" icon={faUser} fw size="lg" />
          <div class="flex">
            <div class="text font-bold">{ res.followers } <span class="font-thin">Followers</span></div>
            <div class="mr-2 ml-2"> · </div>
            <div class="text font-bold">{ res.following } <span class="font-thin">Following</span></div>
          </div>
        </div>
      </div>

      <!-- Info -->
      <div class="info p-10 mb-5 rounded-2xl drop-shadow-2xl bg-[#161B22] flex flex-col">
        <div class="flex">
          <Fa class="mr-2 mb-4" icon={faBuilding} fw size="lg" /> <div class="text">{ res.company }</div>
        </div>
        <div class="flex">
          <Fa class="mr-2 mb-4" icon={faLocationDot} fw size="lg" /> <div class="text">{ res.location }</div>
        </div>
        <div class="flex">
          <Fa class="mr-2" icon={faCompass} fw size="lg" /> <a class="text underline" href={ res.blog } alt="">{ res.blog }</a>        
        </div>
  
      </div>
    </div>

  </div>
</main>

<style>
</style>