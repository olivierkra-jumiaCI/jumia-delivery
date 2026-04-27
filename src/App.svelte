<script>
  import JumiaDelivery from './JumiaDelivery.svelte'
  import JumiaBusiness from './JumiaBusiness.svelte'
  import JumiaRates from './JumiaRates.svelte'
  
  let currentView = 'personal';

  function setView(view) {
    currentView = view;
    window.scrollTo(0, 0);
  }
</script>

{#if currentView === 'personal'}
  <JumiaDelivery onNavigate={setView} />
{:else if currentView === 'business'}
  <JumiaBusiness onNavigate={setView} />
{:else if currentView === 'rates'}
  <JumiaRates onNavigate={setView} />
{/if}
