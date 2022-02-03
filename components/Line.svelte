<script>
  import { Layer } from "svelte-canvas"
  import { curveBumpY } from "d3"
  export let x1 = 0
  export let y1 = 0
  export let x2 = 0
  // export let opacity = 1

  let y2 = 700

  let amount = 0

  // const _opacity = tweened(opacity, {
  //   duration: 800,
  //   interpolate: interpolateLab,
  // })

  $: render = ({ context }) => {
    const curve = curveBumpY(context)
    context.beginPath()
    context.strokeStyle = "rgb(185, 160, 140)"
    curve.lineStart()
    curve.point(x1, y1)
    curve.point(x2, y2)

    curve.lineEnd()
    context.stroke()

    // const _color = tweened(fill, {
    //   duration: 800,
    //   interpolate: interpolateLab,
    // })
    // const _x = tweened(x, { duration: 600, easing: cubicOut })
    // $: _x.set(x)

    // const _y = tweened(y, { duration: 600, easing: cubicOut })
    // $: _y.set(y)

    // $: _color.set(fill)

    /////////////

    // context.beginPath()
    // context.moveTo(x1, y1)
    // context.lineTo(x2, y2)
    // context.stroke()

    ///////////////
    // const curve = curveBumpY(context)
    // context.beginPath()
    // context.strokeStyle = "rgba(219, 191, 168, 0.8)"
    // curve.lineStart()
    // curve.point(x1, y1)
    // curve.point(x2, y2)

    // var amount = 0
    // setInterval(function () {
    //   amount += 0.05
    //   context.clearRect(0, 0, 100, 100)
    //   context.strokeStyle = "black"
    //   context.moveTo(startX, startY)
    //   context.lineTo(
    //     startX + (endX - startX) * amount,
    //     startY + (endY - startY) * amount
    //   )
    //   context.stroke()
    // }, 30)

    //   var amount = 0
    //   setInterval(function () {
    //     amount += 0.05
    //     // context.clearRect(500, 500, 700, 700)
    //     context.strokeStyle = "black"
    //     context.moveTo(x1, y1)
    //     context.lineTo(x1 + (x2 - x1) * amount, y1 + (y2 - y1) * amount)
    //     context.stroke()
    //   }, 30)
  }
</script>

<Layer {render} />

<style>
</style>
