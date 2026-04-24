<script>
  import JumiaDelivery from './JumiaDelivery.svelte'
  import JumiaBusiness from './JumiaBusiness.svelte'
  
  let currentView = 'personal';

  function setView(view) {
    currentView = view;
    window.scrollTo(0, 0);
  }
</script>

{#if currentView === 'personal'}
  <JumiaDelivery onNavigate={setView} />
{:else}
  <JumiaBusiness onNavigate={setView} />
{/if}
