<script>
    import { scaleLinear } from "d3-scale";
    export let colorPalette;
    export let colorCounts = null;
    export let img = null;
    export let fill = "all";

    let width = 400;
    $: circleRadius = width/15/2;

    let maxOccurrence = 0;

    $: if (colorCounts != null){
      maxOccurrence = Math.max(...Object.values(colorCounts))
    }

    $: radiusScale = scaleLinear()
      .domain([0, maxOccurrence])
      .range([0, circleRadius/2]);

</script>

<div class="circle-container" bind:clientWidth={width}>

    {#each colorPalette as color, i}
        {#if img != null}
        <svg width="{width}" height="{circleRadius*2}">
            <circle cx="{circleRadius/2}"
                    cy="{circleRadius/2}"
                    r="{colorCounts != null? radiusScale(colorCounts[color]): circleRadius/2}"
                    fill="{img.color_hex.includes(color) ? color : 'transparent'}"
                    stroke="{img.color_hex.includes(color) ? 'transparent' : '#111213'}" />
        </svg>
        {:else}
            <svg width="{width}" height="{circleRadius*2}">
            <circle cx="{circleRadius/2}"
                    cy="{circleRadius/2}"
                    r="{colorCounts != null? radiusScale(colorCounts[color]): circleRadius/2}"
                    fill="{fill == "all" ? color : 'transparent'}"
                    stroke="{fill == "all" ? 'transparent' : '#111213'}"/>
        </svg>
       {/if}
    {/each}

</div>

<style>
  .circle-container {
    display: flex;
    height: min-content;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: center;
    align-items: center;
  }
</style>
