<script>
  import { geoAlbers, geoPath } from "d3"
  import { _ } from "lodash"

  export let geo
  export let data
  export let width = 1000
  // $: console.log(data)
  // $: console.log(width, [width, width])

  $: projection = geoAlbers()
    .center([87, 28])
    .rotate([-85, 0])
    .parallels([27, 32])
    .fitExtent(
      [
        [0, 0],
        [width, 700],
      ],
      geo
    )
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
  // $: console.log(dataCalc)
  // $: console.log(geo)
  $: path = geoPath(projection)

  // $: data = geo.features

  // $: console.log(path(geo[0]))
  // $: test = projection([87, 28])
  // $: console.log(projection([87, 28])[0])
</script>

<svg x="-5000" {width} height="500">
  <!-- <svg width="1000" height="1000"> -->
  <!-- <circle cx="500" cy="500" r="10" /> -->
  {#each dataCalc as d}
    <circle cx={d.lat} cy={d.long} r="5" />
  {/each}

  <!-- {#each dataCalc as d}
    <text x="500" y="500"> {projection(d.lat, d.long)[0]}</text>
  {/each} -->

  <path d={path(geo)} fill="none" stroke="black" />
</svg>

<style>
  svg {
    overflow: visible;
    display: block;
  }
</style>
