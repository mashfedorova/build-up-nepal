<script>
  import { Layer } from "svelte-canvas"

  export let x = 0
  export let y = 0
  export let fill = "red"

  $: render = ({ context }) => {
    context.fillStyle = fill
    context.beginPath()
    context.arc(x, y, 5, 0, 2 * Math.PI)
    context.fill()
  }
</script>

<Layer {render} />

<style>
</style>
