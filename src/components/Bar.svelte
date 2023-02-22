<script>
    export let width;
    export let height;
    export let label;
    
</script>

<div class="bar" style="width:{width}px; height:{height}px">
    <span class="label">
        {label}
    </span>
</div>

<style>
    .bar {
        display: inline-block;
        background-color: hsl(175, 29%, 91%);
        background-color: hsl(200, 32%, 90%);
        padding-left: 6px;
        border-radius: 0 4px 4px 0;
    }
    .label {
        color: hsl(175, 27%, 34%);
        color: hsl(200, 32%, 40%);
        font-weight: 500;
    }
</style>
