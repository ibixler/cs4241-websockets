<style>
  body {
    margin: 0;
    background: black;
  }
</style>
<script>
  import { onMount } from "svelte";
  let celebration;
  let ws,
    msgs = [],
    ctx = null;

  window.onload = function () {
    ws = new WebSocket("ws://127.0.0.1:3000");

    ws.onopen = () => {
      celebration.play();
      ws.onmessage = async (msg) => {
        const pos = await msg.data.text();
        const [x, y] = pos.split(":").map((v) => parseInt(v));

        ctx.fillStyle = "red";
        ctx.fillRect(x, y, 50, 50);
      };
    };

    const canvas = document.querySelector("canvas");
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    ctx = canvas.getContext("2d");

    window.onclick = (e) => {
      console.log(e.pageX, e.pageY);
      ws.send(`${e.pageX - 50}:${e.pageY - 50}`);
      ctx.fillStyle = "yellow";
      ctx.fillRect(e.pageX - 50, e.pageY - 50, 50, 50);
    };
  };
</script>

<audio
  src="https://sveltejs.github.io/assets/music/strauss.mp3"
  preload="auto"
  bind:this={celebration}
  hidden
></audio>

<body>
  <canvas></canvas>
</body>

