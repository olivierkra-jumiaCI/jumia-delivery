<script>
    import { onMount } from 'svelte';
    import Papa from 'papaparse';
    import logo from './assets/logo.png';

    export let onNavigate;

    let activeTab = 'prices'; // 'prices' or 'zones'
    let tarifs = [];
    let zoneVilles = [];
    let departureSearch = "";
    let arrivalSearch = "";
    let activeZoneFilter = "All";

    onMount(() => {
        Papa.parse("https://docs.google.com/spreadsheets/d/1M52gDOvkoXZtCA7RSmHM1vy4ksO6H5fdQQ-twAkRqKk/export?format=csv&gid=178217986", {
            download: true,
            header: true,
            skipEmptyLines: true,
            complete: function(results) {
                tarifs = results.data.filter(row => row['Départ']).map(row => ({
                    depart: row['Départ'],
                    arrivee: row['Arrivée'],
                    petit: row['Petit (40×20×13 cm)'],
                    moyen: row['Moyen (70×30×20 cm)'],
                    grand: row['Grand (100×100×62 cm)'],
                    delai: row['Délai de livraison estimé (en jours ouvrés)']
                }));

                zoneVilles = results.data.filter(row => row['Zones']).map(row => ({
                    zone: row['Zones'],
                    villes: row['Villes & Localités']
                }));
            }
        });
        window.scrollTo(0, 0);
    });

    $: uniqueZones = [...new Set(tarifs.map(t => t.depart))].sort();
    
    $: filteredTarifs = tarifs.filter(t => {
        const matchesDeparture = t.depart.toLowerCase().includes(departureSearch.toLowerCase());
        const matchesArrival = t.arrivee.toLowerCase().includes(arrivalSearch.toLowerCase());
        const matchesZone = activeZoneFilter === "All" || t.depart === activeZoneFilter;
        return matchesDeparture && matchesArrival && matchesZone;
    });
</script>

