<script>
  import { onMount } from "svelte";
  import svelteLogo from "./assets/svelte.svg";
  import Counter from "./lib/Counter.svelte";
  import os from "socket:os";
  import { Peer } from "socket:peer";

  const toProperCase = (s) => s[0].toUpperCase() + s.slice(1);

  let network;
  let port;
  let address;
  let peer;

  onMount(async () => {
    const pair = await Peer.createKeys();
    const clusterId =
      "7b51fd73d4fa2e54d3df8a3f8400aaf4f189f9be0914eb5cbf632ed1a2ad4f9f";
    peer = new Peer({ ...pair, clusterId });
    network = await peer.join();
    //peer.write('foo', { value: true })

    console.log({ clusterId });

    peer.on("greeting", (value, peer, address, port) => {
      console.log({value, peer, address, port});
    });

    const packet = await peer.emit("greeting", { hello: "world" });
    console.log("packet sent");
    //console.log({ packet });

  });

  function sendText(txt){
    peer.emit("greeting", { hello: txt });
  }
  let text = ''
</script>

<main>
  <div>
    <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
  </div>
  <h1>Vite + svelte svelte (on {toProperCase(os.platform())})</h1>

  <textarea name="" id="" cols="30" rows="10" bind:value={text}></textarea>
  <div class="card">
    <button on:click={()=>sendText(text)}>send</button>
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
