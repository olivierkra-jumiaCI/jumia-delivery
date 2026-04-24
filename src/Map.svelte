<script>
    import { onMount } from 'svelte';
    import 'leaflet/dist/leaflet.css';
    import L from 'leaflet';
    import Papa from 'papaparse';

    let mapElement;
    let map;
    let stations = [];
    let searchTerm = "";
    let selectedStation = null;
    let markers = new Map(); // Store markers to manipulate them (open popups, etc)

    $: filteredStations = stations.filter(s => 
        (s.pus && s.pus.toLowerCase().includes(searchTerm.toLowerCase())) ||
        (s.adresse && s.adresse.toLowerCase().includes(searchTerm.toLowerCase()))
    );

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
            iconUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-icon.png',
            shadowUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-shadow.png',
        });

        map = L.map(mapElement).setView([7.54, -5.55], 6);

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; OpenStreetMap contributors'
        }).addTo(map);

        Papa.parse("https://docs.google.com/spreadsheets/d/1M52gDOvkoXZtCA7RSmHM1vy4ksO6H5fdQQ-twAkRqKk/export?format=csv&gid=0", {
            download: true,
            header: true,
            complete: function(results) {
                stations = results.data.filter(s => s.longlat);
                stations.forEach(station => {
                    const parts = station.longlat.split(',');
                    const lat = parseFloat(parts[0].replace(/['"]/g, '').trim());
                    const lng = parseFloat(parts[1].replace(/['"]/g, '').trim());
                    
                    if (!isNaN(lat) && !isNaN(lng)) {
                        const marker = L.marker([lat, lng]).addTo(map);
                        
                        const popupContent = `
                            <div class="text-sm font-sans">
                                <strong class="text-orange-500 block mb-1 text-base">${station.pus || 'Point Relais'}</strong>
                                <p class="mb-1 text-gray-700">${station.adresse || ''}</p>
                                <p class="mb-2 text-gray-500 text-xs">${station.region || ''}</p>
                            </div>
                        `;
                        marker.bindPopup(popupContent);
                        marker.on('click', () => {
                            selectedStation = station;
                        });
                        markers.set(station, marker);
                    }
                });
                
                if (stations.length > 0) {
                    selectedStation = stations[0];
                }
            }
        });

        return () => {
            if (map) map.remove();
        };
    });
</script>

<div class="flex flex-col md:flex-row h-full bg-white rounded-xl shadow-lg overflow-hidden border border-gray-100">
    <!-- Sidebar -->
    <div class="w-full md:w-80 flex flex-col border-r border-gray-100 bg-gray-50">
        <div class="p-4 bg-white border-b border-gray-100">
            <div class="relative">
                <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-gray-400">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                </span>
                <input 
                    type="text" 
                    bind:value={searchTerm}
                    placeholder="Rechercher une station..." 
                    class="w-full pl-10 pr-4 py-2 bg-gray-100 border-none rounded-lg text-sm focus:ring-2 focus:ring-orange-500 transition"
                />
            </div>
        </div>
        
        <div class="flex-1 overflow-y-auto">
            {#each filteredStations as station}
                <button 
                    on:click={() => selectStation(station)}
                    class="w-full text-left p-4 border-b border-gray-100 transition hover:bg-orange-50 {selectedStation === station ? 'bg-jumia-orange text-white' : 'bg-white'}"
                >
                    <p class="font-bold text-sm">{station.pus}</p>
                </button>
            {/each}
        </div>
    </div>

    <!-- Map and Details -->
    <div class="flex-1 flex flex-col relative">
        <div bind:this={mapElement} class="flex-1 z-10 min-h-[300px]"></div>
        
        {#if selectedStation}
            <div class="p-6 bg-white border-t border-gray-100 shadow-2xl z-20">
                <h3 class="text-xl font-bold text-gray-900 mb-1">{selectedStation.pus}</h3>
                <p class="text-sm text-gray-600 mb-1">{selectedStation.adresse}</p>
                {#if selectedStation.repere}
                    <p class="text-xs text-gray-500 mb-4 italic"><span class="font-bold not-italic">Repère:</span> {selectedStation.repere}</p>
                {/if}
                
                <div class="grid grid-cols-1 sm:grid-cols-3 gap-4 text-xs text-gray-500">
                    <div class="flex items-center gap-2">
                        <svg class="w-4 h-4 text-orange-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                        <span>Lun-Ven: 8h-18h</span>
                    </div>
                    <div class="flex items-center gap-2">
                        <svg class="w-4 h-4 text-orange-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"></path></svg>
                        <span>Sam: 9h-17h</span>
                    </div>
                    <div class="flex items-center gap-2">
                        <svg class="w-4 h-4 text-orange-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z"></path></svg>
                        <span>Service Client: +225 01010101</span>
                    </div>
                </div>
            </div>
        {/if}
    </div>
</div>

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
