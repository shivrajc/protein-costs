<script>
    import { createEventDispatcher } from "svelte";
    import Bar from "./Bar.svelte";
    import Dot from "./Dot.svelte";
    import {scaleLinear, max, format} from 'd3';

    export let data;
    export let sortColumn;
    export let sortOrder;

    $: sortOrderLabel = sortOrder === 'desc' ? '▼' : '▲';

    let hoveredRow;
    const dispatch = createEventDispatcher();

    const maxBarWidth = 380;
    const numberFormat = format(".2f");

    $: xScale = scaleLinear()
        .domain([0, max(data, d => d.cost_per_20_grams_of_protein)])
        .range([0, maxBarWidth]);

    function handleToggleClick(toggle) {
        dispatch('toggle', {toggle});
    }

    function handleSort(column) {
        dispatch('sort', {column});
    }

    function handleRowMouseover(source) {
        hoveredRow = source;
    }

    function handleRowMouseLeave() {
        hoveredRow = null;
    }
    
</script>

<table>
    <tr>
        <th>Protein Source</th>
        <!-- <th>Source Type</th> -->
        <th on:click={() => handleSort("cost_per_20_grams_of_protein")}>
            {sortColumn === 'cost_per_20_grams_of_protein' ? sortOrderLabel : ''}
            Cost per 20 grams of Protein (USD)
        </th>
        <th class="dc dc-1" on:click={() => handleSort("cost_per_package_dollar")}>
            {sortColumn === 'cost_per_package_dollar' ? sortOrderLabel : ''}
            Cost per package
        </th>
        <th class="dc dc-2" on:click={() => handleSort("total_protein_per_package_g")}>
            {sortColumn === 'total_protein_per_package_g' ? sortOrderLabel : ''}
            Total protein per package (g)
        </th>
    </tr>

    {#each data as d (d.protein_source)}
        <!-- svelte-ignore a11y-mouse-events-have-key-events -->
        <tr on:mouseover={() => handleRowMouseover(d.protein_source)} on:mouseout={handleRowMouseLeave} class:hovered={d.protein_source === hoveredRow}>
            <td class="row-header">
                <div class="row-header-group">                
                <span class="row-header-sub-label">
                    {#if d.vegetarian === "T" && d.vegan !== "T"}
                        <Dot type="stripes" width={14} color="mediumseagreen" on:click={() => handleToggleClick("vegetarian")}/>
                        {/if}
                    {#if d.vegan === "T"}
                        <Dot type="solid" width={14} color="mediumseagreen" on:click={() => handleToggleClick("vegan")}/>
                    {/if}                    
                    {#if d.vegan !== "T" && d.vegetarian !== "T"}
                        <Dot type="stripes" width={14} color="lightsalmon" on:click={() => handleToggleClick("non-veg")}/>
                    {/if}                    
                </span>
                {d.protein_source}
                </div>
            </td>
            <td class="data-bar" style="width: {maxBarWidth}px">
                <Bar width={xScale(d.cost_per_20_grams_of_protein)} height={24} label="$ {numberFormat(d.cost_per_20_grams_of_protein)}" />
            </td>
            <td class="dc dc-1"><strong>${d.cost_per_package_dollar}</strong> /{d.cost_per_package_units}</td>
            <td class="dc dc-2">{d.total_protein_per_package_g}</td>
        </tr>
    {/each}
</table>

<style>

    table {
        width: 100%;
        border-collapse: collapse;
    }

    table th {
        text-align: left;
        height: 32px;
        border-bottom: 2px solid;
        cursor: pointer;        
    }

    table th:nth-child(1) {
        padding-left: 20px;
        cursor: default;
    }

    table th:last-child {
        padding-right: 20px;
    }

    table td:nth-child(1) {
        padding-left: 20px;
    }

    table td:last-child {
        padding-right: 20px;
    }

    table tr {
        height: 34px;   
        border-bottom: 1px solid hsl(0, 0%, 89%);                
        vertical-align: middle;
        border-radius: 40px;
    }   

    .hovered {
        background-color: hsl(216, 20%, 96%);
    }

    .dc {
        text-align: right;
    }
    .dc-1 {
        width: 280px;    
        padding-right: 40px;
    }
    .dc-2 {
        width: 280px;
    }

    .row-header-group {
        display: flex;
        gap: 12px;
        justify-content: flex-start;
    }

    .row-header {
        width: 280px;
        padding-right: 40px;
    }
 </style>