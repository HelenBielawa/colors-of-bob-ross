<script>
    import ColorLine from "./ColorLine.svelte";
  import ColorLines from "./ColorLines.svelte";
    export let imgData;
    export let colorPalette;

    let showPic = false;

    function primaryColor(img){
        let colorData = JSON.parse(img.color_presence.replaceAll("'", '"'));
        let max = Object.values(colorData)
                        .reduce((max, obj) => {   
                            const objMax = Math.max(...Object.values(obj));
                            return Math.max(max, objMax);
                        }, 0);
        let maxColor = Object.keys(colorData)
                        .find(key => colorData[key] === max);
        return maxColor;
    }

    function findPic(index){
        return imgData.find(img => img.painting_index == showPic)
    }
    
</script>

    {#if showPic}
        <ColorLine {colorPalette} img={findPic(showPic)}/>
    {:else}
        <ColorLine {colorPalette} fill="null"/>
    {/if}
<div class="cellwrapper">

  {#each imgData as img}
    <div class="cell"
        style="background-color: {img.dominant_color};
                background-image: {showPic == img.painting_index? "url("+img.img_src+")" : "none"};"
        on:mouseover={() => (showPic = img.painting_index)}
        on:click={() => (showPic = img.painting_index)}
        on:focus={() => (showPic = img.painting_index)}
        on:mouseout={() => (showPic = false)}
        >
    </div>
  {/each}
</div>


<style>
    .cellwrapper{
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
    }
    .cell{
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;
      margin: 1px;
      border-radius:2%;
      transition: transform 0.2s ease;
    }
    .cell:hover {
    transform: scale(7);
    }
    @media (min-width: 350px) {
        .cell {
            width: 4vw;
            height: 3vw;
        }
        .cellwrapper{
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            width: 95%;
            max-height: 90%;
        }
    }
    @media (min-width: 768px) {
        .cell {
            width: 1.6vw;
            height: 1.2vw;
        }
        .cellwrapper{
            display: flex;
            flex-direction: row;
            flex-wrap: wrap;
            width: 60%;
            max-height: 90%;
        }
    }

</style>
