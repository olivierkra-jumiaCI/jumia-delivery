<script>
    import { onMount } from 'svelte';
    export let onNavigate;
    let showMobileMenu = false;

    function toggleMobileMenu() {
        showMobileMenu = !showMobileMenu;
    }

    function handleMobileNavigate(view) {
        onNavigate(view);
        showMobileMenu = false;
    }

    let trackingNumber = "";
    function handleTrack() {
        if (trackingNumber.trim()) {
            window.open(`https://packagetracker-services.jumia.com/#/${trackingNumber.trim()}`, '_blank');
        }
    }
    import Map from './Map.svelte';
    import heroImage from './assets/hero.jpg';
    import logo from './assets/logo.png';
    import Papa from 'papaparse';
    import step1 from './assets/step-1.png';
    import step2 from './assets/step-2.png';
    import step3 from './assets/step-3.png';
    import step4 from './assets/step-4.png';

    let showFullRates = false;
    let tarifs = [];
    let zoneVilles = [];
    let zoneToCity = {};

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

                // Create a mapping of Zone -> Representative City
                zoneVilles.forEach(item => {
                    const firstCity = item.villes.split('-')[0].trim();
                    zoneToCity[item.zone] = firstCity;
                });
            }
        });
    });

    function toggleRates() {
        showFullRates = !showFullRates;
    }
</script>

<svelte:head>
    <title>Envoyez des colis à vos amis et à votre famille | Jumia Delivery Côte d'Ivoire</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700;900&display=swap" rel="stylesheet">
</svelte:head>

