<script>
  import { Layer } from "svelte-canvas"
  import { tweened } from "svelte/motion"
  import { cubicOut } from "svelte/easing"

  export let x = 0
  export let y = 0
  export let fill = "red"

  const _x = tweened(x, { duration: 600, easing: cubicOut })
  $: _x.set(x)

  const _y = tweened(y, { duration: 600, easing: cubicOut })
  $: _y.set(y)

  $: render = ({ context }) => {
    context.fillStyle = fill
    context.beginPath()
    context.arc($_x, $_y, 5, 0, 2 * Math.PI)
    context.fill()
  }
</script>

<Layer {render} />

<style>
</style>
