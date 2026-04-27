<script>
    import logo from './assets/logo.png';
    import Map from './Map.svelte';
    import { onMount } from 'svelte';

    export let onNavigate;

    onMount(() => {
        window.scrollTo(0, 0);
    });
</script>

<div class="min-h-screen bg-[#F8F9FA] font-roboto text-gray-800 flex flex-col">
    <!-- Navigation -->
    <nav class="bg-gray-900 shadow-sm sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-20 items-center">
                <div class="flex items-center gap-2 cursor-pointer" on:click={() => onNavigate('personal')}>
                    <img src={logo} alt="Jumia Logo" style="width: 150px; height: 80px !important; object-fit: contain;">
                    <span class="text-white font-medium text-lg border-l border-gray-700 pl-2 ml-2">Agences</span>
                </div>
                <div class="hidden md:flex space-x-8">
                    <button on:click={() => onNavigate('personal')} class="text-gray-300 hover:text-jumia-orange transition bg-transparent">Particuliers (C2C)</button>
                    <button on:click={() => onNavigate('business')} class="text-gray-300 hover:text-jumia-orange transition bg-transparent">Professionnel (B2C)</button>
                    <button on:click={() => onNavigate('rates')} class="text-gray-300 hover:text-jumia-orange transition bg-transparent">Tarifs</button>
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

    <!-- Header Section -->
    <div class="bg-jumia-orange py-12 px-4 text-center">
        <h1 class="text-3xl md:text-4xl font-black text-white mb-2">Nos Points Relais & Agences</h1>
        <p class="text-white/90 text-sm md:text-base max-w-2xl mx-auto">
            Trouvez l'agence Jumia la plus proche de chez vous pour déposer ou retirer vos colis en toute simplicité.
        </p>
    </div>

    <!-- Map Container -->
    <div class="flex-1 relative">
        <div class="absolute inset-0">
            <Map />
        </div>
    </div>
</div>

<style>
    .font-roboto { font-family: 'Roboto', sans-serif; }
    .jumia-orange { color: #F68B1E; }
    .bg-jumia-orange { background-color: #F68B1E; }
</style>
