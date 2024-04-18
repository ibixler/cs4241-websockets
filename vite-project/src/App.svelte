<html lang="en">
  <head>
    <style>
      body { 
        margin:0; 
        background:black 
      } 
    </style>
    <script>
      let ws, msgs = [], ctx = null
      
      window.onload = function() {
        let celebration;
        ws = new WebSocket( 'ws://127.0.0.1:3000' )

        ws.onopen = () => {
          ws.onmessage = async msg => {
            const pos = await msg.data.text()
            const [x,y] = pos.split( ':' ).map( v => parseInt(v) )

            ctx.fillStyle = 'red'
            ctx.fillRect( x,y,50,50 )
          }
        }

        const canvas = document.querySelector('canvas')
        canvas.width = window.innerWidth
        canvas.height = window.innerHeight
        ctx = canvas.getContext( '2d' )

        window.onclick = e => {
          console.log(e.pageX, e.pageY);
          ws.send( `${e.pageX-50}:${e.pageY-50}` )
          ctx.fillStyle = 'yellow'
          ctx.fillRect( e.pageX-50,e.pageY-50,50,50 )
        }
      }
    </script>
  </head>
  <!-- <audio
    src='https://sveltejs.github.io/assets/music/strauss.mp3'
    preload="auto"
    bind:this={celebration}
    controls
  > -->
  <body>
    <canvas></canvas>
  </body>
</html>