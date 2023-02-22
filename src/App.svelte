<script>
  import MediaQuery from './MediaQuery.svelte';
  import AppDesktop from "./components/AppDesktop.svelte";
  import AppMobile from './components/AppMobile.svelte';
</script>

<main>
  <MediaQuery query="(min-width: 800px)" let:matches>
    {#if matches}
      <AppDesktop />
    {/if}
  </MediaQuery>
  
  <!-- <MediaQuery query="(min-width: 480px) and (max-width: 1200px)" let:matches>
    {#if matches}
      <MobileApp />
    {/if}
  </MediaQuery> -->
  
  <MediaQuery query="(max-width: 800px)" let:matches>
    {#if matches}
      <AppMobile />
    {/if}
  </MediaQuery>   
</main>
