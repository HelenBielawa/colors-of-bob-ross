<script>
    export let colorPalette;
    export let seasonData;
    $: console.log("seasondata:", Object.entries(seasonData))
    import { scaleLinear } from "d3-scale";
    import SeasonTooltip from "./SeasonTooltip.svelte";

    let height;

    $: circleBoxSize = height/31;

    var tooltipStatus = false;
    var tooltipColor = "";
    var tooltipSeason;
    var tooltipX;
    var tooltipY;

    const maxOccurrence = Object.values(seasonData)
                                .reduce((max, obj) => {   
                                  const objMax = Math.max(...Object.values(obj));
                                  return Math.max(max, objMax);
                                }, 0);

    $: radiusScale = scaleLinear()
      .domain([0, maxOccurrence])
      .range([0, circleBoxSize/2]);

  //manually created list of hex codes which represent the colors of their group
  //mixes created with: https://colordesigner.io/color-mixer
  //based on aestehtics, not on exact percentages
  const colorGroupsHex = {"white": "#FFFFFF",
                          "black": "000000",
                          "brown": "#682716",
                          "red":  "DB0000",
                          "yellow": "#ecc000",
                          "green": "#0b3219",
                          "blue": "#070f44"}

</script>

<div class="lines-container" bind:clientHeight={height}>

  {#each seasonData as season, index}
      
    <div class="circle-container">
        {#each colorPalette as color}
            <svg width="{circleBoxSize}"
                height="{circleBoxSize}"
                class="circle-svg"
                role="img"
                on:mouseover={(event) => (tooltipStatus = true,
                                      tooltipColor = color,
                                      tooltipSeason = index+1,
                                      tooltipX = event.clientX,
                                      tooltipY = event.clientY)}
                on:focus={(event) => (tooltipStatus = true,
                                      tooltipColor = color,
                                      tooltipSeason = index+1)}
                on:mouseout={() => tooltipStatus = false}
                on:blur={() => tooltipStatus = false}
                >
                <circle cx="{circleBoxSize/2}"
                      cy="{circleBoxSize/2}"
                        r="{radiusScale(season[color])}"
                        fill="{colorPalette.length < 15? colorGroupsHex[color]: color}"></circle>
            </svg>


        {/each}
    </div>
 
  {/each}

  {#if tooltipStatus && colorPalette.length == 15}
     <SeasonTooltip season={tooltipSeason} colorHex={tooltipColor} {tooltipX} {tooltipY}/>
  {/if}

</div>

<style>

  .lines-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
   }
  .circle-container {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: center;
    align-items: center;
  }


</style>
