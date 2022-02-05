<script>
  export let x = 0
  export let y = 0
  export let name
  import { afterUpdate } from "svelte"

  afterUpdate(() => {
    const canvas = document.getElementById("houses")
    const context = canvas.getContext("2d")

    var imageObj = new Image()
    imageObj.src = `/images/${name}.png`
    imageObj.onload = function () {
      context.drawImage(imageObj, x, y, 25, 25)
    }
  })
</script>

<style>
</style>
