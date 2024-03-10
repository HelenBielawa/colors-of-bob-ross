<script>
    import imgData from "../data/paintings_with_dominant_color.json";
    import colorData from "../data/color_counts.json";
  import { style } from "d3";

    export let season; //number of the season
    export let colorHex; //hex code of the hovered color
    export let tooltipX; //event.clientX
    export let tooltipY; //event.clientY
    console.log("season:", season)

    let tooltipWidth;
    let tooltipHeight;
    //translate hex code to color name
    const colorName = colorData.filter(color =>
                                        color.color_hex === colorHex)[0].color_name;
    //get all data for img with the hovered color in the hovered season
    const tooltipData = imgData.filter(img =>
                                        img.season == season && img.color_hex.includes(colorHex));

    //number of img with the hovered color in the hovered season
    const imgCount = tooltipData.length;
    const imgPercentage = Math.round(
                            (imgCount / imgData.filter(img => img.season == season)
                            .length)* 100);

    //change the img in the tooltip every second
    let i = 0;
	setInterval(() => {
        i += 1;
        if (i >= tooltipData.length) {
            i = 0;
        }
	}, 1000);
    $: curr_img = tooltipData[i];


</script>

<div class="tooltip"
    bind:clientHeight={tooltipHeight}
    bind:clientWidth={tooltipWidth}
    style="background:white;top: {tooltipY}px; left: {tooltipX}px">

    <p>
        <b>{season}. Staffel</b>: 
        {imgCount} Bilder mit <b>{colorName}</b> gemalt 
        ({imgPercentage} Prozent dieser Staffel)<br>
    </p>

    {#if tooltipData.length > 0}
        <img src={curr_img.img_src} alt={curr_img.painting_title} />
        <br>
    {/if}
   
</div>

<style>
    .tooltip {
        position: fixed;
        box-shadow: rgba(0, 0, 0, 0.15) 2px 3px 8px;
        border-radius: 1px;
        pointer-events: none;
        height: auto;
        transition: transform 0.1s ease;
        background-image: linear-gradient(to bottom right, #ffffff, #eaeaea);
        padding: 2px;
        transform: translate(-50%, 0);
    }
    .tooltip img {
        width: 90%;
        height: auto;
        object-fit: contain;
    }
    .tooltip p {
        font-size: smaller;
        width: 90%;
    }
    @media (min-width: 350px) {
    .tooltip {
        width: 40%;
    }
    }
    @media (min-width: 768px) {
    .tooltip {
        width: 15%;
    }
    }

</style>