<div class="min-h-screen bg-[#F8F9FA] font-roboto text-gray-800">
    <!-- Navigation -->
    <nav class="bg-gray-900 shadow-sm sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-20 items-center">
                <div class="flex items-center gap-2 cursor-pointer" on:click={() => onNavigate('personal')}>
                    <img src={logo} alt="Jumia Logo" style="width: 150px; height: 80px !important; object-fit: contain;">
                    <span class="text-white font-medium text-lg border-l border-gray-700 pl-2 ml-2">Zone et frais de livraison</span>
                </div>
                <div class="hidden md:flex space-x-8">
                    <button on:click={() => onNavigate('personal')} class="text-gray-300 hover:text-jumia-orange transition bg-transparent">Particuliers (C2C)</button>
                    <button on:click={() => onNavigate('business')} class="text-gray-300 hover:text-jumia-orange transition bg-transparent">Professionnel (B2C)</button>
                </div>
                <div class="flex items-center">
                    <button on:click={() => onNavigate('personal')} class="text-white hover:text-jumia-orange transition font-bold flex items-center gap-2 bg-transparent">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 19l-7-7m0 0l7-7m-7 7h18"></path></svg>
                        Retour
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Orange Header -->
    <div class="bg-jumia-orange py-10 px-4 text-center">
        <h1 class="text-4xl md:text-5xl font-black text-white mb-4">Tableau des tarifs</h1>
        <p class="text-white/90 text-lg max-w-2xl mx-auto">
            Tarification transparente pour toutes les routes de livraison à travers la Côte d'Ivoire. Trouvez le coût exact de votre expédition.
        </p>
    </div>

    <!-- Main Content Container -->
    <div class="max-w-6xl mx-auto px-4 -mt-8 pb-12">
        
        <!-- Tab Switcher -->
        <div class="flex justify-center mb-8">
            <div class="bg-white p-1.5 rounded-xl shadow-lg inline-flex">
                <button 
                    on:click={() => activeTab = 'prices'}
                    class="px-6 py-2.5 rounded-lg text-sm font-bold transition flex items-center gap-2 {activeTab === 'prices' ? 'bg-gray-900 text-white shadow-md' : 'text-gray-500 hover:text-gray-700'}"
                >
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"></path></svg>
                    Liste de prix
                </button>
                <button 
                    on:click={() => activeTab = 'zones'}
                    class="px-6 py-2.5 rounded-lg text-sm font-bold transition flex items-center gap-2 {activeTab === 'zones' ? 'bg-gray-900 text-white shadow-md' : 'text-gray-500 hover:text-gray-700'}"
                >
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
                    Zones de livraisons
                </button>
            </div>
        </div>

        {#if activeTab === 'prices'}
            <div class="bg-white rounded-2xl shadow-sm border border-gray-100 p-6 mb-8">
                <h2 class="text-xl font-bold text-gray-900 mb-6">Guide des tailles de colis</h2>
                <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                    <!-- Small -->
                    <div class="bg-orange-50/50 p-6 rounded-xl border border-orange-100">
                        <div class="w-12 h-12 bg-orange-100 rounded-full flex items-center justify-center mb-4 text-jumia-orange">
                            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20"><path d="M11 3a1 1 0 10-2 0v1a1 1 0 102 0V3zM15.657 5.757a1 1 0 00-1.414-1.414l-.707.707a1 1 0 001.414 1.414l.707-.707zM18 10a1 1 0 01-1 1h-1a1 1 0 110-2h1a1 1 0 011 1zM5.05 6.464A1 1 0 106.464 5.05l-.707-.707a1 1 0 00-1.414 1.414l.707.707zM5 10a1 1 0 01-1 1H3a1 1 0 110-2h1a1 1 0 011 1zM8 16v-1a1 1 0 112 0v1a1 1 0 11-2 0zM13.536 14.95a1 1 0 011.414 0l.707.707a1 1 0 01-1.414 1.414l-.707-.707a1 1 0 010-1.414zM15.657 15.657l-.707.707a1 1 0 01-1.414-1.414l.707-.707a1 1 0 011.414 1.414zM10 11a1 1 0 100-2 1 1 0 000 2z"></path></svg>
                        </div>
                        <h3 class="font-bold text-gray-900 mb-1">Petit colis</h3>
                        <p class="text-xs text-gray-500 mb-3">0-5kg</p>
                        <p class="text-[10px] text-gray-600"><strong>Dimensions:</strong> 40x20x13 cm</p>
                        <p class="text-[10px] text-gray-600 mt-1"><strong>Exemples:</strong> Téléphones, vêtements, documents.</p>
                    </div>
                    <!-- Medium -->
                    <div class="bg-gray-50 p-6 rounded-xl border border-gray-100">
                        <div class="w-12 h-12 bg-gray-200 rounded-full flex items-center justify-center mb-4 text-gray-600">
                             <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20"><path d="M7 3a1 1 0 000 2h6a1 1 0 100-2H7zM4 7a1 1 0 011-1h10a1 1 0 110 2H5a1 1 0 01-1-1zM2 11a2 2 0 012-2h12a2 2 0 012 2v4a2 2 0 01-2 2H4a2 2 0 01-2-2v-4z"></path></svg>
                        </div>
                        <h3 class="font-bold text-gray-900 mb-1">Moyen colis</h3>
                        <p class="text-xs text-gray-500 mb-3">5-15kg</p>
                        <p class="text-[10px] text-gray-600"><strong>Dimensions:</strong> 70x30x20 cm</p>
                        <p class="text-[10px] text-gray-600 mt-1"><strong>Exemples:</strong> Mixeurs, chaussures, ordinateurs.</p>
                    </div>
                    <!-- Large -->
                    <div class="bg-gray-50 p-6 rounded-xl border border-gray-100">
                        <div class="w-12 h-12 bg-gray-200 rounded-full flex items-center justify-center mb-4 text-gray-600">
                            <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20"><path d="M2 4a1 1 0 011-1h2a1 1 0 011 1v12a1 1 0 01-1 1H3a1 1 0 01-1-1V4zM8 4a1 1 0 011-1h2a1 1 0 011 1v12a1 1 0 01-1 1H9a1 1 0 01-1-1V4zM15 3a1 1 0 00-1 1v12a1 1 0 001 1h2a1 1 0 001-1V4a1 1 0 00-1-1h-2z"></path></svg>
                        </div>
                        <h3 class="font-bold text-gray-900 mb-1">Grand colis</h3>
                        <p class="text-xs text-gray-500 mb-3">15-30kg+</p>
                        <p class="text-[10px] text-gray-600"><strong>Dimensions:</strong> 100x100x62 cm</p>
                        <p class="text-[10px] text-gray-600 mt-1"><strong>Exemples:</strong> TV, Meubles, Gros Electroménagers.</p>
                    </div>
                </div>
            </div>

            <div class="bg-white rounded-2xl shadow-sm border border-gray-100 p-6 mb-6">
                <h2 class="text-xl font-bold text-gray-900 mb-6">Recherche d'itinéraires</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div class="relative">
                        <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                        </span>
                        <input 
                            type="text" 
                            bind:value={departureSearch}
                            placeholder="Recherche zone de départ..." 
                            class="w-full pl-10 pr-4 py-3 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:border-jumia-orange focus:ring-0 transition"
                        />
                    </div>
                    <div class="relative">
                        <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                        </span>
                        <input 
                            type="text" 
                            bind:value={arrivalSearch}
                            placeholder="Recherche zone d'arrivée..." 
                            class="w-full pl-10 pr-4 py-3 bg-gray-50 border border-gray-200 rounded-xl text-sm focus:border-jumia-orange focus:ring-0 transition"
                        />
                    </div>
                </div>
            </div>

            <!-- Zone Filters -->
            <div class="flex flex-wrap gap-2 mb-8 overflow-x-auto pb-2 scrollbar-hide">
                <button 
                    on:click={() => activeZoneFilter = "All"}
                    class="px-5 py-2 rounded-full text-xs font-bold transition whitespace-nowrap {activeZoneFilter === 'All' ? 'bg-jumia-orange text-white' : 'bg-white text-gray-500 border border-gray-200 hover:border-jumia-orange'}"
                >
                    All Routes
                </button>
                {#each uniqueZones as zone}
                    <button 
                        on:click={() => activeZoneFilter = zone}
                        class="px-5 py-2 rounded-full text-xs font-bold transition whitespace-nowrap {activeZoneFilter === zone ? 'bg-jumia-orange text-white' : 'bg-white text-gray-500 border border-gray-200 hover:border-jumia-orange'}"
                    >
                        {zone}
                    </button>
                {/each}
            </div>

            <!-- Routes Table -->
            <div class="bg-white rounded-2xl shadow-xl overflow-hidden border border-gray-100">
                <div class="overflow-x-auto custom-scrollbar">
                    <table class="w-full text-sm text-left">
                        <thead>
                            <tr class="bg-jumia-orange text-white text-xs uppercase font-black">
                                <th class="py-5 px-6">Départ</th>
                                <th class="py-5 px-6">Arrivée</th>
                                <th class="py-5 px-6">Petit colis</th>
                                <th class="py-5 px-6">Moyen colis</th>
                                <th class="py-5 px-6">Grand colis</th>
                                <th class="py-5 px-6">délai de livraison ( estimé en jours ouvés)</th>
                            </tr>
                        </thead>
                        <tbody class="divide-y divide-gray-100">
                            {#each filteredTarifs as tarif}
                                <tr class="hover:bg-orange-50/30 transition odd:bg-gray-50/50">
                                    <td class="py-5 px-6 font-bold text-gray-900">{tarif.depart}</td>
                                    <td class="py-5 px-6 font-bold text-gray-900">{tarif.arrivee}</td>
                                    <td class="py-5 px-6 font-black text-gray-900">{tarif.petit} FCFA</td>
                                    <td class="py-5 px-6 font-black text-gray-900">{tarif.moyen} FCFA</td>
                                    <td class="py-5 px-6 font-black text-gray-900">{tarif.grand} FCFA</td>
                                    <td class="py-5 px-6 text-gray-500 italic">{tarif.delai}</td>
                                </tr>
                            {/each}
                            {#if filteredTarifs.length === 0}
                                <tr>
                                    <td colspan="6" class="py-20 text-center text-gray-400 italic">No routes found matching your criteria.</td>
                                </tr>
                            {/if}
                        </tbody>
                    </table>
                </div>
            </div>

        {:else}
            <!-- Delivery Zones Content -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                {#each zoneVilles as item}
                    <div class="bg-white p-8 rounded-3xl border border-gray-200 shadow-sm hover:shadow-xl transition group">
                        <div class="flex items-center gap-4 mb-4">
                            <div class="w-12 h-12 bg-orange-100 rounded-2xl flex items-center justify-center text-jumia-orange group-hover:bg-jumia-orange group-hover:text-white transition">
                                <svg class="w-6 h-6" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd"></path></svg>
                            </div>
                            <h3 class="text-xl font-black text-gray-900">{item.zone}</h3>
                        </div>
                        <p class="text-sm text-gray-500 leading-relaxed pl-16">
                            {item.villes}
                        </p>
                    </div>
                {/each}
            </div>
        {/if}

        <div class="mt-12 pt-6 border-t border-gray-200 text-center text-gray-400 text-sm">
            <p>© {new Date().getFullYear()} Jumia Delivery Côte d'Ivoire. Tous droits réservés.</p>
        </div>
    </div>
</div>

<style>
    .font-roboto { font-family: 'Roboto', sans-serif; }
    .jumia-orange { color: #F68B1E; }
    .text-jumia-orange { color: #F68B1E; }
    .bg-jumia-orange { background-color: #F68B1E; }
    
    .custom-scrollbar::-webkit-scrollbar {
        height: 6px;
        width: 6px;
    }
    .custom-scrollbar::-webkit-scrollbar-track {
        background: #f1f1f1;
        border-radius: 10px;
    }
    .custom-scrollbar::-webkit-scrollbar-thumb {
        background: #F68B1E;
        border-radius: 10px;
    }
    .scrollbar-hide::-webkit-scrollbar {
        display: none;
    }
</style>
