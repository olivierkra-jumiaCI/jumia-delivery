<script>
    import { onMount } from 'svelte';
    import 'leaflet/dist/leaflet.css';
    import L from 'leaflet';
    import Papa from 'papaparse';

    let mapElement;
    let map;
    let stations = [];
    let expandedRegions = new Set();

    function toggleRegion(region) {
        if (expandedRegions.has(region)) {
            expandedRegions.delete(region);
        } else {
            expandedRegions.add(region);
        }
        expandedRegions = expandedRegions;
    }

    $: filteredStations = stations.filter(s => 
        (s.pus && s.pus.toLowerCase().includes(searchTerm.toLowerCase())) ||
        (s.adresse && s.adresse.toLowerCase().includes(searchTerm.toLowerCase())) ||
        (s.region && s.region.toLowerCase().includes(searchTerm.toLowerCase()))
    );

    $: groupedStations = filteredStations.reduce((acc, s) => {
        const region = s.region || "Autres";
        if (!acc[region]) acc[region] = [];
        acc[region].push(s);
        return acc;
    }, {});

    $: sortedRegions = Object.keys(groupedStations).sort((a, b) => {
        if (a.startsWith('Abidjan')) return -1;
        if (b.startsWith('Abidjan')) return 1;
        return a.localeCompare(b);
    });

    $: if (searchTerm) {
        expandedRegions = new Set(Object.keys(groupedStations));
    }

    function selectStation(station) {
        selectedStation = station;
        if (map && station.longlat) {
            const parts = station.longlat.split(',');
            const lat = parseFloat(parts[0].replace(/['"]/g, '').trim());
            const lng = parseFloat(parts[1].replace(/['"]/g, '').trim());
            if (!isNaN(lat) && !isNaN(lng)) {
                map.setView([lat, lng], 15);
                const marker = markers.get(station);
                if (marker) marker.openPopup();
            }
        }
    }

    onMount(() => {
        delete L.Icon.Default.prototype._getIconUrl;
        L.Icon.Default.mergeOptions({
            iconRetinaUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-icon-2x.png',
            iconUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker.png',
            shadowUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-shadow.png',
        });

        map = L.map(mapElement).setView([7.54, -5.55], 7);

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; OpenStreetMap contributors'
        }).addTo(map);

        Papa.parse("https://docs.google.com/spreadsheets/d/1M52gDOvkoXZtCA7RSmHM1vy4ksO6H5fdQQ-twAkRqKk/export?format=csv&gid=0", {
            download: true,
            header: true,
            complete: function(results) {
                stations = results.data.filter(s => s.longlat && s.pus);
                stations.forEach(station => {
                    const parts = station.longlat.split(',');
                    const lat = parseFloat(parts[0].replace(/['"]/g, '').trim());
                    const lng = parseFloat(parts[1].replace(/['"]/g, '').trim());
                    
                    if (!isNaN(lat) && !isNaN(lng)) {
                        const marker = L.marker([lat, lng]).addTo(map);
                        const popupContent = `
                            <div class="p-2">
                                <h4 class="font-bold text-jumia-orange">${station.pus}</h4>
                                <p class="text-xs text-gray-600">${station.adresse}</p>
                            </div>
                        `;
                        marker.bindPopup(popupContent);
                        marker.on('click', () => {
                            selectedStation = station;
                            // Ensure region is expanded
                            if (station.region) expandedRegions.add(station.region);
                            expandedRegions = expandedRegions;
                        });
                        markers.set(station, marker);
                    }
                });
            }
        });
    });
</script>

<div class="flex flex-col md:flex-row h-full bg-white rounded-xl shadow-lg overflow-hidden border border-gray-100">
    <!-- Sidebar -->
    <div class="w-full md:w-96 flex flex-col border-r border-gray-100 bg-gray-50">
        <div class="p-5 bg-white border-b border-gray-100">
            <div class="relative group">
                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400 group-focus-within:text-jumia-orange transition">
                    <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                </span>
                <input 
                    type="text" 
                    bind:value={searchTerm}
                    placeholder="Chercher une ville ou une localité..." 
                    class="w-full pl-10 pr-4 py-3 bg-gray-50 border-2 border-gray-100 rounded-xl text-sm focus:border-jumia-orange focus:ring-0 focus:bg-white transition"
                />
            </div>
        </div>
        
        <div class="flex-1 overflow-y-auto custom-scrollbar p-3 space-y-2">
            {#each sortedRegions as region}
                <div class="bg-white rounded-xl border border-gray-100 overflow-hidden shadow-sm">
                    <button 
                        on:click={() => toggleRegion(region)}
                        class="w-full flex items-center justify-between p-4 text-left hover:bg-gray-50 transition"
                    >
                        <span class="font-bold text-gray-700 text-sm">{region}</span>
                        <span class="text-jumia-orange">
                            {#if expandedRegions.has(region)}
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 12H4"></path></svg>
                            {:else}
                                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4"></path></svg>
                            {/if}
                        </span>
                    </button>
                    
                    {#if expandedRegions.has(region)}
                        <div class="border-t border-gray-50 divide-y divide-gray-50 bg-gray-50">
                            {#each groupedStations[region] as station}
                                <button 
                                    on:click={() => selectStation(station)}
                                    class="w-full text-left p-4 pl-6 transition hover:bg-white {selectedStation === station ? 'bg-white border-l-4 border-jumia-orange' : ''}"
                                >
                                    <p class="font-semibold text-xs {selectedStation === station ? 'text-jumia-orange' : 'text-gray-600'}">{station.pus}</p>
                                    <p class="text-[10px] text-gray-400 mt-1">{station.adresse}</p>
                                </button>
                            {/each}
                        </div>
                    {/if}
                </div>
            {/each}
            
            {#if sortedRegions.length === 0}
                <div class="p-8 text-center">
                    <p class="text-gray-400 text-sm italic">Aucune agence trouvée</p>
                </div>
            {/if}
        </div>
    </div>

    <!-- Map -->
    <div class="flex-1 relative">
        <div bind:this={mapElement} class="h-full w-full z-10"></div>
        
        {#if selectedStation}
            <div class="absolute bottom-6 left-6 right-6 md:left-auto md:w-80 bg-white p-6 rounded-2xl shadow-2xl z-20 border border-gray-100 animate-slideUp">
                <div class="flex justify-between items-start mb-4">
                    <h3 class="text-lg font-bold text-gray-900 leading-tight">{selectedStation.pus}</h3>
                    <button on:click={() => selectedStation = null} class="text-gray-400 hover:text-gray-600">
                         <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12"></path></svg>
                    </button>
                </div>
                <p class="text-xs text-gray-500 mb-4">{selectedStation.adresse}</p>
                
                <div class="space-y-3">
                    <div class="flex items-center gap-3 text-[10px] text-gray-600">
                        <div class="w-7 h-7 rounded-lg bg-orange-50 flex items-center justify-center flex-shrink-0">
                            <svg class="w-4 h-4 text-jumia-orange" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                        </div>
                        <div>
                            <p class="font-bold">Horaires</p>
                            <p>Lun-Ven: 8h-18h | Sam: 9h-17h</p>
                        </div>
                    </div>
                    <div class="flex items-center gap-3 text-[10px] text-gray-600">
                        <div class="w-7 h-7 rounded-lg bg-orange-50 flex items-center justify-center flex-shrink-0">
                            <svg class="w-4 h-4 text-jumia-orange" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"></path></svg>
                        </div>
                        <div>
                            <p class="font-bold">Contact</p>
                            <p>25 20 00 61 61</p>
                        </div>
                    </div>
                </div>
            </div>
        {/if}
    </div>
</div>>

<style>
    :global(.leaflet-popup-content-wrapper) {
        border-radius: 8px !important;
        padding: 0 !important;
    }
    :global(.leaflet-popup-content) {
        margin: 12px !important;
    }
    .bg-jumia-orange {
        background-color: #F68B1E;
    }
</style>
