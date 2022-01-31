<!-- <svelte:head>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.6.1/gsap.min.js"></script>
<script src="https://scdnjs.cloudflare.com/ajax/libs/gsap/3.6.1/ScrollTrigger.min.js"></script>
  <script src=".scripts/DrawSVGPlugin.min.js"></script>
</svelte:head> -->
<script>
  import { gsap } from "gsap"
  import { ScrollTrigger } from "gsap/ScrollTrigger"
  import { onMount } from "svelte"
  import { json, csv } from "d3"
  import Map from "./../components/Map.svelte"
  import MapCanvass from "./../components/MapCanvass.svelte"
  gsap.registerPlugin(ScrollTrigger)

  let geo = []
  let data = []

  let width = document.body.clientWidth
  $: widthChart = width > 900 ? 800 : width - 100
  $: console.log(width)

  function resize() {
    width = document.body.clientWidth
  }

  onMount(resize)

  onMount(async () => {
    const shapesNepal = await json("data/oknp_admin1.json", (d) => {
      return {
        ...d,
      }
    })
    geo = shapesNepal

    const raw = await csv("data/nepal.csv", (d) => {
      return {
        ...d,
      }
    })
    data = raw
  })

  // $: console.log(data)
  onMount(() => {
    gsap.to("#wobble", {
      xPercent: -50,
      yPercent: -100,
      // ease: "power2.out",
      // duration: 10,
      scrollTrigger: {
        trigger: "#svgIntro",
        // pin: true, // pin the trigger element while active
        start: "93% 99%", // when the center of the trigger hits 40% from the top of the viewport
        end: "+=1000", // end after scrolling 1000px beyond the start
        scrub: 1, // smooth scrubbing, takes 1 second to "catch up" to the scrollbar
        markers: true,
      },
    })

    gsap.to("#main-title", {
      // xPercent: -200,
      yPercent: -700,
      ease: "power2.out",
      // duration: 10,
      scrollTrigger: {
        trigger: "#svgIntro",
        // pin: true, // pin the trigger element while active
        start: "93% 99%", // when the center of the trigger hits 40% from the top of the viewport
        end: "+=1000", // end after scrolling 1000px beyond the start
        scrub: 1, // smooth scrubbing, takes 1 second to "catch up" to the scrollbar
        markers: true,
      },
    })

    gsap.to("#subtitle", {
      // xPercent: -200,
      yPercent: -2000,
      // delay: 0.5,
      ease: "power2.out",
      // duration: 10,
      scrollTrigger: {
        trigger: "#svgIntro",
        // pin: true, // pin the trigger element while active
        start: "93% 99%", // when the center of the trigger hits 40% from the top of the viewport
        end: "+=1000", // end after scrolling 1000px beyond the start
        scrub: 1, // smooth scrubbing, takes 1 second to "catch up" to the scrollbar
        markers: true,
      },
    })
    gsap.to("#logo-title", {
      // xPercent: -200,
      yPercent: -1500,
      // delay: 0.5,
      // ease: "power2.out",
      // duration: 10,
      scrollTrigger: {
        trigger: "#svgIntro",
        // pin: true, // pin the trigger element while active
        start: "93% 99%", // when the center of the trigger hits 40% from the top of the viewport
        end: "+=1000", // end after scrolling 1000px beyond the start
        scrub: 1, // smooth scrubbing, takes 1 second to "catch up" to the scrollbar
        markers: true,
      },
    })
  })
</script>

