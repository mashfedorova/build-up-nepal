<script>
  import House from "./House.svelte"

  export let houses
  export let width

  $: houseWidth = 30
  $: perRow = Math.floor((width - 60) / houseWidth)
  $: canvasHeight = (Math.ceil(houses.length / perRow) + 0.5) * houseWidth
</script>

<canvas id="houses" {width} height={canvasHeight}>
  {#each houses as d, i}
    <House
      {houses}
      x={((i % perRow) + 0.5) * houseWidth}
      y={(Math.floor(i / perRow) + 0.5) * houseWidth}
      name={d}
      {width}
      {canvasHeight}
    />
  {/each}
</canvas>

<style>
  #houses {
    margin: 0 auto;
    display: block;
  }
</style>
