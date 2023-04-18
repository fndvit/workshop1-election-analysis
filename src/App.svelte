<script>
  import BarChartLayer from './lib/BarChartLayer.svelte';
  import MultiLineLayer from './lib/MultiLineLayer/MultiLineLayer.svelte';
  import Map from "./lib/Map.svelte";
  import LateralMenu from "./lib/LateralMenu.svelte";
import * as d3 from "d3";
import jQuery from 'jquery';
import BasicStats from "./lib/BasicStats.svelte";
import ScaleForm from "./lib/ScaleForm.svelte";
import StoryButton from "./lib/StoryButton.svelte"
import { create_scale2 } from "./lib/BasicStats.svelte";
import { observable_data } from "./data/observable_data.js";

import { onMount } from "svelte";

let years = [];
let main_parties = [];
$: selected_schema = "interpolateWarm";
// let selectedYear = 2015; 
// let selectedParty = "ERC";

let selectedYear = ''; 
let selectedParty = "";

const createColorScale = (selected_schema, min, max) => {
  return d3.scaleSequential(d3[selected_schema]).domain([min, max]);
  //return d3.scaleSequential(d3[selected_schema]).domain([0, 100]);
};

const getYears = () => {

  years = [...new Set(observable_data.map((d) => d.year))];
  years = years.sort((a, b) => a - b);

};

const getMainParties = () => {
  /*    console.info(
    observable_data.map(d=>d.year)
  ) */
  main_parties = [...new Set(observable_data.map((d) => d.main_party))];
  
  //main_parties = main_parties.sort((a, b) => a - b);
};

$: filtered_data = observable_data.filter(
  (d) => d.year == parseInt(selectedYear) && d.main_party == selectedParty
);

$: f = filtered_data
  .filter((d) => d.voted_proportion > 0)
  .map((d) => d.voted_proportion);

  
$: stats = {
  min: d3.min(f),
  max: d3.max(f),
  avg: d3.mean(f),
};
$:{
  console.info(filtered_data)
  console.warn(f,stats)

}
$: colorScale = createColorScale(selected_schema, stats.min, stats.max);

$: console.log(filtered_data);

//not dynamic when clicking on story buttons..
/*
$:max_voted_features_party=observable_data.filter((d2)=>
  {
    
    return (d2.main_party==selectedParty && d2.year==selectedYear)
  }).sort((a,b)=>b.voted_proportion-a.voted_proportion).slice(0,5);
  */
function story(main_party,year)
    {
      //selectedParty=main_party;
        filtered_data=observable_data.filter((d2)=>
        {
          
          return (d2.main_party==main_party && d2.year==year)
        })
        jQuery('.maplibregl-ctrl-bottom-right').show();
        jQuery('.stories_container .story').hide();
        let story_div=jQuery('.'+main_party.toLowerCase()+'_story')
        story_div.fadeIn('slow');
        let max_voted_features_party=observable_data.filter((d2)=>
        {
          
          return (d2.main_party==main_party && d2.year==year)
        }).sort((a,b)=>b.voted_proportion-a.voted_proportion).slice(0,5);
        console.log(max_voted_features_party)
        let html='<h3>Max voted proportion municipalities</h3>';
        html+=max_voted_features_party.map(d=>{
          return '<li>'+d.municipality+':'+'<span class="voted_proportion">'+d.voted_proportion+'</span></li>'
        }).join('')

        story_div.find('ul').empty().append(html)
  }
onMount(() => {
  getYears();
  getMainParties();

  setTimeout(() => {
    selectedYear = 2015;
    selectedParty = "ERC";
  }, 500);

  
  
});
</script>
<main>
  <header>
    <h1>Catalunya Municipal Elections</h1>
    <p>By Marina Rovira Boix, Joseph Ricafort, Pere Roca Ristol</p>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  </header>

  <BarChartLayer/>
  
  <MultiLineLayer />

  <!-- <Counter/> -->
  <section class="introduction">
    <h2>Introduction</h2>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam</p>
    <p>Quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  </section>
  <section class="charts">
    <h2>Charts</h2>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam</p>
    <p>Quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  </section>
  <section class="maps">
    <h2>Maps</h2>
    <div class="forms">
     <LateralMenu bind:selectedYear {years} />
     
      {#if selectedYear}
       
        <LateralMenu bind:selectedParty {main_parties} />
      {/if}
    </div>
      {#if selectedParty && selectedYear && selected_schema}
        <BasicStats
          {filtered_data}
          sel_schema="selected_schema"
          {colorScale}
          {stats}
        />
        
       
      {/if} 
    
    
    <Map {filtered_data} {colorScale}  />
    {#if filtered_data.length > 0}
    <StoryButton class="primary sm" on:click={()=>story('PSC',2019)}>
      PSC story, 2019
    </StoryButton>

    <StoryButton class="primary sm" on:click={()=>story('VOX',2019)}>
      VOX story
    </StoryButton>

    <ScaleForm
      bind:selected_schema
      sel_schema="selected_schema"
      on:change={create_scale2(selected_schema, filtered_data)}
    />
  {/if}
  

  
  </section>
  <section class="stats-analyis">
    <h2>Stats Analysis</h2>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam</p>
    <p>Quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  </section>
</main>

<style>
      .forms
    {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-gap: 10px;
        padding: 10px;
    }
    :global(.maplibregl-control-container) {
    z-index: 99999999;
    
}
:global(ul)
{
  list-style-type: none;
}
:global(.stories_container)
{
  float: right;
  grid-template-columns: 1fr 1fr;
  grid-gap: 10px;
  padding: 10px;
  overflow-y: auto;
    height: 300px;
    border: 1px solid grey;
}
:global(.stories_ctrl)
{
  max-width: 25%;
}

:global(.maplibregl-ctrl-bottom-right)
{
  z-index: 99999;
    background: #242424;
    display: none;
    max-width: 25%;
} 

:global(.stories_container .story)
{
  display: none;
}
  :global(.scale_container path.domain),
  :global(.scale_container line) {
    opacity: 0;
  }
  section {
    padding: 10px;
    margin: 10px;
    color: white;
  }
:global(.mapboxgl-popup), :global(.maplibregl-popup) 
{
	color:black!important;
	z-index: 111111111!important;
}
section, header {
  max-width: 700px;
  margin: 0 auto;
}
:global(.maplibregl-ctrl-bottom-right)
{
  z-index: 99999;
    background: #242424;
}

</style>
