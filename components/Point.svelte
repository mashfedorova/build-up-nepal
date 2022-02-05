<script>
  import { Layer } from "svelte-canvas"
  import { tweened } from "svelte/motion"
  import { cubicOut } from "svelte/easing"
  import { interpolateLab } from "d3-interpolate"

  export let fill = "rgb(188, 74, 60)"

  export let x = 0
  export let y = 0
  export let width

  const _color = tweened(fill, {
    duration: 800,
    interpolate: interpolateLab,
  })
  const _x = tweened(x, { duration: 600, easing: cubicOut })
  $: _x.set(x)

  const _y = tweened(y, { duration: 600, easing: cubicOut })
  $: _y.set(y)

  $: _color.set(fill)

  $: render = ({ context }) => {
    context.beginPath()
    context.arc($_x, $_y, width > 640 ? 5 : 3, 0, 2 * Math.PI)
    context.fillStyle = $_color
    context.fill()
    context.stroke()
    context.strokeStyle = "#e0e1e2"
  }
</script>

<Layer {render} />

<style>
</style>
