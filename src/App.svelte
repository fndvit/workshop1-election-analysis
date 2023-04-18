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
import dataVotes from "./data/fiveyears_votes.json"
import dataVotesLeftRight from "./data/political_alignment.json"

  import { onMount } from "svelte";


  let years = [];
  let main_parties = [];
  let buttons_parties=[];
  $: selected_schema = "interpolateWarm";
  // let selectedYear = 2015; 
  // let selectedParty = "ERC";

let selectedYear = ''; 
let selectedParty = "";

const multilineColors1 = ['#18307b', '#eedd00','#eb6109','#fdb94d','#ed5975','#aaa','#00ff7f','#4488cc','#ee0000','#912c45','#63be21'];
const multilineColors2 = ['#7b3294','#c2a5cf','#a6dba0','#008837'];


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
    buttons_parties=main_parties.filter((d)=>
    {
      //,'Ciutadans','Podem'
      return ['psc','vox','jxc','erc','cs','pp','cup'].indexOf(d.toLowerCase())>-1
  })

    main_parties = [...new Set(observable_data.map((d) => d.main_party))];
  console.log(main_parties)
    
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
  function getWhereWin(party,year)
  {

  /* let r = d3.nest()
  .key(function(d) { return d.municipality_code; })
  .entries(observable_data.filter(d=>d.year==year)); */

  // let r=observable_data.filter(d=>d.year==year).reduce(function(memo,data,i)
  //   {
  //     let pos=memo.munis.indexOf(data.municipality_code);
  //     if (pos==-1)
  //     {
  //       memo.winning.push({code:data.municipality_code,prop:data.voted_proportion,party:data.main_party});
  //       memo.munis.push(data.municipality_code)
  //     }
  //     else
  //     {
  //       for (var p in memo.winning)
  //       {
  //         if (memo.winning[p].municipality_code==data.municipality_code && data.voted_proportion>memo.winning[pos].prop)
  //         {
  //           memo.winning[pos].party=data.main_party;
  //           memo.winning[pos].prop=data.voted_proportion;
  //         }
  //       }
  //     }
  //   return memo
      
  //   },{munis:[],winning:[]})
    let r='test'
    console.warn(r)
    return r
    //r.winning.filter(d=>d.party===party)
  }
  function story(main_party,year)
      {
        //console.log(getWhereWin('PP',2019))
        console.log(arguments)
        //selectedParty=main_party;
          filtered_data=observable_data.filter((d2)=>
          {
            
            return (d2.main_party==main_party && d2.year==year)
          })
          jQuery('.maplibregl-ctrl-bottom-right').show();
          jQuery('.stories_container .story').hide();
          let story_div=jQuery('.'+main_party.toLowerCase()+'_story');

          story_div.find('h4').text(year);
          
          story_div.fadeIn('slow');

        
          let max_voted_features_party=observable_data.filter((d2)=>
          {
            
            return (d2.main_party==main_party && d2.year==year)
          }).sort((a,b)=>b.voted_proportion-a.voted_proportion).slice(0,5);
          console.log(max_voted_features_party)
          
          let html=max_voted_features_party.map(d=>{
            return '<li>'+d.municipality+':'+'<span class="voted_proportion">'+d.voted_proportion+'</span></li>'
          }).join('')
          html+='<span class="next_year">See 2019 results</span>'

          story_div.find('ul').empty().append(html)
          story_div.find('.next_year').on('click',function(){
            story(main_party,2019)
          })
    }
    getMainParties();
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
  </header>

  
  <!-- <Counter/> -->
    <h2>Introduction</h2>
    <p>The next 28th of May, people from 947 municipalities are summoned to the polls to choose who will govern each of the country's councils. </p>
    <br>

    <p>The municipal elections, which are held every four years, are the ones that have the most impact on a day-to-day basis, because they serve to choose the representatives who will have the responsibility of adopting the policies most linked to the citizenry. The parties face them not only with the desire to take on the maximum number of mayors and representatives possible, but also as a platform to consolidate their local power.</p>
    <br>
    
    <p>This project aims to explore a little bit the results of the recent years elections. We have used the last 5 years of information for all the municipalities.
      Although the data is fine, there was the need to include a new column about to which main party in Spanish/Catalan government all the little municipal parties are related to. 
      This is due to, in the municipalities, usually the main parties have a party there, but in some towns, the name is not a clear relation.
    </p>
 
    <h2>Exploration</h2>
 
    <p>With that said, let’s progress to discuss the data. </p>
    <p>First, in the chart below, there’s an evolution for the last 5 years of elections for each party. The exploration is done using the general names, rather than all the municipal parties.</p>
    <p>Let’s consider this 4 groups for the parties:</p>

      <dl>
        <dt><b>Right:</b> VOX, Ciutadans, PP</dt>
        <dt><b>Moderate right:</b> JxCat, CIU</dt>
        <dt><b>Moderate left:</b> ERC, PSC, PACMA</dt>
        <dt><b>Left:</b> CUP, Podem</dt>
      </dl>

    <MultiLineLayer inputData={dataVotesLeftRight}  colors={multilineColors2}/>



    <p>From this graph, we can conclude that in the vast majority of Catalonia, people vote more for left-wing parties. However, the correct matches have increased a bit.</p>
    <p>If we take a look at the evolution of each match, the graph is as follows.</p>
    
  
    <MultiLineLayer inputData={dataVotes} colors={multilineColors1}/>


    <p>We can see a party that has dropped a lot, which is the PSC, considered moderate left and the current party that governs in Spain alongside Podemos, which has dropped a little but is always between 200,000 and 300,000 votes.</p>
    <p>The Others category is also increased, which includes all the municipal parties that could not be related to the main ones. These are just local groups that exist only for municipal elections, usually with people from that place who want to lead change but not be associated with a major party. The main reason is usually the association with the ideals of one of the elders.</p>

    <br>

    <p>A curious case in the correct parties section is Vox & Ciutadans, both recently created (in the last 20 years) while others like PP or PSOE have been around since the 70s. They were new in 2003 and both are in 0 votes, but they got about 23,000 votes. Ciutadans has always won more votes than VOX. However, they are having an internal crisis at the moment, so this could change in the future. What we can detect from the graph is that while both are increasing votes, the PP (which is the other one on the right) is decreasing its numbers.</p>
    <p>Hopefully this means that the people who vote well stay on the right, but the total number of people who vote for them does not increase.</p>

    <br>

    <p>Finally, but not least, there is a notable increase in JxCat and ERC, although one moderate left and the other moderate right, they are, along with the CUP, the three parties that defend the independence of Catalonia. Last year all three together got 43% of the total votes.</p>
    <p>We can better see the gains and losses of councilors between 2015 and 2019.</p>

    <BarChartLayer/>

    <h2>Maps</h2>

    <p>After the exploratory analysis, below you’ll find a map that provides a deeper inside in the municipality's results.</p>
    <br>
    <p>TThe 2015 and 2019 results can be explored in more detail. There are stories that are easily seen in the face of the drastic increase (ERC/JxCAT) or decrease (PP/CUP) of votes.
      For cases like the PSC where the increase is more general, winning votes distributed among many municipalities, it allows us to see in more detail which these municipalities are.
    </p>
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
    
    
    <Map {filtered_data} {colorScale} />
    {#if filtered_data.length > 0}

    
    {#each buttons_parties as party}
    <StoryButton class="primary sm" on:click={()=>story({party}.party,2015)}>
      {party} story
    </StoryButton> 
   
   {/each} 

  <!--   <ScaleForm
      bind:selected_schema
      sel_schema="selected_schema"
      on:change={create_scale2(selected_schema, filtered_data)}
    /> -->
  {/if}
  

  <br>
  <br>
    <h2>Stats Analysis</h2>
    <p>Preliminary analysis of the stats was done using R and Observable</p>
    <a href="https://observablehq.com/d/7eb01f000c28ad2e">Observable notebook</a><br>
    <a href="https://rpubs.com/josephricafort/catalunyaelections">R stats</a>
    <br>
    <p>Finally, we want to thank Anton Bardera and Matt Osborn for their help with the development of this project </p>

</main>

<style>

  :global(.next_year)
  {
    margin-top: 5px;
    color:orange;
    cursor: pointer;
    
  }
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

:global(.story ul)
{
  list-style-type: none;
  padding: 5px;
}
:global(.stories_container)
{
  float: right;
  grid-template-columns: 1fr 1fr;
  grid-gap: 10px;
  padding: 10px;
  overflow-y: auto;
  width:135px;
    height: 300px;
    border: 1px solid grey;
}
:global(.stories_ctrl)
{
  max-width: 25%;
}
:global(.maplibregl-control-container,.maplibregl-ctrl-top-right)
{
  z-index: 99999;
}
:global(.maplibregl-ctrl-bottom-right)
{
  
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
