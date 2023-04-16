<script>
  import { LayerCake, ScaledSvg, Html } from 'layercake';
  import { scaleBand } from 'd3-scale';

  import Bar from './Bar.svelte';
  import AxisX from './AxisX.svelte';
  import AxisY from './AxisY.svelte';
  import BasicChart from "./BasicChart.svelte"; 

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