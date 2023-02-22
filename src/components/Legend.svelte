<script>
    import { createEventDispatcher } from "svelte";
    import Dot from "./Dot.svelte";

    const dispatch = createEventDispatcher();

    function handleToggleClick(toggle) {
        dispatch('toggle', {toggle});
    }
    export let tags;
</script>

<div class="legend">
    {#each tags as tag}
      {#if tag.name === "vegetarian"}          
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <div class="legend-item" on:click={() => handleToggleClick("vegetarian")}>
            <Dot type="stripes" width={14} color="mediumseagreen"/>
            <span>{tag.name}</span>
        </div>
        
      {/if}
      {#if tag.name === "vegan"}  
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <div class="legend-item" on:click={() => handleToggleClick("vegan")}>            
            <Dot type="solid" width={14} color="mediumseagreen"/>
            <span>{tag.name}</span>
        </div>                
      {/if}
      {#if tag.name === "non-vegetarian"}     
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <div class="legend-item" on:click={() => handleToggleClick("non-vegetarian")} >            
            <Dot type="stripes" width={14} color="lightsalmon"/>
            <span>{tag.name}</span>
        </div>            
      {/if}
    {/each}
</div>

<style>
    .legend {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        gap: 40px;
    }

    .legend-item {
        cursor: pointer;
    }

    .legend-item span {
        margin-left: 2px;
    }
</style>