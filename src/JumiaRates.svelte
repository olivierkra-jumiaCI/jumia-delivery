<script>
    import { onMount } from 'svelte';
    import Papa from 'papaparse';
    import logo from './assets/logo.png';

    export let onNavigate;

    let tarifs = [];
    let zoneVilles = [];

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
</script>

<div class="min-h-screen bg-gray-50 font-roboto text-gray-800">
    <!-- Navigation -->
    <nav class="bg-gray-900 shadow-sm sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-20 items-center">
                <div class="flex items-center gap-2 cursor-pointer" on:click={() => onNavigate('personal')}>
                    <img src={logo} alt="Jumia Logo" style="width: 150px; height: 80px !important; object-fit: contain;">
                    <span class="text-white font-medium text-lg border-l border-gray-700 pl-2 ml-2">Grille Tarifaire</span>
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

    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        <div class="bg-white rounded-2xl shadow-xl overflow-hidden border border-gray-100">
            <div class="p-8 border-b border-gray-100 bg-white">
                <h1 class="text-3xl font-extrabold text-gray-900">Grille Tarifaire Complète</h1>
                <p class="text-gray-500 mt-2">Découvrez nos tarifs transparents basés sur la taille de vos colis et les zones de destination.</p>
            </div>
            
            <div class="overflow-x-auto custom-scrollbar">
                <table class="w-full text-sm text-left">
                    <thead class="text-xs text-gray-500 uppercase border-b bg-gray-50 sticky top-0 z-10">
                        <tr>
                            <th class="py-4 px-6">Départ</th>
                            <th class="py-4 px-6">Arrivée</th>
                            <th class="py-4 px-6">Petit Colis</th>
                            <th class="py-4 px-6">Colis Moyen</th>
                            <th class="py-4 px-6">Grand Colis</th>
                            <th class="py-4 px-6">Délai Estimé</th>
                        </tr>
                    </thead>
                    <tbody class="divide-y divide-gray-100">
                        {#each tarifs as tarif}
                            <tr class="hover:bg-gray-50 transition">
                                <td class="py-4 px-6 font-medium text-gray-900">{tarif.depart}</td>
                                <td class="py-4 px-6 font-medium text-gray-900">{tarif.arrivee}</td>
                                <td class="py-4 px-6 text-jumia-orange font-bold">{tarif.petit} FCFA</td>
                                <td class="py-4 px-6 text-jumia-orange font-bold">{tarif.moyen} FCFA</td>
                                <td class="py-4 px-6 text-jumia-orange font-bold">{tarif.grand} FCFA</td>
                                <td class="py-4 px-6 text-gray-500">{tarif.delai}</td>
                            </tr>
                        {/each}
                    </tbody>
                </table>
            </div>

            <!-- Zones & Villes List -->
            <div class="p-8 lg:p-12 border-t bg-gray-50">
                <div class="max-w-3xl">
                    <h2 class="text-2xl font-bold text-gray-900 mb-2">Zones & Localités Couvertes</h2>
                    <p class="text-gray-500 mb-8">Nous desservons plus de 123 villes à travers la Côte d'Ivoire, réparties par zones tarifaires.</p>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    {#each zoneVilles as item}
                        <div class="bg-white p-6 rounded-2xl border border-gray-200 shadow-sm hover:shadow-md transition">
                            <h3 class="font-bold text-jumia-orange mb-3 text-lg flex items-center gap-2">
                                <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M5.05 4.05a7 7 0 119.9 9.9L10 18.9l-4.95-4.95a7 7 0 010-9.9zM10 11a2 2 0 100-4 2 2 0 000 4z" clip-rule="evenodd"></path></svg>
                                {item.zone}
                            </h3>
                            <p class="text-sm text-gray-600 leading-relaxed">{item.villes}</p>
                        </div>
                    {/each}
                </div>
            </div>
        </div>
        
        <div class="mt-12 text-center text-gray-500 text-sm">
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
    }
    .custom-scrollbar::-webkit-scrollbar-track {
        background: #f1f1f1;
        border-radius: 10px;
    }
    .custom-scrollbar::-webkit-scrollbar-thumb {
        background: #F68B1E;
        border-radius: 10px;
    }
</style>
