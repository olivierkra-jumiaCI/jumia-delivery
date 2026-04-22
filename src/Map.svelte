<script>
    import { onMount } from 'svelte';
    import 'leaflet/dist/leaflet.css';
    import stations from './lib/data/stations.json';
    import L from 'leaflet';

    let mapElement;
    let map;

    onMount(() => {
        // Fix for default icon issues in Leaflet with bundlers
        delete L.Icon.Default.prototype._getIconUrl;
        L.Icon.Default.mergeOptions({
            iconRetinaUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-icon-2x.png',
            iconUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-icon.png',
            shadowUrl: 'https://unpkg.com/leaflet@1.9.4/dist/images/marker-shadow.png',
        });

        // Initialize map centered on Ivory Coast
        map = L.map(mapElement).setView([7.54, -5.55], 6);

        L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
            attribution: '&copy; OpenStreetMap contributors'
        }).addTo(map);

        // Add markers
        stations.forEach(station => {
            if (station.longlat && station.longlat.includes(',')) {
                const parts = station.longlat.split(',');
                // Handle cases where the coordinates might have quotes or spaces
                const lat = parseFloat(parts[0].replace(/['"]/g, '').trim());
                const lng = parseFloat(parts[1].replace(/['"]/g, '').trim());
                
                if (!isNaN(lat) && !isNaN(lng)) {
                    const marker = L.marker([lat, lng]).addTo(map);
                    
                    const popupContent = `
                        <div class="text-sm font-sans">
                            <strong class="text-orange-500 block mb-1 text-base">${station.pus || 'Pickup Station'}</strong>
                            <p class="mb-1 text-gray-700">${station.adresse || 'No address provided'}</p>
                            <p class="mb-2 text-gray-500 text-xs">${station.region || ''}</p>
                            <span class="inline-block px-2 py-1 text-xs font-bold rounded ${station.statut === 'Live' ? 'bg-green-100 text-green-800' : 'bg-red-100 text-red-800'}">
                                ${station.statut || 'Unknown'}
                            </span>
                        </div>
                    `;
                    marker.bindPopup(popupContent);
                }
            }
        });

        return () => {
            if (map) map.remove();
        };
    });
</script>

<div bind:this={mapElement} class="w-full h-full rounded-lg shadow-inner z-10"></div>
