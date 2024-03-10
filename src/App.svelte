<script>
  import data from './data/paintings_with_dominant_color.json';
  import colorCounts from './data/color_counts.json';
  import seasonColors from './data/season_colors.json';
  import seasonColorsGrouped from './data/season_colors_grouped.json';

  import Scrolly from './helpers/Scrolly.svelte';

  import ColorLine from './components/ColorLine.svelte';
  import ColorLines from './components/ColorLines.svelte';
  import Raster from './components/Raster.svelte';


  //color palette with my own order and without doubles for blacks and whites
  const colorPalette = ['#FFFFFF', '#000000', '#221B15','#4E1500', '#5F2E1F', '#8A3324', '#CD5C5C', '#DB0000', '#FFB800', '#FFEC00', '#C79B00', '#0A3410', '#102E3C', '#021E44', '#0C0040']
  const colorGroups = ["white", "black", "brown", "red", "yellow", "green", "blue"];

  //manipulate the Color Counts Data to match requirement of the component
  const colorCountObject = colorCounts.reduce((acc, color) => {
                                    const { color_hex, color_count } = color;
                                    acc[color_hex] = (acc[color_hex] || 0) + color_count;
                                    return acc;
                                  }, {});
          
  let currentStep = 0;

  //text content for Scrolly Boxes
    const steps = [
        "<strong>Die Farben von Bob Ross</strong>",

        "In seiner Serie 'The Joy of Painting' hat Ross in jeder Episode ein Bild gemalt und das Publikum angeleitet, mitzumachen. In einzelnen Folgen malte er nicht selbst, sondern lud sich einen Gast ein. So sind in 31 Staffeln insgesamt 403 Bilder entstanden. Dabei hat Ross insgesamt weniger Farben genutzt, als in jedem handelsüblichen Wasserfarbkasten enthalten sind.",

        "<span style='position: relative; vertical-align: middle;'>Mit einigen Farben malte Ross sehr selten, andere kommen in fast jedem Bild vor. Das wird deutlich, wenn man zählt, wie oft welche Farbe in den Episoden von 'The Joy of Painting' genutzt wurde. Ross malte nur 55 Mal mit dem Braunton 'Burnt Umber' <span style='display: inline-block; width: 20px; height: 20px; border-radius: 50%; background-color: #8A3324;'></span>. Mit dem hellen Pink 'Indian Red' <span style='display: inline-block; width: 20px; height: 20px; border-radius: 50%; background-color: #CD5C5C;'></span> malte Ross sogar nur ein einziges Mal. Im Gesamtvergleich verschwindet es.</span>",

        "In der ersten Staffel von 'The Joy of Painting' startete Ross mit diesen neun Farben. Klick auf die Farbtupfer, um zu sehen, wie oft und für welche Bilder Ross sie in dieser Staffel genutzt hat.",
        
        "<span style='position: relative; vertical-align: middle;'>In der zweiten Staffel kam mit 'Burnt Umber' <span style='display: inline-block; width: 20px; height: 20px; border-radius: 50%; background-color: #8A3324;'></span> ein zweiter Braunton dazu, und zwei Gelbvarianten <span style='display: inline-block; width: 20px; height: 20px; border-radius: 50%; background-color: #C79B00;'></span> <span style='display: inline-block; width: 20px; height: 20px; border-radius: 50%; background-color: #FFB800;'></span>.</span>",

        "<span style='position: relative; vertical-align: middle;'>Ab der siebten Staffel (unterste Palette) tauschte Ross 'Burnt Umber' durch einen anderen Braunton aus: Ab dann war regelmäßig ein Klecks 'Dark Sienna' <span style='display: inline-block; width: 20px; height: 20px; border-radius: 50%; background-color: #4E1500;'></span> auf seiner Farbpalette.</span>",

        "Im Gesamtüberblick wirken schwarz und weiß sehr präsent, schließlich hat Ross fast immer mindestens einen Schwarz- oder Weißton als Grundierung genutzt. Aber wenn wir auch die anderen Farben gruppieren...",
        "...sehen wir, dass braune, gelbe und blaue Töne viel präsenter sind.",

        "Durch die Schichtung von Farben und die Arbeit auf der Leinwand haben die Bilder natürlich eine höhere Farbvarianz als 15 Farben. Die folgende Ansicht zeigt alle 403 Bilder aus 'The Joy of Painting' jeweils in der Farbe, die im Bild dominant ist. Klick auf ein einzelnes Bild, um in der Palette zu sehen, mit welchen Farben Ross (oder einer seiner Gäste) es gemalt hat."
    ];

</script>
  <!--Change the Dataviz in the background based on Scrolly Step-->
  <div class="scrolly-background">
    {#if currentStep == 0}
      <div class="spacer"/>
      <ColorLine {colorPalette}/>
    {:else if currentStep == 1}
      <div class="spacer"/>
      <ColorLine {colorPalette}/>
    {:else if currentStep == 2}
      <div class="spacer"/>
      <ColorLine {colorPalette} colorCounts={colorCountObject}/>
    {:else if currentStep == 3}
      <ColorLines {colorPalette} seasonData={[seasonColors["1"]]}/>
    {:else if currentStep == 4}
      <ColorLines {colorPalette} seasonData={[seasonColors["1"], seasonColors["2"]]}/>
    {:else if currentStep == 5}
      <ColorLines {colorPalette} seasonData={["1","2", "3", "4", "5", "6", "7"].map(i => seasonColors[i])}/>
    {:else if currentStep ==6 }
      <ColorLines {colorPalette} seasonData={Object.entries(seasonColors).map(s => s[1])}/>
    {:else if currentStep == 7}
      <ColorLines colorPalette={colorGroups} seasonData={Object.entries(seasonColorsGrouped).map(s => s[1])}/>
    {:else if currentStep == 8}

    {/if}
  </div>

  <!--Change the text in the foreground based on Scrolly step-->
  <Scrolly bind:value={currentStep}>
    {#each steps as text, i}
      <div class="step" class:active={currentStep === i}>
        <div class="step-content">
          {@html text}
        </div>
      </div>
    {/each}
    </Scrolly>

  <!--At the end of the Scroller, show the interactive raster with all pictures-->
  <main>
    <Raster imgData={data} {colorPalette}/>
  </main>
 
<style>
	.spacer {
		height: 20vh;
	}
  .step {
    position: relative;
    height: 90vh;
    width: 0;
    margin: 5%;
    opacity: 1;
    transition: opacity 300ms ease;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: 2;
  }

  .step-content {
    background: #FFFFFF;
    padding: 1rem 1rem;
    border-radius: 5px;
    width: 50vw;
    height: min-content;
    color: black;
    opacity: 0.94;
    box-shadow: 0px 0px 1px 0px rgba(227, 226, 226, 0.75);
    transition: transform 0.1s ease;
  }
  .step.active {
    opacity: 1;
  }
  .scrolly-background {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    background: #f4f1e9;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
  }
</style>