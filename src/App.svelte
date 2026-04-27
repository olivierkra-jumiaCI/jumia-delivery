<script>
  import { onMount } from 'svelte';
  import JumiaDelivery from './JumiaDelivery.svelte'
  import JumiaBusiness from './JumiaBusiness.svelte'
  import JumiaRates from './JumiaRates.svelte'
  
  let currentView = 'personal';

  function handleHash() {
    const hash = window.location.hash;
    if (hash === '#/business') {
      currentView = 'business';
    } else if (hash === '#/tarifs') {
      currentView = 'rates';
    } else {
      currentView = 'personal';
    }
    window.scrollTo(0, 0);
  }

  onMount(() => {
    handleHash();
    window.addEventListener('hashchange', handleHash);
    return () => window.removeEventListener('hashchange', handleHash);
  });

  function setView(view) {
    if (view === 'personal') window.location.hash = '#/';
    else if (view === 'business') window.location.hash = '#/business';
    else if (view === 'rates') window.location.hash = '#/tarifs';
  }
</script>

{#if currentView === 'personal'}
  <JumiaDelivery onNavigate={setView} />
{:else if currentView === 'business'}
  <JumiaBusiness onNavigate={setView} />
{:else if currentView === 'rates'}
  <JumiaRates onNavigate={setView} />
{/if}
