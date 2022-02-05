<script>
  import { Canvas } from "svelte-canvas"
  import Squares from "./Squares.svelte"

  export let trees
  export let width = 1000

  $: pathWidth = 50
  $: perRow = Math.floor((width - 10) / pathWidth)
  $: svgHeight = (Math.ceil(trees.length / perRow) + 0.5) * pathWidth
</script>

<Canvas {width} height={svgHeight}>
  {#each trees as d, i}
    <Squares
      {trees}
      x={((i % perRow) + 0.2) * pathWidth}
      y={(Math.floor(i / perRow) + 0.5) * pathWidth}
      name={d}
    />
  {/each}
</Canvas>

<style>
</style>
