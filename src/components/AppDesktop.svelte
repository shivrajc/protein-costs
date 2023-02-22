<script>
  import { onMount } from 'svelte';
  import {csv} from 'd3';
  import Table from './Table.svelte';
  import Header from './Header.svelte';
  import Legend from './Legend.svelte';
  import Footer from './Footer.svelte';
  import ClearButton from './ClearButton.svelte';
  
  let dataset = [];
  let tags =  [];
  
  onMount(
		async () => {
			dataset = await csv('src/data/data.csv');     
      tags = [...new Set(dataset.map(d => d.tags))];
		}
	)  

  let vegetarian = false;
  let vegan = false;
  let nonVeg = false;
  let sortColumn = "cost_per_20_grams_of_protein";
  let sortOrder = "asc";

  $: dataFiltered = dataset.filter(d => vegetarian ? d.vegetarian === "T" : true)
    .filter(d => vegan ? d.vegan === "T" : true)
    .filter(d => nonVeg ? d.vegetarian !== "T" && d.vegan !== "T" : true) 
    .sort((a, b) => sortOrder === 'desc' ? b[sortColumn] - a[sortColumn] : a[sortColumn] - b[sortColumn]);

  $: tags = [...new Set(dataFiltered.map(d => d.tags))].map((tag, i) => {
              let id = tag === "vegan" ? 1 : tag === "vegetarian" ? 2 : 3;
              return {id, name:tag};
            }).sort((a, b) => a.id - b.id);

  function handleToggleClick(event) {
    if (event.detail.toggle === "vegetarian") {
      vegetarian = !vegetarian;
    } else if(event.detail.toggle === "vegan") {
      vegan = !vegan;
    } else {
      nonVeg = !nonVeg;
    }  
  }

  function handleSort(event) {    
      sortColumn = event.detail.column;
      sortOrder === 'desc' ? sortOrder = 'asc' : sortOrder = 'desc';
  }

  function clearFilters() {
    vegetarian = false;
    vegan = false;
    nonVeg = false;
  }

</script>

<main>
  <div class="main-container">
    <div class="viz-container" > 
      <div class="header">
        <Header text="Cheapest ways to get your Protein" subText="Cost per 20g of protein of various protein sources" />
      </div>

      <div class="table">
        <div class="filter-section">
          <Legend {tags} on:toggle={handleToggleClick}/>

          {#if vegetarian || vegan || nonVeg}
            <ClearButton on:click={clearFilters}/>            
          {/if}
        </div>
        <Table data={dataFiltered} on:toggle={handleToggleClick} on:sort={handleSort} {sortColumn} {sortOrder}/>
      </div>

    </div>
    <Footer />
  </div>

</main>

<style>

  .main-container {
    /* height: auto; */
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    height: 100vh;
    /* margin-top: 4px; */
  }
  
  
  .header {
    height: 110px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    width: 100%;
    background-color: hsl(210, 24%, 97%);
    border-bottom: 1px solid hsl(210, 28%, 84%);
    padding: 0 40px;    
  }
  
  .viz-container {
    /* background-color: rgba(255, 255, 255, 0.8); */
    box-shadow: hsla(240, 30%, 28%, 0.25) 0px 2px 5px -1px, hsla(0, 0%, 0%, 0.3) 0px 1px 3px -1px;
    border-radius: 12px; 
  }
  .table {
    background-color: hsla(0, 0%, 100%, 0.96);
    padding: 20px 40px 40px 40px;
    margin: 0 auto;
  }

  .filter-section {
    height: 30px;
    padding: 0 20px;
    display: flex;
    justify-content: space-between;    
    align-items: center;
    margin-bottom: 12px;
  }
</style>
