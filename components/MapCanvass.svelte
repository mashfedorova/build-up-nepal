<script>
  import { geoAlbers, geoPath } from "d3"
  import { _ } from "lodash"
  import { Canvas } from "svelte-canvas"
  import { Layer } from "svelte-canvas"
  import {
    forceSimulation,
    forceCollide,
    forceX,
    forceY,
    scalePoint,
    scaleOrdinal,
  } from "d3"
  import Point from "./Point.svelte"

  export let data
  export let geo
  export let width = 1000
  export let whichX = "lat"
  export let fX = ""
  export let groupingVar = "one"
  export let entrepreneur = ""
  export let colorVar = "const"
  export let whichY = "long"

  let r = 4

  let xScale = function () {}
  let yScale = function () {}
  let xScale2 = function () {}
  let colorScale = function () {}

  $: if (fX === "typeRec" && width > 640) {
    xScale = scalePoint()
      .domain([
        "Entrepreneur",
        "INGO/NGO project",
        "Returning Migrant",
        "Other",
      ])
      .range([120, width - 120])
  }

  $: if (fX === "typeRec" && width < 641) {
    xScale = scalePoint()
      .domain([
        "Entrepreneur",
        "INGO/NGO project",
        "Returning Migrant",
        "Other",
      ])
      .range([60, width - 60])
  }

  $: if (fX === "statusGrouped" && width > 640) {
    xScale = scalePoint()
      .domain(["Running", "Other"])
      .range([150, width - 150])
  }

  $: if (fX === "statusGrouped" && width < 641) {
    xScale = scalePoint()
      .domain(["Running", "Other"])
      .range([100, width - 100])
  }

  // $: console.log(data)
  // $: if (fX === "peakImpYear") {
  //   xScale = scalePoint()
  //     .domain([2016, 2017, 2018, 2019, 2020, 2021])
  //     .range([80, width - 80])
  // }

  $: if (colorVar === "const") {
    colorScale = scaleOrdinal()
      .domain(["Running", "Other"])
      .range(["#BC4A3C", "#BC4A3C", "#BC4A3C", "#BC4A3C"])
  }

  $: if (colorVar === "type") {
    colorScale = scaleOrdinal()
      .domain(["Running", "Other"])
      .range(["#BC4A3C", "#b8b8b8"])
  }

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
      one: xScale(d.typeRec),
      two: xScale(d.statusGrouped),
      startYear: +d.startYear,
      color: colorScale(d.statusGrouped),
    }
  })

  $: projection = geoAlbers()
    .center([87, 28])
    .rotate([-85, 0])
    .parallels([27, 32])
    .scale(width / (2 * Math.PI))
    .fitExtent(
      [
        [0, -200],
        [width, 1000],
      ],
      geo
    )

  $: simulation = forceSimulation(dataCalc)
    .force(
      "x",
      forceX()
        .x((d) => d[groupingVar])
        // .x((d) => d[simX])
        .strength(0.5)
    )
    .force("y", forceY(400).strength(0.5))
    .force(
      "collide",
      forceCollide(width > 640 ? 6 : 3)
        .strength(0.5)
        .iterations(1)
    )
    .alphaDecay(0.1)
    .alpha(1)
    .restart()

  $: for (let i = 0; i < dataCalc.length; ++i) simulation.tick()

  $: render = ({ context }) => {
    const path = geoPath().projection(projection).context(context)
    context.beginPath()
    path(geo)
    context.strokeStyle = "#83572b"
    context.stroke()
    context.save()
    if (whichX === "x") {
      context.translate(width / 2, 500)
    }
  }

  $: render2 = ({ context }) => {
    context.fillStyle = "black"
    context.beginPath()
    context.font = width > 640 ? "16px Alegreya Sans" : "10px Alegreya Sans"
    context.fillText(
      entrepreneur,
      width > 640 ? xScale("Entrepreneur") - 40 : xScale("Entrepreneur") - 35,
      530
    )
    context.fill()

    context.fillStyle = "black"
    context.beginPath()
    context.font = width > 640 ? "16px Alegreya Sans" : "10px Alegreya Sans"
    context.fillText(
      "INGO/NGO",
      width > 640
        ? xScale("INGO/NGO project") - 35
        : xScale("INGO/NGO project") - 30,
      530
    )
    context.fill()

    context.fillStyle = "black"
    context.beginPath()
    context.font = width > 640 ? "16px Alegreya Sans" : "10px Alegreya Sans"
    context.fillText(
      "Returning Migrant",
      width > 640
        ? xScale("Returning Migrant") - 50
        : xScale("Returning Migrant") - 40,
      530
    )
    context.fill()

    context.fillStyle = "black"
    context.beginPath()
    context.font = width > 640 ? "16px Alegreya Sans" : "10px Alegreya Sans"
    context.fillText(
      "Other",
      width > 640 ? xScale("Other") - 20 : xScale("Other") - 10,
      530
    )
    context.fill()

    context.fillStyle = "black"
    context.beginPath()
    context.font = width > 640 ? "16px Alegreya Sans" : "10px Alegreya Sans"
    context.fillText(
      "Running",
      width > 640 ? xScale("Running") - 20 : xScale("Running") - 10,
      530
    )
    context.fill()
  }
</script>

<Canvas {width} height="1000">
  {#if whichX === "x"}
    <Layer render={render2} />
  {/if}
  {#if whichX === "lat"}
    <Layer {render} />
  {/if}
  {#each dataCalc as d}
    <Point x={d[whichX]} y={d[whichY]} fill={d.color} {width} />
  {/each}
</Canvas>

<style>
</style>
