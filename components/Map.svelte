<script>
  import { geoAlbers, geoPath } from "d3"
  import { _ } from "lodash"

  export let geo
  export let data
  export let width = 1000

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

  $: path = geoPath(projection)
</script>

<svg x="-5000" {width} height="500">
  {#each dataCalc as d}
    <circle cx={d.lat} cy={d.long} r="5" />
  {/each}

  <path d={path(geo)} fill="none" stroke="black" />
</svg>

<style>
  svg {
    overflow: visible;
    display: block;
  }
</style>
