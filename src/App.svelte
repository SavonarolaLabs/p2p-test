<script>
  import { onMount } from 'svelte';
  import svelteLogo from './assets/svelte.svg'
  import Counter from './lib/Counter.svelte'
  import os from 'socket:os'
  import { Peer } from 'socket:peer'

  const toProperCase = s => s[0].toUpperCase() + s.slice(1)

  onMount(async ()=>{
    const pair = await Peer.createKeys()
    const clusterId = await Peer.createClusterId()

    const peer = new Peer({ ...pair, clusterId })
console.log("---------------")
console.log(peer)
console.log(("---------------"))
    
   peer.on('greeting', (value, peer, address, port) => {
   console.log(value)
     })

// window.onload = () => {
   const value = { english: 'hello, world' }
   const packet = await peer.emit('greeting', value)
// }

console.log(pair)
console.log(clusterId)
    console.log("log")

})




</script>

<main>
  <div>
    <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
  </div>
  <h1>Vite + Svelte (on {toProperCase(os.platform())})</h1>

  <div class="card">
    <Counter />
  </div>
</main>

<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
</style>
