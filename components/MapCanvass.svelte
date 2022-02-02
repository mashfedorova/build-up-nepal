<script>
  import { geoAlbers, geoPath } from "d3"
  import { _ } from "lodash"
  import { Canvas } from "svelte-canvas"
  import { Layer } from "svelte-canvas"
  import { forceSimulation, forceCollide, forceX, forceY, scalePoint } from "d3"
  import Point from "./Point.svelte"
  import Line from "./Line.svelte"
  import { getContext } from "svelte"
  import { dirty_components } from "svelte/internal"
  import { fade } from "svelte/transition"
  // import scaleCanvas from "./scale-canvas.js"

  // const data = getContext("MapCanvass")
  // $: console.log(data)

  export let data
  export let geo
  export let width = 1000
  export let whichY = "long"
  export let whichX = "lat"
  export let fX = ""
  export let lines = ""
  // export let simX = ""
  export let test = "one"

  let r = 4

  // $: console.log(whichX)

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

  let xScale = function () {}
  let xScale2 = function () {}

  $: if (fX === "typeRec") {
    xScale = scalePoint()
      .domain([
        "Entrepreneur",
        "INGO/NGO project",
        "Female Entrepreneur",
        "Returning Migrant",
        "Other",
      ])
      .range([80, width - 80])
  }

  $: if (fX === "statusGrouped") {
    xScale = scalePoint()
      .domain(["Running", "Struggling", "Closed", "Data pending"])
      .range([80, width - 80])
  }

  $: console.log(data)
  // $: if (fX === "peakImpYear") {
  //   xScale = scalePoint()
  //     .domain([2016, 2017, 2018, 2019, 2020, 2021])
  //     .range([80, width - 80])
  // }

  $: xScaleYears = scalePoint()
    .domain([2016, 2017, 2018, 2019, 2020, 2021])
    .range([80, width - 80])

  // $: xScale = scalePoint()
  //   .domain([
  //     "Entrepreneur",
  //     "INGO/NGO project",
  //     "Female Entrepreneur",
  //     "Returning Migrant",
  //     "Other",
  //   ])
  //   .range([80, width - 80])

  // $: xScale2 = scalePoint()
  //   .domain([2016, 2017, 2018, 2019, 2020, 2021])
  //   .range([80, width - 80])

  // $: console.log(xScale)
  // $: data.map((d) => d.peakImpYear)

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
      typeRec: d.typeRec,
      // x: xScale(d.typeRec),
      // statusGrouped: d.statusGrouped,
      one: xScale(d.typeRec),
      // two: xScale(d.peakImpYearNum),
      two: xScale(d.statusGrouped),
      startYear: +d.startYear,
    }
  })

  // $: console.log(data)
  // $: console.log(dataCalc)

  // $: console.log(xScale(2019))
  // $: console.log(dataCalc)

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

  // $: initialNodes = $data.map((d) => ({ ...d }))
  // $: simulation = forceSimulation(dataCalc)
  // let nodes = []

  // simulation.on("tick", () => {
  //   nodes = simulation.nodes()
  // })

  // $: console.log(nodes)

  $: simulation = forceSimulation(dataCalc)
    .force(
      "x",
      forceX()
        .x((d) => d[test])
        // .x((d) => d[simX])
        .strength(0.5)
    )
    .force("y", forceY(400).strength(0.5))
    .force("collide", forceCollide(6).strength(0.5).iterations(1))
    // .alpha(1)
    // .restart()
    .alphaDecay(0.1)

  // $:  simulation = forceSimulation(dataCalc)
  //     .force(
  //       "x",
  //       forceX()
  //         .x((d) => d[test])
  //         // .x((d) => d[simX])
  //         .strength(0.5)
  //     )
  //     .force("y", forceY(400).strength(0.5))
  //     .force("collide", forceCollide(6).strength(0.5).iterations(1))
  //     .alpha(1)
  //     .restart()

  $: for (let i = 0; i < dataCalc.length; ++i) simulation.tick()

  // $: simulation.alpha(1).restart()
  // $: simulation.restart()

  // $: render2 = ({ context }) => {
  //   // context.beginPath()

  //   // context.rect(200, 200, 400, 400)
  //   // context.fill()

  //   let rectWidth = 200

  //   setInterval(() => {
  //     rectWidth += 10
  //     context.beginPath()
  //     // context.fillStyle = "rgba(219, 191, 168, 0.658)"
  //     context.fillStyle = "blue"
  //     context.fillRect(0, 200, 800, 200)
  //     context.fillRect(0, 200, 800, rectWidth)
  //     // context.fill()
  //   }, 10)
  // }

  $: render = ({ context }) => {
    // map outline
    context.fillStyle = "red"
    const path = geoPath().projection(projection).context(context)
    context.beginPath()
    path(geo)
    context.strokeStyle = "black"
    context.stroke()
    context.save()
    if (whichX === "x") {
      context.translate(width / 2, 500)
    }

    // for (const { x, y } of dataCalc) {
    //   // context.beginPath()
    //   // context.arc(x, y, 5, 0, 2 * Math.PI)
    //   // context.fill()
    // }
    // context.restore()

    // test lines
    // var startX = 50
    // var startY = 50
    // var endX = 100
    // var endY = 100
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

    // context.strokeStyle = "black"
    // context.moveTo(200, 200)
    // context.lineTo(300, 300)

    // context.stroke()
  }
</script>

<!-- <circle cx="500" cy="500" r="10" fill="red" /> -->
<Canvas {width} height="1000">
  <!-- <Layer render={render2} /> -->
  {#if whichX === "lat"}
    <Layer {render} />
  {/if}
  {#each dataCalc as d}
    <Point x={d[whichX]} y={d[whichY]} />
  {/each}
  {#if lines === "show"}
    {#each dataCalc as d}
      <Line x1={d.x} y1={d.y} x2={xScaleYears(d.startYear)} />
    {/each}
  {/if}
</Canvas>

<style>
</style>
