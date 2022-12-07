<script lang="ts">
  import { beforeUpdate, onMount } from "svelte";
  // require("dotenv").config();
  import Fa from 'svelte-fa'
  import { faBuilding, faCompass, faLocationDot, faUser, faThumbTack } from '@fortawesome/free-solid-svg-icons'

  export let params
  
  let res: any = {}
  let orgs: any = []
  
  let _flws = []
  let flws = []
  let _flwn = []
  let flwn = []

  onMount(async () => {

    const resp = await fetch(`https://api.github.com/users/${ params.id }`).then(resp => resp.json())
    const oresp = await fetch(`https://api.github.com/users/${ params.id }/orgs`).then(resp => resp.json())

    orgs = oresp
    res = resp
    console.log(res)
    // console.log(orgs)
    
    if (res.company == null) {
      res.company = "__"
    }
    if (res.location == null) {
      res.location = "__"
    }
    if (res.blog == null) {
      res.blog = "__"
    }

    let p:number = Math.ceil(parseInt(res.followers) / 30)
    let p2:number = Math.ceil(parseInt(res.following) / 30)

    for (let i = 0; i < p; i++) {
      const followers = await fetch(`https://api.github.com/users/${ params.id }/followers?page=${ i + 1 }`).then(resp => resp.json())

      followers.forEach((el: any) => {
        _flws.push({ login: el.login, avatar_url: el.avatar_url })
      })

    }
    flws = _flws

    for (let i = 0; i < p2; i++) {
      const following = await fetch(`https://api.github.com/users/${ params.id }/following?page=${ i + 1 }`).then(resp => resp.json())

      following.forEach((el: any) => {
        _flwn.push({ login: el.login, avatar_url: el.avatar_url })
      })

    }
    flwn = _flwn

  })

</script>

<main class="overflow-hidden">
  <div class="user h-screen w-screen flex justify-center items-center flex-col">
    <div class="flex justify-center items-end flex-wrap">
  
      <!-- Profile -->
      <div class="profile p-10 md:m-5 md:rounded-2xl drop-shadow-2xl bg-[#161B22] flex justify-center items-center flex-wrap">
        <img class="avatar rounded-3xl w-64 h-64 md:mr-12 transition ease-in-out  hover:brightness-50" src={res.avatar_url} alt="" />
        <div class="usr">
          <div class="name text-3xl font-bold">{ res.name }</div>
          <a class="sub-name text-2xl font-light underline" href={ res.html_url } target="_blank" rel="noreferrer" alt="">@{ res.login }</a>
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
            <Fa class="mr-2" icon={faCompass} fw size="lg" /> <a class="text underline" href={ res.blog } target="_blank" rel="noreferrer" alt="">{ res.blog }</a>        
          </div>
        </div>
      </div>
  
    </div>
  
    <div class="flex justify-center items-start flex-wrap">
  
      <!-- Organizations -->
      <div class="org-card p-10 mb-5 mr-5 rounded-l-2xl rounded-r-2xl drop-shadow-2xl bg-[#161B22] max-h-[150px] overflow-auto scrollbar scrollbar-thumb-neutral-700 scrollbar-thumb-rounded-full scrollbar-thin flex justify-center items-center flex-wrap max-w-[400px]">
        { #each orgs as org }
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <img class="org-img m-2 rounded-2xl w-12 cursor-pointer transition ease-out hover:brightness-50" src={ org.avatar_url } alt="" on:click={ () => { window.open(`http://github.com/${org.login}`, '_blank') } } />
        { /each }
  
        <!-- <div class="org-more m-2 rounded-2xl w-12 h-12 cursor-pointer transition ease-out hover:brightness-50 bg-[#161B22] text-4xl flex justify-center" on:click={ () => { window.open(`http://github.com/${res.login}`, '_blank') } }>+</div> -->
      </div>
  
      <!-- Bio -->
      <div class="flw p-10 mb-5 rounded-2xl drop-shadow-2xl bg-[#161B22] flex justify-center items-center max-w-[400px]">
        <Fa class="mr-4" icon={faThumbTack} fw size="lg" />
        <div class="text">{ res.bio }</div>
      </div>
    </div>

  </div>

  <div class="under h-screen w-screen flex justify-center items-center flex-col">

    <div class="flex justify-center items-end">

      <!-- Followers -->
      <div class="flw-card p-10 mb-5 mr-5 rounded-l-2xl rounded-r-2xl drop-shadow-2xl bg-[#161B22] max-h-[300px] w-[600px] overflow-auto scrollbar scrollbar-thumb-neutral-700 scrollbar-thumb-rounded-full scrollbar-thin flex justify-start items-start flex-col">
        <span class="font-bold text-xl mb-8">Followers</span>
        { #each flws as flw, i }
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <div class="flex justify-start items-center transition ease-in-out hover:brightness-50 hover:0 cursor-pointer" on:click={ () => { window.open(`http://github.com/${flw.login}`, '_blank') } }>
            <img class="flw-img m-2 rounded-2xl w-12" src={ flw.avatar_url } alt="" />
            <div class="flw-name font-bold mr-12">{ flw.login }</div>
          </div>
        { /each }
      </div>

      <!-- Following -->
      <div class="flw-card p-10 mb-5 mr-5 rounded-l-2xl rounded-r-2xl drop-shadow-2xl bg-[#161B22] max-h-[300px] w-[600px] overflow-auto scrollbar scrollbar-thumb-neutral-700 scrollbar-thumb-rounded-full scrollbar-thin flex justify-start items-start flex-col">
        <span class="font-bold text-xl mb-8">Following</span>
        { #each flwn as fln, i }
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <div class="flex justify-start items-center transition ease-in-out hover:brightness-50 hover:0 cursor-pointer" on:click={ () => { window.open(`http://github.com/${fln.login}`, '_blank') } }>
            <img class="flw-img m-2 rounded-2xl w-12" src={ fln.avatar_url } alt="" />
            <div class="flw-name font-bold">{ fln.login }</div>
          </div>
        { /each }
      </div>

    </div>

  </div>
</main>

<style>
</style>