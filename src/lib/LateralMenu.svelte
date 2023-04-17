
<script>
  import { utcYears } from "d3";

export let years;
export let selectedYear;
export let main_parties;
console.log(main_parties)

export let selectedParty;
</script>

    
    {#if years && years.length>0}
    <div class="yearMenu">
    <select class="menu" 
					name="menu" 
					id="menu" 
                    autocomplete="off"
					bind:value={selectedYear}>
                    <option disabled selected value="">Select a year</option>
                        {#each years as year}
                             {#if year===2015}
                                <option selected value={year}>{year}</option>
                                {/if}
                                {#if year!==2015}
                                <option  value={year}>{year}</option>
                                {/if}
                        {/each}
                        </select>
                       <!--  {#if selectedYear}
                            <p>Selected year: {selectedYear}</p>
                        {/if} -->
    </div>     
                
    {/if} 
     {#if main_parties && main_parties.length>0} 
     <div class="partymenu">
        <select class="menu" 
        name="Partymenu" 
        id="Partymenu" 
        autocomplete="off"
        bind:value={selectedParty}>
        <option disabled selected value="">Select a party</option>
            {#each main_parties as party}
             {#if party==='ERC'}
            <option selected value={party}>{party}</option>
            {/if}
            {#if party!=='ERC'}
            <option  value={party}>{party}</option>
            {/if}
            
            {/each}
        </select>

       <!--  {#if selectedParty}
            <p>Selected main party: {selectedParty}</p>
        {/if} -->
    </div>
    {/if} 



<style>

div
{
    /* display:'inline-grid'; */
    padding: 5px;
    /* width: 100%; */
    
    color: white;
}
</style>