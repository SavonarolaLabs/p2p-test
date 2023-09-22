<script>
  import { onMount } from "svelte";
  import svelteLogo from "./assets/svelte.svg";
  import os from "socket:os";

  const toProperCase = (s) => s[0].toUpperCase() + s.slice(1);

  let text = '';
  let subcluster;

  import { network, Encryption } from 'socket:network'

  onMount(async () => {
    console.log('onMount')
    const sharedKey = await Encryption.createSharedKey()
    const peerId = await Encryption.createId();
    const clusterId = await Encryption.createClusterId();
    console.log({sharedKey, peerId, clusterId})

    const socket = await network({
      peerId,
      clusterId
    })
    console.og({socket})

    socket.on('#ready', () => {
      console.log('ready')
    })

    subcluster = await socket.subcluster({ sharedKey })
    console.og({subcluster})
    subcluster.on('message', async (value, packet) => {
      console.log(JSON.stringify(value))
    })
  });

  async function sendText(txt){
    console.log("sendText");

    //peer.emit("greeting", { hello: txt });
    const opts = await Encryption.createKeyPair()
    const message = { foo: txt }
    console.log({opts});

    console.log("before subcluster.emit");
    const packets = await subcluster.emit('message', message, opts);
    console.log("after subcluster.emit");
  }
</script>

<main>
  <h1>Vite + svelte svelte (on {toProperCase(os.platform())})</h1>

  <textarea name="" id="" cols="30" rows="10" bind:value={text}></textarea>
  <div class="card">
    <button on:click={()=>sendText(text)}>send</button>
  </div>
</main>