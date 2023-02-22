<script>
    export let type;
    export let width;
    export let color;
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<span 
    class="dot" 
    class:stripes={type === "stripes"}
    style="background-color: {color}; width:{width}px; height:{width}px; border: 1px solid {color};"
    on:click    
    >
</span>

<style>
    .dot {
        border-radius: 50%;         
        vertical-align: middle;
        display: inline-block;
        margin-bottom: 2px;     
        cursor: pointer;           
    }

    .stripes {
        background: repeating-linear-gradient(
            45deg,
            transparent,
            transparent 2px,
            hsl(0, 0%, 100%) 2px,
            hsl(0, 0%, 100%) 4px
        );
    }
</style>