<svelte:window on:resize={resize} />
<div class="intro" bind:this={width}>
  <svg id="svgIntro" width="100%">
    <!-- <svg id="ourSVG" height="100%" width="100%"> -->

    <!-- <mask id="mask-image-2012">
    <path
      id="wobble-2012-image"
      d="M511.6,30.4c12.9,6.8,100.3,54.2,114.9,154c10.2,69.7-14.1,147.8-73.4,190.3c-61,43.7-122.3,47.5-242.3,45.1
	C191.6,417.4,121.6,416,65,362c-6.5-6.2-78.2-77.4-62.7-176.5C16,97.8,88.1,51.9,109,40c13.6-7.8,41.2-21.5,202-25
	C420.4,12.6,475.9,11.6,511.6,30.4z"
      fill="red"
    />
  </mask> -->
    <mask id="mask">
      <!-- <g transform="translate(500,150)"> -->
      <!-- <path
      id="wobble"
      d="M53.48-414.83C-36.82-373-33.76-278.17-19.91-194c12.17,74,66.56,136.46,2.05,205.61s-109.6,66-153.69,147.16,31,143.15-55,230.15-274,14.62-267,206S-750.66,726.14-680.2,819.76c75,99.68,487.9,207.62,632,282.55,342.19,177.9,764,148.24,902.86-42.68C994.14,867.89,1031.26,327.21,819.11-35.12,647.45-328.3,300.19-406.44,276.48-428.27,223.48-477.07,92.55-432.94,53.48-414.83Z"
      fill="white"
    /> -->
      <path
        id="wobble"
        d="M4167.3,866.6l-45.7-2.6c-45.5-2.1-137.2-8.2-219.1,13.3c-82.2,20.7-154.6,68.9-231.7,103.9
	c-76.8,35.8-158.4,58.3-239.4,82.3c-81,23.9-162.6,46.5-237.7,87.2c-75.3,40.1-144.6,97-226.7,118.1c-82.1,21.1-176.8,6.4-259.7,25
	c-82.7,19.2-154.1,70.4-232.6,101.5c-78.5,31.1-165.1,39.3-241.9,75.1c-77.1,35-144.4,97.6-221.6,132.5
	c-76.8,35.8-163.5,44-242.8,72.7c-79.7,27.8-152.1,76.1-234.3,96.7c-81.9,21.5-173.6,15.4-253.8,41.8
	c-80.4,25.8-149.7,82.7-226.7,118.1c-77,35.4-161.6,49.3-241.9,75.1c-80.2,26.4-156.6,63.2-237.7,87.2s-167.7,32.1-246.2,63.3
	c-78.5,31.1-149.9,82.3-224.2,125.2c-74.6,42.1-152.1,76.1-231.7,103.9c-79.4,28.7-160.9,51.2-239.4,82.3s-155,67.9-233.5,99
	c-78.5,31.1-160,53.6-240.2,80c-80.4,25.8-159.9,54-239.4,82.3c-79.5,28.2-159,56.5-244.5,68c-85.3,12.1-177,5.9-222.5,3.9l-6.3-0.4
	c-24.4-1.4-43.3-13.2-49.6-30.9l-1382.7,32l-1557.3-1782.4C-5077.8-460.3-971.7-7042.1,3974-1657.5l707.9,1955.2L4167.3,866.6z"
        transform="translate(500,150)"
        fill="white"
      />
      <!-- </g> -->
    </mask>
    <!-- <Map {geo} {data} width={widthChart} /> -->

    <image
      href="images/3I1A9619.jpeg"
      width="100%"
      height="100%"
      preserveAspectRatio="xMidYMid slice"
      mask="url(#mask)"
    />

    <!-- <text id="main-title" x="23%" y="20%" mask="url(#mask)">Build-up Nepal</text
    > -->
    <!-- <svg id="svgTitle" x="23%" y="22%" width="100" height="100" mask="url(#mask)"> -->
    <svg id="svgTitle" x="23%" y="15%" overflow="visible">
      <text id="main-title" x="0%" y="65px">Build-up Nepal</text>
      <text id="subtitle" x="40px" y="95px">an interactive story</text>
      <image
        id="logo-title"
        href="images/logoCropped.png"
        x="-80px"
        y="1px"
        width="75px"
      />
    </svg>
  </svg>
  <!-- <Map {geo} {data} width={widthChart} /> -->
  <div class="canvas" bind:this={width}>
    <MapCanvass {geo} {data} width={widthChart} />
  </div>
</div>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>
<p>
  Lorem ipsum dolor sit, amet consectetur adipisicing elit. Maxime, magnam
  cumque illum esse inventore id quidem provident, quae error possimus a
  repudiandae qui eveniet tempora natus necessitatibus nam obcaecati totam ad
  quisquam architecto rerum quos? Est adipisci, nostrum ipsa asperiores enim
  odit accusamus nam repellat optio eum assumenda vitae hic porro qui molestias
  laborum similique doloremque illo quos perferendis voluptatum esse quidem
  quasi. Libero aut commodi exercitationem, minus animi totam sunt voluptatibus
  reiciendis eius similique quis assumenda quod eligendi debitis in pariatur
  nobis facilis blanditiis illum harum a laudantium quas ab? Quo voluptatibus
  quia labore eos, totam temporibus est veniam.
</p>

<style>
  @import url("https://fonts.googleapis.com/css2?family=Playfair+Display:wght@900&display=swap");
  @import url("https://fonts.googleapis.com/css2?family=Alegreya+Sans&display=swap");
  @import url("https://fonts.googleapis.com/css2?family=Roboto:wght@900&display=swap");

  /* svg {
    overflow: visible;
  } */
  #svgIntro {
    position: absolute;
    overflow: visible;
    display: inline-block;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    height: 110vh;
    margin-bottom: 500px;
  }

  /* #svgTitle {
    display: inline-block;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
  } */

  #main-title {
    font-family: "Playfair Display", serif;
    /* font-family: "Bree Serif", serif; */
    font-family: "Roboto", sans-serif;
    font-size: 5rem;
    fill: rgba(194, 85, 68, 0.795);
    letter-spacing: 5px;
  }

  #subtitle {
    /* font-family: "Alegreya Sans", sans-serif; */
    font-family: "Montserrat", sans-serif;
    letter-spacing: 0.2rem;
    font-weight: 600;
    font-size: 1.2rem;
    fill: rgb(238, 220, 211);
  }

  /* .intro {
    margin: 0 auto;
    width: 50%;
  } */

  .canvas {
    margin: 0 auto;
    width: 90%;
    display: flex;
    justify-content: center;
  }
</style>
