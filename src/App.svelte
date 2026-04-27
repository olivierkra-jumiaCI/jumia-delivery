<script>
  import { onMount } from 'svelte';
  import JumiaDelivery from './JumiaDelivery.svelte'
  import JumiaBusiness from './JumiaBusiness.svelte'
  import JumiaRates from './JumiaRates.svelte'
  import JumiaAgencies from './JumiaAgencies.svelte'
  
  let currentView = 'personal';

  function handleHash() {
    const hash = window.location.hash;
    if (hash === '#/business') {
      currentView = 'business';
    } else if (hash === '#/tarifs') {
      currentView = 'rates';
    } else if (hash === '#/agences') {
      currentView = 'agencies';
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
    else if (view === 'agencies') window.location.hash = '#/agences';
  }
</script>

{#if currentView === 'personal'}
  <JumiaDelivery onNavigate={setView} />
{:else if currentView === 'business'}
  <JumiaBusiness onNavigate={setView} />
{:else if currentView === 'rates'}
  <JumiaRates onNavigate={setView} />
{:else if currentView === 'agencies'}
  <JumiaAgencies onNavigate={setView} />
{/if}
