<script>
  import { Layer } from "svelte-canvas"
  export let x = 0
  export let y = 0
  export let name

  $: render = ({ context }) => {
    var imageObj = new Image()
    imageObj.src = `/images/${name}.png`
    imageObj.onload = function () {
      context.drawImage(imageObj, x, y, 50, 50)
    }
  }
</script>

<Layer {render} />

<style>
</style>
