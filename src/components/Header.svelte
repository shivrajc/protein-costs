<script>
    export let text;
    export let subText;
</script>

<div class="header">
    <h1>
        {text}
    </h1>
    <p class="sub-header">{subText}</p>
    
</div>

<style>
    .header {
        font-size: 1.1rem;
        text-align: left;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: flex-start;    
        font-family: 'Montserrat', sans-serif;
    }

    .sub-header {
        font-size: 1.4rem;
        margin: 8px 0 0 4px;
        font-weight: 400;
    }
</style>