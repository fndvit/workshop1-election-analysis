<script>
   

   import { LayerCake, ScaledSvg, Html } from 'layercake';
  import { scaleBand } from 'd3-scale';

  import Bar from './lib/Bar.svelte';
  import AxisX from './lib/AxisX.svelte';
  import AxisY from './lib/AxisY.svelte';
   import BasicChart from "./lib/BasicChart.svelte"; 
  import Tooltip from "./lib/Tooltip.svelte";

  const xKey = 'value';
  const yKey = 'year';

  const data=[
    {
      "year": "2015",
      "value": 2
    },
    {
      "year": "2016",
      "value": 4
    },
    {
      "year": "2017",
      "value": 7
    },
    {
      "year": "2018",
      "value": 1
    },
    {
      "year": "2019",
      "value": 1
    }
  ]

  
 
</script>

<main>
  <header>
    <h1>Catalunya Municipal Elections</h1>
    <p>By Marina Rovira Boix, Joseph Ricafort, Pere Roca Ristol</p>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  </header>
  <div class="chart-container">
    <LayerCake
      ssr={true}
      percentRange={true}
      padding={{ top: 0, right: 20, bottom: 20, left: 35 }}
      x='value'
      y='year'
      yScale={scaleBand().paddingInner([0.05]).round(true)}
      yDomain={data.map(d=>d.year)}
      xDomain={[0, null]}
      data={data}
    >
      <Html>
        <AxisX
          gridlines={true}
          baseline={true}
          snapTicks={true}
        />
        <AxisY gridlines={false}/>
      </Html>
      <ScaledSvg>
        <BasicChart/>
      </ScaledSvg>
    </LayerCake>
  
  </div>
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
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam</p>
    <p>Quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  </section>
  <section class="stats-analyis">
    <h2>Stats Analysis</h2>
    <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam</p>
    <p>Quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
  </section>
</main>

<style>
section, header {
  max-width: 700px;
  margin: 0 auto;
}
.chart-container {
    width: 100%;
    height: 250px;
  }
</style>
