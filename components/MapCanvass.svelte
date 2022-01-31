<script>
  import { geoAlbers, geoPath } from "d3"
  import { _ } from "lodash"
  import { Canvas } from "svelte-canvas"
  import { Layer } from "svelte-canvas"
  import Point from "./Point.svelte"
  // import scaleCanvas from "./scale-canvas.js"

  export let geo
  export let data
  export let width = 1000

  $: console.log(width)

  let canvasElement
  let blankMap

  // $: projection = geoAlbers()
  //   .center([87, 28])
  //   .rotate([-85, 0])
  //   .parallels([27, 32])
  //   .fitExtent(
  //     [
  //       [0, 0],
  //       [1000, 1100],
  //     ],
  //     geo
  //   )

  $: dataCalc = data.map((d) => {
    const latF = +d.lat
    const longF = +d.long
    const lat = projection([d.long, d.lat])[0]
    const long = projection([d.long, d.lat])[1]
    return {
      latF: latF,
      longF: longF,
      lat: lat,
      long: long,
    }
  })

  // $: path = geoPath(projection)

  $: projection = geoAlbers()
    .center([87, 28])
    .rotate([-85, 0])
    .parallels([27, 32])
    .scale(width / (2 * Math.PI))
    .fitExtent(
      [
        [0, 0],
        [width, 1000],
      ],
      geo
    )

  $: render = ({ context }) => {
    // context.fillStyle = "red"

    const path = geoPath().projection(projection).context(context)
    context.beginPath()
    path(geo)
    // context.arc(100, 100, 5, 0, 2 * Math.PI)
    context.strokeStyle = "black"
    context.stroke()
  }
</script>

<!-- <circle cx="500" cy="500" r="10" fill="red" /> -->
<Canvas {width} height="1000">
  <Layer {render} />
  {#each dataCalc as { lat, long }}
    <Point x={lat} y={long} fill="tomato" r="5" stroke="black" />
  {/each}
</Canvas>

<style>
</style>