<div class="bg-gray-50 text-gray-800">
    <!-- Navigation -->
    <nav class="bg-gray-900 shadow-sm sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-20 items-center">
                <div class="flex items-center gap-2">
                    <img src={logo} alt="Jumia Logo" style="width: 150px; height: 80px !important; object-fit: contain;">
                    <span class="text-white font-medium text-lg border-l border-gray-700 pl-2 ml-2">Livraison</span>
                </div>
                <div class="hidden md:flex space-x-8">
                    <button on:click={() => onNavigate('personal')} class="text-white font-bold border-b-2 border-jumia-orange bg-transparent">Particuliers (C2C)</button>
                    <button on:click={() => onNavigate('business')} class="text-gray-300 hover:text-jumia-orange transition bg-transparent">Professionnel (B2C)</button>
                    <a href="#tarifs" class="text-gray-300 hover:text-jumia-orange transition flex items-center">Tarifs</a>
                    <a href="#points-relais" class="text-gray-300 hover:text-jumia-orange transition flex items-center">Points Relais</a>
                </div>
                <div class="flex items-center md:hidden">
                    <button on:click={toggleMobileMenu} class="text-gray-300 hover:text-white focus:outline-none bg-transparent">
                        <svg class="h-8 w-8" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                            {#if showMobileMenu}
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" />
                            {:else}
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
                            {/if}
                        </svg>
                    </button>
                </div>
                <div class="hidden md:flex items-center">
                    <button class="bg-jumia-orange text-white px-5 py-2 rounded shadow hover:bg-orange-600 transition font-bold">
                        Suivre un Colis
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Menu -->
        {#if showMobileMenu}
            <div class="md:hidden bg-gray-800 border-t border-gray-700 py-4 px-4 space-y-4 animate-fadeIn">
                <div class="flex flex-col space-y-3">
                    <button on:click={() => handleMobileNavigate('personal')} class="text-left text-white font-bold py-2 bg-transparent">Particuliers (C2C)</button>
                    <button on:click={() => handleMobileNavigate('business')} class="text-left text-gray-300 hover:text-jumia-orange py-2 bg-transparent">Professionnel (B2C)</button>
                    <a href="#tarifs" on:click={() => showMobileMenu = false} class="text-gray-300 hover:text-jumia-orange py-2">Tarifs</a>
                    <a href="#points-relais" on:click={() => showMobileMenu = false} class="text-gray-300 hover:text-jumia-orange py-2">Points Relais</a>
                </div>
                <button class="w-full bg-jumia-orange text-white px-5 py-3 rounded shadow font-bold text-center">
                    Suivre un Colis
                </button>
            </div>
        {/if}
    </nav>

    <!-- Hero Section -->
    <div class="relative bg-white overflow-hidden">
        <div class="max-w-7xl mx-auto">
            <div class="relative z-10 pb-8 bg-white sm:pb-16 md:pb-20 lg:max-w-2xl lg:w-full lg:pb-28 xl:pb-32">
                <svg class="hidden lg:block absolute right-0 inset-y-0 h-full w-48 text-white transform translate-x-1/2" fill="currentColor" viewBox="0 0 100 100" preserveAspectRatio="none" aria-hidden="true">
                    <polygon points="50,0 100,0 50,100 0,100" />
                </svg>

                <main class="mt-10 mx-auto max-w-7xl px-4 sm:mt-12 sm:px-6 md:mt-16 lg:mt-20 lg:px-8 xl:mt-28">
                    <div class="sm:text-center lg:text-left">
                        <span class="inline-block py-1 px-3 rounded bg-orange-100 text-orange-600 text-xs font-bold tracking-wider mb-4 uppercase">Pour vous et votre famille</span>
                        <h1 class="text-4xl tracking-tight font-extrabold text-gray-900 sm:text-5xl md:text-6xl">
                            <span class="block xl:inline">Envoyez des colis en toute sécurité</span>
                            <span class="block text-jumia-orange">partout en Côte d'Ivoire.</span>
                        </h1>
                        <p class="mt-3 text-base text-gray-500 sm:mt-5 sm:text-lg sm:max-w-xl sm:mx-auto md:mt-5 md:text-xl lg:mx-auto">
                            Vous avez une expédition à faire ? <br class="hidden md:block">
                            Rendez-vous dans l’agence Jumia la plus proche et profitez d’un service rapide et fiable avec Jumia Delivery.<br>
                            <span class="font-bold text-jumia-orange">Jumia Delivery le meilleur service de livraison et d'expédition de colis en Côte d'Ivoire</span>
                        </p>
                        
                        <!-- Tracking Widget -->
                        <div class="mt-8 sm:max-w-lg sm:mx-auto sm:text-center lg:text-left lg:mx-0">
                            <form class="mt-3 sm:flex shadow-lg rounded-lg overflow-hidden border border-gray-200" on:submit|preventDefault={handleTrack}>
                                <label for="tracking" class="sr-only">Numéro de suivi</label>
                                <input 
                                    type="text" 
                                    name="tracking" 
                                    id="tracking" 
                                    bind:value={trackingNumber}
                                    class="block w-full py-4 px-4 text-gray-900 placeholder-gray-500 focus:outline-none" 
                                    placeholder="Entrez votre numéro de suivi (ex. JPD-DAB-6531511489)"
                                >
                                <button type="submit" class="mt-3 w-full px-6 py-3 border border-transparent text-base font-medium bg-gray-800 text-white shadow-sm hover:bg-gray-900 focus:outline-none sm:mt-0 sm:w-auto">
                                    Suivre
                                </button>
                            </form>
                            <p class="mt-2 text-xs text-gray-400">Suivez la localisation en temps réel via notre application ou site web.</p>
                        </div>
                    </div>
                </main>
            </div>
        </div>
        <div class="lg:absolute lg:inset-y-0 lg:right-0 lg:w-1/2 bg-orange-50 flex items-center justify-center">
            <!-- Illustration placeholder -->
            <div class="relative w-full h-64 sm:h-72 md:h-96 lg:h-full flex items-center justify-center text-orange-200">
                <svg class="w-1/2 h-1/2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1" d="M12 8v13m0-13V6a2 2 0 112 2h-2zm0 0V5.5A2.5 2.5 0 109.5 8H12zm-7 4h14M5 12a2 2 0 110-4h14a2 2 0 110 4M5 12v7a2 2 0 002 2h10a2 2 0 002-2v-7"></path>
                </svg>
                <div class="absolute inset-0 bg-gradient-to-r from-white to-transparent lg:via-white/20"></div>
                <img src={heroImage} alt="Agent Jumia remettant un colis" class="absolute inset-0 w-full h-full object-cover">
            </div>
        </div>
    </div>

    <!-- Value Props -->
    <div class="bg-white py-12">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid grid-cols-1 md:grid-cols-3 gap-8 text-center">
                <div class="p-6 bg-orange-50 rounded-xl border border-orange-100">
                    <div class="w-12 h-12 bg-orange-100 text-jumia-orange rounded-full flex items-center justify-center mx-auto mb-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 12l2 2 4-4m5.618-4.016A11.955 11.955 0 0112 2.944a11.955 11.955 0 01-9.618 3.033A12.02 12.02 0 003 9c0 5.591 3.824 10.29 9 11.622 5.176-1.332 9-6.03 9-11.622 0-1.042-.133-2.052-.382-3.016z"></path></svg>
                    </div>
                    <h3 class="text-lg font-bold text-gray-900">Protection Premium</h3>
                    <p class="mt-2 text-gray-600">Protégez vos colis de haute valeur contre les pertes et les dommages pendant le transport, expédier vos colis en toute confiance.</p>
                </div>
                <div class="p-6 bg-orange-50 rounded-xl border border-orange-100">
                    <div class="w-12 h-12 bg-orange-100 text-jumia-orange rounded-full flex items-center justify-center mx-auto mb-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z"></path></svg>
                    </div>
                    <h3 class="text-lg font-bold text-gray-900">Tarifs Abordables</h3>
                    <p class="mt-2 text-gray-600">Tarifs à partir de seulement <span class="font-bold text-jumia-orange">750 FCFA</span>. Tarification transparente sans frais cachés.</p>
                </div>
                <div class="p-6 bg-orange-50 rounded-xl border border-orange-100">
                    <div class="w-12 h-12 bg-orange-100 text-jumia-orange rounded-full flex items-center justify-center mx-auto mb-4">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
                    </div>
                    <h3 class="text-lg font-bold text-gray-900">Couverture Nationale</h3>
                    <p class="mt-2 text-gray-600">Plus de 231 Points Relais dans plus de 123 villes. Nous livrons dans toutes les régions de Côte d'Ivoire.</p>
                </div>
            </div>
        </div>
    </div>

    <!-- How It Works -->
    <div class="bg-white py-20">
        <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8">
            <h2 class="text-3xl font-bold text-gray-900 text-center mb-12">Comment faire expédier un colis ?</h2>
            <div class="grid grid-cols-1 md:grid-cols-2 border border-gray-100 rounded-3xl overflow-hidden shadow-sm divide-y md:divide-y-0 md:divide-x divide-gray-100">
                <!-- Step 1 -->
                <div class="p-8 md:p-12 flex items-center gap-8 bg-white hover:bg-gray-50/50 transition">
                    <div class="w-24 h-24 flex-shrink-0 flex items-center justify-center overflow-hidden">
                        <img src={step1} alt="Point Relais" class="w-full h-full object-contain">
                    </div>
                    <p class="text-gray-700 text-lg leading-relaxed">
                        <span class="font-black text-2xl text-gray-900 mr-2">1-</span> Se rendre dans le point relais JUMIA le plus proche. (Plus de 191 agences dans plus de 123 villes).
                    </p>
                </div>

                <!-- Step 2 -->
                <div class="p-8 md:p-12 flex items-center gap-8 bg-white hover:bg-gray-50/50 transition">
                    <div class="w-24 h-24 flex-shrink-0 flex items-center justify-center overflow-hidden">
                        <img src={step2} alt="Colis" class="w-full h-full object-contain">
                    </div>
                    <p class="text-gray-700 text-lg leading-relaxed">
                        <span class="font-black text-2xl text-gray-900 mr-2">2-</span> Faire enregistrer son colis et régler les frais d'expédition auprès de l'agent en point relais Jumia.
                    </p>
                </div>

                <!-- Step 3 -->
                <div class="p-8 md:p-12 flex items-center gap-8 bg-white hover:bg-gray-50/50 transition border-t border-gray-100 md:border-t-0">
                    <div class="w-24 h-24 flex-shrink-0 flex items-center justify-center overflow-hidden">
                        <img src={step3} alt="Alertes" class="w-full h-full object-contain">
                    </div>
                    <p class="text-gray-700 text-lg leading-relaxed">
                        <span class="font-black text-2xl text-gray-900 mr-2">3-</span> Les informations d'expéditions sont automatiquement transmises par sms et ou e-mail aux expéditeur et destinataire.
                    </p>
                </div>

                <!-- Step 4 -->
                <div class="p-8 md:p-12 flex items-center gap-8 bg-white hover:bg-gray-50/50 transition border-t border-gray-100">
                    <div class="w-24 h-24 flex-shrink-0 flex items-center justify-center overflow-hidden">
                        <img src={step4} alt="Suivi" class="w-full h-full object-contain">
                    </div>
                    <p class="text-gray-700 text-lg leading-relaxed">
                        <span class="font-black text-2xl text-gray-900 mr-2">4-</span> Suivre en temps réel la localisation de votre colis grâce à notre <a href="https://packagetracker-services.jumia.com/#/" target="_blank" rel="noopener noreferrer" class="text-jumia-orange font-bold hover:underline">Application de suivi.</a>
                    </p>
                </div>
            </div>

            <div class="mt-10 max-w-4xl mx-auto">
                <div class="bg-blue-50 border-l-4 border-blue-400 p-6 rounded-r-2xl">
                    <div class="flex gap-4">
                        <div class="flex-shrink-0">
                            <svg class="h-6 w-6 text-blue-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                            </svg>
                        </div>
                        <div class="text-sm text-blue-800 space-y-3">
                            <p class="font-bold">Un SMS sera envoyé à chaque étape, que ce soit pour la livraison ou le retour du colis.</p>
                            <p class="text-blue-700 leading-relaxed">
                                <span class="font-bold">NB:</span> Les frais de livraison sont à la charge de l'expéditeur et doivent être réglés lors du dépôt en point relais Jumia. Les frais de livraison ne sont pas remboursables une fois le colis créé, sauf en cas de dommage ou de perte avérée.
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Pricing Section -->
    <div id="tarifs" class="py-16 bg-white scroll-mt-20">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <h2 class="text-3xl font-bold text-gray-900 text-center mb-10">Tarification Transparente</h2>
            
            <div class="max-w-4xl mx-auto bg-white shadow-xl rounded-2xl overflow-hidden border border-gray-100">
                <div class="grid grid-cols-1 md:grid-cols-2">
                    <div class="p-8 bg-blue-50">
                        <h3 class="text-xl font-bold text-gray-800 mb-4">Tailles de Colis</h3>
                        <div class="space-y-4">
                            <div class="flex items-center">
                                <div class="w-12 h-12 bg-white rounded border border-gray-200 flex items-center justify-center font-bold text-gray-400 text-xs">Petit</div>
                                <div class="ml-4">
                                    <p class="font-bold text-gray-900">Petit Colis (40×20×13 cm)</p>
                                    <p class="text-sm text-gray-500">Téléphones, chemises, livres, etc.</p>
                                </div>
                            </div>
                            <div class="flex items-center">
                                <div class="w-16 h-16 bg-white rounded border border-gray-200 flex items-center justify-center font-bold text-gray-400 text-xs">Moyen</div>
                                <div class="ml-4">
                                    <p class="font-bold text-gray-900">Colis Moyen (70×30×20 cm)</p>
                                    <p class="text-sm text-gray-500">Mixeurs, chaussures, ordinateurs portables.</p>
                                </div>
                            </div>
                            <div class="flex items-center">
                                <div class="w-20 h-20 bg-white rounded border border-gray-200 flex items-center justify-center font-bold text-gray-400 text-xs">Grand</div>
                                <div class="ml-4">
                                    <p class="font-bold text-gray-900">Grand Colis (100×100×62 cm)</p>
                                    <p class="text-sm text-gray-500">Micro-ondes, articles volumineux.</p>
                                </div>
                            </div>
                        </div>

                        <!-- Zone & Villes Section -->
                        <div class="mt-8 pt-6 border-t border-blue-100">
                            <h3 class="text-lg font-bold text-gray-800 mb-4">Zones & Localités</h3>
                            <div class="space-y-3 max-h-[250px] overflow-y-auto pr-2 custom-scrollbar">
                                {#each zoneVilles as item}
                                    <div class="bg-white p-3 rounded-lg border border-blue-50 shadow-sm">
                                        <p class="font-bold text-jumia-orange text-sm mb-1">{item.zone}</p>
                                        <p class="text-xs text-gray-500 leading-relaxed">{item.villes}</p>
                                    </div>
                                {/each}
                            </div>
                        </div>
                    </div>
                    <div class="p-8">
                        <h3 class="text-xl font-bold text-gray-800 mb-4">Trajets Populaires (Petit Colis)</h3>
                        <table class="w-full text-sm text-left">
                            <thead class="text-xs text-gray-500 uppercase border-b">
                                <tr>
                                    <th class="py-2">Trajet</th>
                                    <th class="py-2">Délai</th>
                                    <th class="py-2 text-right">Prix</th>
                                </tr>
                            </thead>
                            <tbody class="divide-y divide-gray-100">
                                {#if tarifs.length > 0}
                                    {#each tarifs.slice(0, 5) as tarif}
                                        <tr>
                                            <td class="py-3 font-medium text-gray-900">
                                                {zoneToCity[tarif.depart] || ''} <span class="text-xs text-gray-400 font-normal">({tarif.depart})</span> 
                                                <span class="text-gray-400 mx-1">→</span> 
                                                {zoneToCity[tarif.arrivee] || ''} <span class="text-xs text-gray-400 font-normal">({tarif.arrivee})</span>
                                            </td>
                                            <td class="py-3 text-gray-500">{tarif.delai}</td>
                                            <td class="py-3 text-right font-bold text-jumia-orange">{tarif.petit} FCFA</td>
                                        </tr>
                                    {/each}
                                {:else}
                                    <tr>
                                        <td colspan="3" class="py-10 text-center text-gray-400">Chargement des tarifs...</td>
                                    </tr>
                                {/if}
                            </tbody>
                        </table>
                        <div class="mt-4 pt-4 border-t border-gray-100 text-center">
                            <button on:click|preventDefault={toggleRates} class="text-jumia-orange font-bold text-sm hover:underline cursor-pointer bg-transparent border-none p-0">Voir la grille tarifaire complète →</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Stations / Network -->
    <div id="points-relais" class="bg-gray-50 py-16 scroll-mt-20">
        <div class="max-w-7xl mx-auto px-4 text-center">
            <h2 class="text-3xl font-bold text-gray-900 mb-4">Nos Points Relais & Zones d'Expédition</h2>
            <p class="text-gray-500 max-w-2xl mx-auto mb-10">Avec plus de 283 Points Relais, déposer un colis est aussi simple que de marcher dans la rue.</p>
            
            <div class="flex flex-wrap justify-center gap-3 mb-10">
                <span class="px-4 py-2 bg-white rounded-full border border-gray-200 text-gray-600 text-sm">Abidjan</span>
                <span class="px-4 py-2 bg-white rounded-full border border-gray-200 text-gray-600 text-sm">Yamoussoukro</span>
                <span class="px-4 py-2 bg-white rounded-full border border-gray-200 text-gray-600 text-sm">Bouaké</span>
                <span class="px-4 py-2 bg-white rounded-full border border-gray-200 text-gray-600 text-sm">San Pedro</span>
                <span class="px-4 py-2 bg-white rounded-full border border-gray-200 text-gray-600 text-sm">Daloa</span>
                <span class="px-4 py-2 bg-white rounded-full border border-gray-200 text-gray-600 text-sm">Korhogo</span>
                <span class="px-4 py-2 bg-white rounded-full border border-gray-200 text-gray-600 text-sm">+100 Autres Villes</span>
            </div>

            <div class="bg-white p-1 rounded-2xl shadow-2xl w-full max-w-6xl h-[600px] mx-auto relative overflow-hidden border border-gray-100 text-left">
                <!-- Interactive Leaflet Map -->
                <Map />
            </div>
        </div>
    </div>

    <!-- Conditions & Rules Section -->
    <div class="bg-white py-16 border-t border-gray-100">
        <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
            <h2 class="text-3xl font-bold text-gray-900 mb-10 text-center">Conditions et Règles d'Expédition</h2>
            
            <div class="space-y-12 text-gray-700 leading-relaxed">
                <!-- A -->
                <section>
                    <h3 class="text-xl font-bold text-jumia-orange mb-4">A- Conditions et modalités de confiscation des colis retournés</h3>
                    <p>
                        Le destinataire dispose de 05 jours pour récupérer son colis à la PUS de destination. En cas de non-récupération, le colis sera retourné à la PUS d'origine où il restera disponible pour un retrait par l'expéditeur pendant un délai de 05 jours supplémentaires. Si l'expéditeur ne récupère pas le colis dans ce délai, celui-ci sera transféré vers notre <span class="font-bold">entrepôt de dépôt à Koumassi (all VRTC)</span> et conservé pendant une période maximale de 21 jours. Au terme de cette période, tout colis non réclamé sera considéré comme abandonné et deviendra la propriété de Jumia avec <span class="font-bold">un transfert à notre entrepôt final sis à pk24</span>. Veuillez consulter le point 4 des conditions générales d'utilisation pour plus d'informations.
                    </p>
                </section>

                <!-- B -->
                <section>
                    <h3 class="text-xl font-bold text-jumia-orange mb-4">B- Règles d'expédition</h3>
                    <p class="mb-4">Les points relais Jumia peuvent refuser les articles jugés fragiles ou mal emballés ainsi que tous les colis volumineux.</p>
                    <p class="mb-4">Les articles interdits comprennent les matériaux dangereux, les animaux (vivants ou morts), l'argent liquide, les drogues et les armes à feu, etc.</p>
                    <p class="text-orange-600 font-medium italic">Important : Aucune indemnisation ne sera fournie pour les envois interdits, et l'expéditeur pourra être tenu responsable.</p>
                </section>

                <!-- C -->
                <section>
                    <h3 class="text-xl font-bold text-jumia-orange mb-4">C- Règles de livraison</h3>
                    <p class="mb-4">Lors de la livraison au destinataire, l'agent en point relais Jumia procède ainsi :</p>
                    <ol class="list-decimal ml-6 space-y-2">
                        <li>Confirmer le numéro de suivi,</li>
                        <li>Vérifier l'identité du récupérant avec une pièce d'identité légale et valide,</li>
                        <li>Enregistrer les date, signature, numéro des pièce d'identité et téléphone,</li>
                        <li>Remettre le colis après les vérifications, garantissant sécurité et traçabilité.</li>
                    </ol>
                </section>

                <!-- D -->
                <section>
                    <h3 class="text-xl font-bold text-jumia-orange mb-4">D- Politique de remboursement</h3>
                    
                    <div class="space-y-6">
                        <div>
                            <h4 class="font-bold text-gray-900 mb-2">1. Assurance Supplémentaire (Protection Premium)</h4>
                            <p>
                                En souscrivant à l'option <span class="font-bold">Protection Premium</span> pour votre envoi, et après règlement des frais additionnels correspondants (conformément aux articles 5.7 et 6.10 de nos Conditions Générales d'Utilisation), vous bénéficiez d'une <span class="font-bold">couverture renforcée à 100%</span> en cas de perte du colis.
                            </p>
                            <p class="mt-2">
                                Dans ce cadre, Jumia vous indemnisera à hauteur de la valeur la plus basse entre la valeur déclarée du colis et sa valeur actuelle sur la plateforme Jumia.
                            </p>
                            <p class="mt-2">
                                En cas de dommage constaté sur un colis retourné au point relais Jumia de dépôt, vous pourrez prétendre à une <span class="font-bold">indemnisation allant jusqu'à 75%</span> de la valeur de l'article, calculée sur la base de sa valeur de reconstitution ou de remplacement.
                            </p>
                        </div>

                        <div>
                            <h4 class="font-bold text-gray-900 mb-2">2. Colis sans Protection Premium</h4>
                            <p>
                                Pour les envois ne bénéficiant pas de la Protection Premium, l'indemnisation en cas de perte, de dommage ou de retour à l'expéditeur est limitée à <span class="font-bold">deux fois (2x) le montant des frais de livraison</span>.
                            </p>
                        </div>

                        <div>
                            <h4 class="font-bold text-gray-900 mb-2">3. Limite d'Indemnisation</h4>
                            <p>
                                Quelle que soit la nature de l'envoi (avec ou sans Protection Premium), le plafond d'indemnisation pour un colis individuel est fixé à <span class="font-bold">400 000 FCFA</span> (quatre cent mille francs CFA).
                            </p>
                            <p class="mt-2">
                                Jumia se réserve le droit de modifier ce plafond à sa discrétion, sous réserve de notification préalable, conformément à nos Conditions Générales d'Utilisation.
                            </p>
                        </div>

                        <p class="font-bold">NB : Pour toute réclamation, n'hésitez pas à contacter notre service client.</p>
                    </div>
                </section>

                <!-- E -->
                <section class="bg-orange-50 p-6 rounded-xl border border-orange-100">
                    <h3 class="text-xl font-bold text-jumia-orange mb-4">E- Message de licence et contact service client</h3>
                    <p class="mb-4 italic">Opérateur agréé pour l'expédition des services postaux nationaux par l'ARTCI.</p>
                    <p class="mb-4">Pour toute réclamation, veuillez appeler le service client :</p>
                    <ul class="space-y-2 font-bold">
                        <li class="flex items-center gap-2">
                            <svg class="w-4 h-4 text-orange-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"></path></svg>
                            25 20 00 61 61
                        </li>
                        <li class="flex items-center gap-2">
                            <svg class="w-4 h-4 text-orange-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"></path></svg>
                            07 00 88 60 43
                        </li>
                    </ul>
                    <a href="https://www.jumia.ci/sp-termes-conditions-jumia-delivery/" target="_blank" rel="noopener noreferrer" class="inline-block mt-6 text-jumia-orange font-bold hover:underline">Voir nos conditions Générales d'utilisation →</a>
                </section>
            </div>
        </div>
    </div>

    <!-- FAQ -->
    <div class="max-w-3xl mx-auto px-4 py-16">
        <h2 class="text-3xl font-bold text-gray-900 mb-8 text-center">Foire Aux Questions</h2>
        <div class="space-y-4">
            <details class="group bg-white rounded-lg shadow-sm border border-gray-200">
                <summary class="flex justify-between items-center font-medium cursor-pointer list-none p-4">
                    <span>Comment suivre mon colis ?</span>
                    <span class="transition group-open:rotate-180">
                        <svg fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24"><path d="M6 9l6 6 6-6"></path></svg>
                    </span>
                </summary>
                <div class="text-gray-600 mt-0 group-open:animate-fadeIn p-4 pt-0 text-sm">
                    C'est très simple ! Cliquez sur le lien de suivi que nous vous avons envoyé par SMS ou par email juste après l'enregistrement de votre colis.
                </div>
            </details>
            <details class="group bg-white rounded-lg shadow-sm border border-gray-200">
                <summary class="flex justify-between items-center font-medium cursor-pointer list-none p-4">
                    <span>Quels sont les tarifs et les méthodes de paiement ?</span>
                    <span class="transition group-open:rotate-180">
                        <svg fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24"><path d="M6 9l6 6 6-6"></path></svg>
                    </span>
                </summary>
                <div class="text-gray-600 mt-0 group-open:animate-fadeIn p-4 pt-0 text-sm">
                    <p class="mb-2"><strong>Calcul des tarifs :</strong> Les tarifs sont calculés en fonction de la taille du colis (S, M, ou L) et de la destination (ville/pays).</p>
                    <p><strong>Moyens de paiement :</strong> Vous pouvez régler vos frais d'expédition directement dans nos agences, soit en espèces, soit via mobile money.</p>
                </div>
            </details>
            <details class="group bg-white rounded-lg shadow-sm border border-gray-200">
                <summary class="flex justify-between items-center font-medium cursor-pointer list-none p-4">
                    <span>Quels articles puis-je envoyer ?</span>
                    <span class="transition group-open:rotate-180">
                        <svg fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24"><path d="M6 9l6 6 6-6"></path></svg>
                    </span>
                </summary>
                <div class="text-gray-600 mt-0 group-open:animate-fadeIn p-4 pt-0 text-sm">
                    Vous pouvez envoyer la plupart des marchandises générales, y compris vêtements, chaussures, livres, petits appareils électroménagers (mixeurs) et denrées non périssables. Nous ne transportons pas d'espèces, de bétail, de produits frais ou de matières dangereuses.
                </div>
            </details>
            <details class="group bg-white rounded-lg shadow-sm border border-gray-200">
                <summary class="flex justify-between items-center font-medium cursor-pointer list-none p-4">
                    <span>Que faire si mon colis est perdu ou endommagé ?</span>
                    <span class="transition group-open:rotate-180">
                        <svg fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24"><path d="M6 9l6 6 6-6"></path></svg>
                    </span>
                </summary>
                <div class="text-gray-600 mt-0 group-open:animate-fadeIn p-4 pt-0 text-sm">
                    Nous offrons une compensation standard de 2x les frais d'expédition. Pour les objets de valeur, nous recommandons d'ajouter la Protection Premium qui couvre jusqu'à 500 000 FCFA de valeur.
                </div>
            </details>
            <details class="group bg-white rounded-lg shadow-sm border border-gray-200">
                <summary class="flex justify-between items-center font-medium cursor-pointer list-none p-4">
                    <span>Comment dois-je emballer mon article ?</span>
                    <span class="transition group-open:rotate-180">
                        <svg fill="none" height="24" shape-rendering="geometricPrecision" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="1.5" viewBox="0 0 24 24" width="24"><path d="M6 9l6 6 6-6"></path></svg>
                    </span>
                </summary>
                <div class="text-gray-600 mt-0 group-open:animate-fadeIn p-4 pt-0 text-sm">
                    Les articles doivent être solidement emballés pour résister au transport. Les agences Jumia peuvent refuser les articles jugés fragiles s'ils ne sont pas correctement protégés.
                </div>
            </details>
        </div>
    </div>

    <!-- CTA -->
    <div class="bg-jumia-orange">
        <div class="max-w-2xl mx-auto text-center py-16 px-4 sm:py-20 sm:px-6 lg:px-8">
            <h2 class="text-3xl font-extrabold text-white sm:text-4xl">
                <span class="block">Prêt à envoyer un colis ?</span>
                <span class="block text-orange-900 text-2xl mt-2 opacity-80">Trouvez un point relais près de chez vous aujourd'hui.</span>
            </h2>
            <a href="#" class="mt-8 w-full inline-flex items-center justify-center px-5 py-3 border border-transparent text-base font-medium rounded-md text-jumia-orange bg-white hover:bg-gray-50 sm:w-auto">
                Localiser un Point Relais
            </a>
        </div>
    </div>

    <!-- Footer -->

</div>

{#if showFullRates}
    <div class="fixed inset-0 z-[100] flex items-center justify-center p-4 bg-black bg-opacity-50 transition-opacity">
        <div class="bg-white rounded-lg shadow-xl w-full max-w-4xl max-h-[90vh] flex flex-col">
            <div class="flex justify-between items-center p-6 border-b">
                <h3 class="text-2xl font-bold text-gray-900">Grille Tarifaire Complète</h3>
                <button on:click={toggleRates} class="text-gray-400 hover:text-gray-600 focus:outline-none">
                    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                </button>
            </div>
            <div class="flex-1 overflow-y-auto custom-scrollbar">
                <table class="w-full text-sm text-left">
                    <thead class="text-xs text-gray-500 uppercase border-b bg-gray-50 sticky top-0 z-10">
                        <tr>
                            <th class="py-3 px-4">Départ</th>
                            <th class="py-3 px-4">Arrivée</th>
                            <th class="py-3 px-4">Petit Colis</th>
                            <th class="py-3 px-4">Colis Moyen</th>
                            <th class="py-3 px-4">Grand Colis</th>
                            <th class="py-3 px-4">Délai Estimé</th>
                        </tr>
                    </thead>
                    <tbody class="divide-y divide-gray-100">
                        {#each tarifs as tarif}
                            <tr class="hover:bg-gray-50">
                                <td class="py-3 px-4 font-medium text-gray-900">{tarif.depart}</td>
                                <td class="py-3 px-4 font-medium text-gray-900">{tarif.arrivee}</td>
                                <td class="py-3 px-4 text-gray-600 font-bold text-jumia-orange">{tarif.petit} FCFA</td>
                                <td class="py-3 px-4 text-gray-600 font-bold text-jumia-orange">{tarif.moyen} FCFA</td>
                                <td class="py-3 px-4 text-gray-600 font-bold text-jumia-orange">{tarif.grand} FCFA</td>
                                <td class="py-3 px-4 text-gray-500">{tarif.delai}</td>
                            </tr>
                        {/each}
                    </tbody>
                </table>

                <!-- Zones & Villes List -->
                <div class="p-8 border-t bg-gray-50">
                    <h4 class="text-xl font-bold text-gray-900 mb-6">Zones & Localités</h4>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        {#each zoneVilles as item}
                            <div class="bg-white p-5 rounded-xl border border-gray-200 shadow-sm">
                                <p class="font-bold text-jumia-orange mb-2 text-base">{item.zone}</p>
                                <p class="text-sm text-gray-600 leading-relaxed">{item.villes}</p>
                            </div>
                        {/each}
                    </div>
                </div>
            </div>

            <div class="p-6 border-t bg-white text-right rounded-b-lg flex-shrink-0">
                <button on:click={toggleRates} class="bg-gray-800 text-white px-5 py-2 rounded shadow hover:bg-gray-900 transition font-bold">Fermer</button>
            </div>
        </div>
    </div>
{/if}

<style>
    .jumia-orange, .text-jumia-orange { color: #F68B1E; }
    .bg-jumia-orange { background-color: #F68B1E; }
    .jumia-black { color: #282828; }
    .hero-pattern {
        background-color: #f3f4f6;
        background-image: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23e5e7eb' fill-opacity='0.4'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
    }
    .custom-scrollbar::-webkit-scrollbar {
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
    .custom-scrollbar::-webkit-scrollbar-thumb:hover {
        background: #d47619;
    }
</style>
