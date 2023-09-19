<script>
  import { onMount } from 'svelte';
  import svelteLogo from './assets/svelte.svg'
  import Counter from './lib/Counter.svelte'
  import os from 'socket:os'
  import { Peer } from 'socket:peer'

  const toProperCase = s => s[0].toUpperCase() + s.slice(1)

  onMount(async ()=>{
    const pair = await Peer.createKeys()
    const clusterId = '7b51fd73d4fa2e54d3df8a3f8400aaf4f189f9be0914eb5cbf632ed1a2ad4f9f'
    const peer = new Peer({ ...pair, clusterId })
    const network=await peer.join()
    //peer.write('foo', { value: true })


console.log({clusterId})

    
   peer.on('greeting', (value, peer, address, port) => {
   console.log('!!!!!!!!!!!GREETING')
   console.log(value)
  })



// window.onload = () => {
   const value = { english: 'hello, world' }
   const packet = await peer.emit('greeting', value)
   console.log("packet sent")
   console.log({packet})
// }





network.onData = (packet, port, address, data) => {
  if (packet.message.length) return // ignore control packets

  const unsealed = data
  if (!unsealed) return // use caution

  console.log(Date.now(),{unsealed})
}

network.onConnect = (peer, packet, port, address) => {
  network.send("abc", port, address)
}

})




</script>

<main>
  <div>
    <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
  </div>
  <h1>Vite + svelte svelte (on {toProperCase(os.platform())})</h1>

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
