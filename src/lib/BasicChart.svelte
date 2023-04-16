<script>
  import { LayerCake } from 'layercake';
  import { getContext } from 'svelte';
  const { data, xGet, yGet, xScale, yScale }= getContext("LayerCake");
  // Define some data
  const points = [
    {x: 0, y: 0},
    {x: 5, y: 10},
    {x: 10, y: 20},
    {x: 15, y: 30},
    {x: 20, y: 40}
  ];
  export let fill = '#00bbff';
</script>
<g class="bar-group">
  {#each $data as d, i}
    <rect
      class='group-rect'
      data-id="{i}"
      x="{$xScale.range()[0]}"
      y="{$yGet(d)}"
      height={$yScale.bandwidth()}
      width="{$xGet(d)}"
      {fill}
    ></rect>
  {/each}
</g>
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

