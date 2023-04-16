<script>
  // @ts-ignore
  import { LayerCake, ScaledSvg, Html } from 'layercake';
  // @ts-ignore
  import { scaleBand, scaleLinear } from 'd3-scale';
  import { max } from 'd3-array';

  // @ts-ignore
  import Bar from './Bar.svelte';
  import AxisX from './AxisX.svelte';
  import AxisY from './AxisY.svelte';
  import BasicChart from "./BasicChart.svelte"; 
  //import data from "../../data/fiveyears_tally.json";


  const data= [
    {
      "main_party":"ERC",
      "gained": 64,
      "lost": 22,
      "no_change": 567,
      "total": 631
    },
    {
      "main_party":"PSC",
      "gained": 22,
      "lost": 40,
      "no_change": 384,
      "total": 406
    },
    {
      "main_party":"Ciutadans",
      "gained": 0,
      "lost": 4,
      "no_change": 79,
      "total": 79
    },
    {
      "main_party":"Others",
      "gained": 21,
      "lost": 35,
      "no_change": 212,
      "total": 233
    },
    {
      "main_party":"PP",
      "gained": 3,
      "lost": 0,
      "no_change": 222,
      "total": 225
    },
    {
      "main_party":"JxC",
      "gained": 2,
      "lost": 0,
      "no_change": 12,
      "total": 14
    },
    {
      "main_party":"CIU",
      "gained": 3,
      "lost": 5,
      "no_change": 15,
      "total": 18
    },
    {
      "main_party":"Podem",
      "gained": 1,
      "lost": 9,
      "no_change": 45,
      "total": 46
    },
    {
      "main_party":"CUP",
      "gained": 0,
      "lost": 0,
      "no_change": 5,
      "total": 5
    },
    {
      "main_party":"PACMA",
      "gained": 0,
      "lost": 0,
      "no_change": 3,
      "total": 3
    },
  ]

  const xKey = 'main_party';
  const yKey = 'total';
  
  </script>
  
  <div class="chart-container">
    <LayerCake
      ssr={true}
      percentRange={true}
      padding={{ top: 0, right: 20, bottom: 20, left: 35 }}
      x='main_party'
      y='total'
      xScale={scaleBand().paddingInner([0.05]).round(true)}
      yScale={scaleLinear().nice()}
      yDomain={[0, max(data, d => d.total)]}
      xDomain={data.map(d=>d.main_party)}
      data={data}
    >
      <Html>
        <AxisX
          gridlines={false}
        />
        <AxisY 
        baseline={true}
        gridlines={true}/>
      </Html>
      <ScaledSvg>
        <BasicChart/>
      </ScaledSvg>
    </LayerCake>
  
  </div>

  <style>
    /*
    The wrapper div needs to have an explicit width and height in CSS.
    It can also be a flexbox child or CSS grid element.
    The point being it needs dimensions since the <LayerCake> element will
    expand to fill it.
  */
  .chart-container {
    width: 100%;
    height: 300px;
  }
</style>