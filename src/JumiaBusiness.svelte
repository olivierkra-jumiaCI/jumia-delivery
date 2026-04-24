<script>
    import logo from './assets/logo.png';
    export let onNavigate;
</script>

<svelte:head>
    <title>Logistics for Business & Online Sellers | Jumia Delivery</title>
    <link href="https://fonts.googleapis.com/css2?family=Roboto:wght@300;400;500;700;900&display=swap" rel="stylesheet">
</svelte:head>

<div class="bg-gray-50 text-gray-800 font-roboto">
    <!-- Navigation -->
    <nav class="bg-white shadow-sm sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16 items-center">
                <div class="flex items-center gap-2">
                    <!-- Jumia Logo - Using the white one but on white background we need the black one or a dark container -->
                    <!-- Since the provided HTML had a black JUMIA text, I'll use a version that works on white background -->
                    <div class="text-2xl font-black tracking-tighter flex items-center">
                        <span class="text-black">JUMIA</span>
                        <span class="bg-jumia-orange text-white rounded-full w-6 h-6 flex items-center justify-center ml-1 text-sm">★</span>
                    </div>
                    <span class="text-gray-500 font-medium text-lg border-l border-gray-300 pl-2 ml-2">Delivery</span>
                </div>
                <div class="hidden md:flex space-x-8">
                    <button on:click={() => onNavigate('personal')} class="text-gray-500 hover:text-jumia-orange transition bg-transparent">Personnel (C2C)</button>
                    <button on:click={() => onNavigate('business')} class="text-gray-900 font-bold border-b-2 border-jumia-orange bg-transparent">Professionnel (B2C)</button>
                    <a href="#features" class="text-gray-500 hover:text-jumia-orange transition flex items-center">Fonctionnalités</a>
                    <a href="#integration" class="text-gray-500 hover:text-jumia-orange transition flex items-center">API & Intégration</a>
                </div>
                <div class="flex items-center gap-4">
                     <a href="#" class="text-sm font-medium text-gray-600 hover:text-jumia-orange">Connexion</a>
                    <button class="bg-jumia-orange text-white px-5 py-2 rounded shadow hover:bg-orange-600 transition font-bold">
                        Commencer
                    </button>
                </div>
            </div>
        </div>
    </nav>

    <!-- Hero Section with Form -->
    <div class="relative bg-gray-900 overflow-hidden">
        <div class="absolute inset-0">
             <!-- Abstract Background -->
            <img class="w-full h-full object-cover opacity-20" src="https://images.unsplash.com/photo-1586528116311-ad8dd3c8310d?ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80" alt="Warehouse logistics">
            <div class="absolute inset-0 bg-gradient-to-r from-gray-900 via-gray-900 to-transparent"></div>
        </div>
        
        <div class="relative max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-16 md:py-24">
            <div class="lg:grid lg:grid-cols-12 lg:gap-8">
                
                <!-- Hero Copy -->
                <div class="sm:text-center md:max-w-2xl md:mx-auto lg:col-span-7 lg:text-left flex flex-col justify-center">
                    <div class="inline-flex items-center px-3 py-1 rounded-full bg-orange-900/50 border border-orange-500 text-orange-400 text-xs font-bold tracking-wider uppercase mb-6 w-max">
                        Pour les vendeurs en ligne et entreprises
                    </div>
                    <h1 class="text-4xl tracking-tight font-extrabold text-white sm:text-5xl md:text-6xl">
                        <span class="block">Développez votre business</span>
                        <span class="block text-jumia-orange">avec une logistique fiable.</span>
                    </h1>
                    <p class="mt-4 text-lg text-gray-300">
                        Arrêtez de vous soucier de la livraison et concentrez-vous sur vos ventes. Du **Paiement à la Livraison** aux **Expéditions en Masse**, nous fournissons l'infrastructure de confiance.
                    </p>
                    
                    <div class="mt-8 flex flex-col sm:flex-row gap-4 justify-center lg:justify-start">
                        <div class="flex items-center gap-2 text-gray-300 text-sm font-medium">
                            <svg class="w-5 h-5 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
                            <span>Couverture Nationale</span>
                        </div>
                        <div class="flex items-center gap-2 text-gray-300 text-sm font-medium">
                            <svg class="w-5 h-5 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
                            <span>Paiements Rapides</span>
                        </div>
                        <div class="flex items-center gap-2 text-gray-300 text-sm font-medium">
                            <svg class="w-5 h-5 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
                            <span>Suivi en Temps Réel</span>
                        </div>
                    </div>
                </div>

                <!-- Lead Gen Form -->
                <div class="mt-12 lg:mt-0 lg:col-span-5">
                    <div class="bg-white sm:w-full sm:mx-auto sm:rounded-lg sm:overflow-hidden shadow-2xl">
                        <div class="px-4 py-8 sm:px-10">
                            <div class="text-center mb-6">
                                <h3 class="text-xl font-bold text-gray-900">Ouvrez un compte Business</h3>
                                <p class="text-sm text-gray-500">Inscrivez-vous pour voir les tarifs spéciaux.</p>
                            </div>
                            <form action="#" method="POST" class="space-y-4" on:submit|preventDefault>
                                <div>
                                    <label for="company-name" class="sr-only">Nom de l'entreprise</label>
                                    <input type="text" name="company-name" id="company-name" placeholder="Nom de votre Boutique / Entreprise" class="block w-full px-4 py-3 border border-gray-300 rounded-md placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-jumia-orange focus:border-transparent">
                                </div>
                                <div>
                                    <label for="email" class="sr-only">Email professionnel</label>
                                    <input type="email" name="email" id="email" placeholder="Adresse Email" class="block w-full px-4 py-3 border border-gray-300 rounded-md placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-jumia-orange focus:border-transparent">
                                </div>
                                <div>
                                    <label for="phone" class="sr-only">Numéro de téléphone</label>
                                    <input type="tel" name="phone" id="phone" placeholder="Numéro de téléphone" class="block w-full px-4 py-3 border border-gray-300 rounded-md placeholder-gray-500 focus:outline-none focus:ring-2 focus:ring-jumia-orange focus:border-transparent">
                                </div>
                                <div>
                                    <label for="volume" class="sr-only">Volume mensuel</label>
                                    <select id="volume" name="volume" class="block w-full px-4 py-3 border border-gray-300 rounded-md text-gray-500 focus:outline-none focus:ring-2 focus:ring-jumia-orange focus:border-transparent">
                                        <option value="" disabled selected>Volume mensuel estimé</option>
                                        <option value="0-50">0 - 50 colis</option>
                                        <option value="51-200">51 - 200 colis</option>
                                        <option value="201-1000">201 - 1,000 colis</option>
                                        <option value="1000+">1,000+ colis</option>
                                    </select>
                                </div>
                                <div class="pt-2">
                                    <button type="submit" class="w-full flex justify-center py-3 px-4 border border-transparent rounded-md shadow-sm text-sm font-bold text-white bg-jumia-orange hover:bg-orange-600 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-jumia-orange transition">
                                        Demander un compte
                                    </button>
                                </div>
                                <p class="text-xs text-center text-gray-400 mt-2">
                                    En cliquant sur Demander un compte, vous acceptez nos Conditions de Service.
                                </p>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Value Proposition Grid -->
    <div id="features" class="py-16 bg-white">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="text-center">
                <h2 class="text-base font-semibold text-jumia-orange tracking-wide uppercase">Pourquoi Jumia Delivery ?</h2>
                <p class="mt-2 text-3xl leading-8 font-extrabold tracking-tight text-gray-900 sm:text-4xl">
                    Conçu pour le commerce moderne
                </p>
                <p class="mt-4 max-w-2xl text-xl text-gray-500 mx-auto">
                    Que vous vendiez sur Instagram, ayez votre propre site ou gériez une grande entreprise, nos outils sont conçus pour vous aider à grandir.
                </p>
            </div>

            <div class="mt-16">
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
                    
                    <!-- Feature 1 -->
                    <div class="relative p-6 border border-gray-100 rounded-2xl bg-gray-50 hover:shadow-lg transition">
                        <div class="absolute top-6 right-6 text-gray-200">
                             <svg class="w-16 h-16 opacity-20" fill="currentColor" viewBox="0 0 20 20"><path d="M4 4a2 2 0 00-2 2v4a2 2 0 002 2V6h10a2 2 0 00-2-2H4zm2 6a2 2 0 012-2h8a2 2 0 012 2v4a2 2 0 01-2 2H8a2 2 0 01-2-2v-4zm6 4a2 2 0 100-4 2 2 0 000 4z"></path></svg>
                        </div>
                        <h3 class="text-lg font-bold text-gray-900 mb-2">Paiement à la Livraison (COD)</h3>
                        <p class="text-gray-500 text-sm">Boostez vos ventes en proposant le COD. Nous collectons les paiements lors de la livraison et vous les reversons directement de manière sécurisée.</p>
                    </div>

                    <!-- Feature 2 -->
                    <div class="relative p-6 border border-gray-100 rounded-2xl bg-gray-50 hover:shadow-lg transition">
                        <div class="absolute top-6 right-6 text-gray-200">
                             <svg class="w-16 h-16 opacity-20" fill="currentColor" viewBox="0 0 20 20"><path d="M7 3a1 1 0 000 2h6a1 1 0 100-2H7zM4 7a1 1 0 011-1h10a1 1 0 110 2H5a1 1 0 01-1-1zM2 11a2 2 0 012-2h12a2 2 0 012 2v4a2 2 0 01-2 2H4a2 2 0 01-2-2v-4z"></path></svg>
                        </div>
                        <h3 class="text-lg font-bold text-gray-900 mb-2">Expéditions en Masse</h3>
                        <p class="text-gray-500 text-sm">Vous avez 100 commandes aujourd'hui ? Ne les saisissez pas une par une. Téléchargez un fichier CSV pour créer des centaines d'envois et imprimer vos étiquettes instantanément.</p>
                    </div>

                    <!-- Feature 3 -->
                    <div class="relative p-6 border border-gray-100 rounded-2xl bg-gray-50 hover:shadow-lg transition">
                        <div class="absolute top-6 right-6 text-gray-200">
                             <svg class="w-16 h-16 opacity-20" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm1-12a1 1 0 10-2 0v4a1 1 0 00.293.707l2.828 2.829a1 1 0 101.415-1.415L11 9.586V6z" clip-rule="evenodd"></path></svg>
                        </div>
                        <h3 class="text-lg font-bold text-gray-900 mb-2">Tableau de Bord Business</h3>
                        <p class="text-gray-500 text-sm">Suivez tous vos envois, surveillez vos taux de réussite et gérez vos rapports financiers depuis un panneau de contrôle intuitif.</p>
                    </div>

                     <!-- Feature 4 -->
                    <div class="relative p-6 border border-gray-100 rounded-2xl bg-gray-50 hover:shadow-lg transition">
                        <div class="absolute top-6 right-6 text-gray-200">
                             <svg class="w-16 h-16 opacity-20" fill="currentColor" viewBox="0 0 20 20"><path d="M11 3a1 1 0 100 2h2.586l-6.293 6.293a1 1 0 101.414 1.414L15 6.414V9a1 1 0 102 0V4a1 1 0 00-1-1h-5z"></path><path d="M5 5a2 2 0 00-2 2v8a2 2 0 002 2h8a2 2 0 002-2v-3a1 1 0 10-2 0v3H5V7h3a1 1 0 000-2H5z"></path></svg>
                        </div>
                        <h3 class="text-lg font-bold text-gray-900 mb-2">Gestion des Retours</h3>
                        <p class="text-gray-500 text-sm">Les retours arrivent. Nous les rendons sans douleur. Les livraisons échouées vous sont retournées efficacement, avec des mises à jour claires.</p>
                    </div>

                     <!-- Feature 5 -->
                    <div class="relative p-6 border border-gray-100 rounded-2xl bg-gray-50 hover:shadow-lg transition">
                        <div class="absolute top-6 right-6 text-gray-200">
                             <svg class="w-16 h-16 opacity-20" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M2.166 4.999A11.954 11.954 0 0010 1.944 11.954 11.954 0 0017.834 5c.11.65.166 1.32.166 2.001 0 5.225-3.34 9.67-8 11.317C5.34 16.67 2 12.225 2 7c0-.682.057-1.35.166-2.001zm11.541 3.708a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd"></path></svg>
                        </div>
                        <h3 class="text-lg font-bold text-gray-900 mb-2">Assurance Premium</h3>
                        <p class="text-gray-500 text-sm">Protégez vos articles de valeur. L'assurance premium optionnelle couvre jusqu'à 500 000 FCFA contre la perte ou le dommage.</p>
                    </div>

                     <!-- Feature 6 -->
                    <div class="relative p-6 border border-gray-100 rounded-2xl bg-gray-50 hover:shadow-lg transition">
                        <div class="absolute top-6 right-6 text-gray-200">
                             <svg class="w-16 h-16 opacity-20" fill="currentColor" viewBox="0 0 20 20"><path fill-rule="evenodd" d="M4 2a2 2 0 00-2 2v11a3 3 0 106 0V4a2 2 0 00-2-2H4zm1 14a1 1 0 100-2 1 1 0 000 2zm5-1.757l4.9-4.9a2 2 0 000-2.828L13.485 5.1a2 2 0 00-2.828 0L10 5.757v8.486zM16 18H9.071l6-6H16a2 2 0 012 2v2a2 2 0 01-2 2z" clip-rule="evenodd"></path></svg>
                        </div>
                        <h3 class="text-lg font-bold text-gray-900 mb-2">Support Dédié</h3>
                        <p class="text-gray-500 text-sm">Les gros vendeurs ont accès à des gestionnaires de compte dédiés pour résoudre les problèmes rapidement et optimiser votre logistique.</p>
                    </div>

                </div>
            </div>
        </div>
    </div>

    <!-- Integration Section -->
    <div id="integration" class="bg-gray-900 py-16 text-white">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 flex flex-col md:flex-row items-center gap-12">
            <div class="md:w-1/2">
                <span class="text-jumia-orange font-bold uppercase tracking-wide text-sm">Pour les Développeurs</span>
                <h2 class="text-3xl font-extrabold mt-2 mb-4">Intégration API Transparente</h2>
                <p class="text-gray-300 text-lg mb-6">
                    Connectez votre boutique en ligne directement à Jumia Delivery. Automatisez la création de commandes, récupérez les tarifs et les mises à jour de suivi sans quitter votre système.
                </p>
                <ul class="space-y-3 mb-8">
                    <li class="flex items-center gap-3">
                        <svg class="w-5 h-5 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
                        <span>Architecture API RESTful</span>
                    </li>
                    <li class="flex items-center gap-3">
                        <svg class="w-5 h-5 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
                        <span>Webhooks pour les statuts de livraison</span>
                    </li>
                    <li class="flex items-center gap-3">
                        <svg class="w-5 h-5 text-green-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M5 13l4 4L19 7"></path></svg>
                        <span>Environnement Sandbox pour les tests</span>
                    </li>
                </ul>
                <a href="#" class="text-white border border-gray-600 px-6 py-3 rounded hover:bg-gray-800 transition inline-block">Voir la Documentation API</a>
            </div>
            <div class="md:w-1/2 bg-gray-800 rounded-lg p-6 font-mono text-sm text-green-400 shadow-2xl border border-gray-700 w-full overflow-hidden">
                <div class="flex gap-2 mb-4 border-b border-gray-700 pb-2">
                    <div class="w-3 h-3 rounded-full bg-red-500"></div>
                    <div class="w-3 h-3 rounded-full bg-yellow-500"></div>
                    <div class="w-3 h-3 rounded-full bg-green-500"></div>
                </div>
                <p class="text-gray-500"># Créer un envoi via API</p>
                <p><span class="text-purple-400">POST</span> /api/v1/shipments</p>
                <p>{"{"}</p>
                <p class="pl-4">"recipient": {"{"}</p>
                <p class="pl-8">"name": "Jean Koffi",</p>
                <p class="pl-8">"phone": "+225...",</p>
                <p class="pl-8">"city": "Abidjan"</p>
                <p class="pl-4">{"}"},</p>
                <p class="pl-4">"package": {"{"}</p>
                <p class="pl-8">"weight": 2.5,</p>
                <p class="pl-8">"cod_amount": 15000</p>
                <p class="pl-4">{"}"}</p>
                <p>{"}"}</p>
                <p class="mt-4 text-white">Status: <span class="text-green-400">201 Created</span></p>
            </div>
        </div>
    </div>

    <!-- Stats -->
    <div class="py-16 bg-orange-50">
        <div class="max-w-7xl mx-auto px-4 text-center">
            <h2 class="text-3xl font-bold text-gray-900 mb-12">Fait confiance par plus de 10 000 entreprises</h2>
            <div class="grid grid-cols-2 md:grid-cols-4 gap-8">
                <div>
                    <div class="text-4xl font-black text-jumia-orange mb-2">283+</div>
                    <div class="text-gray-600 font-medium">Points Relais</div>
                </div>
                <div>
                    <div class="text-4xl font-black text-jumia-orange mb-2">107</div>
                    <div class="text-gray-600 font-medium">Villes Couvertes</div>
                </div>
                 <div>
                    <div class="text-4xl font-black text-jumia-orange mb-2">1M+</div>
                    <div class="text-gray-600 font-medium">Colis Livrés</div>
                </div>
                 <div>
                    <div class="text-4xl font-black text-jumia-orange mb-2">99.5%</div>
                    <div class="text-gray-600 font-medium">Fiabilité de Paiement</div>
                </div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-gray-900 text-gray-400 py-12">
        <div class="max-w-7xl mx-auto px-4 grid grid-cols-1 md:grid-cols-4 gap-8">
            <div>
                <h3 class="text-white font-bold mb-4 uppercase text-sm tracking-wider">À propos de Jumia Delivery</h3>
                <p class="text-sm leading-relaxed">Aider les entreprises à se développer grâce à une infrastructure logistique fiable à travers l'Afrique.</p>
            </div>
            <div>
                <h3 class="text-white font-bold mb-4 uppercase text-sm tracking-wider">Solutions Business</h3>
                <ul class="space-y-2 text-sm">
                    <li><a href="#" class="hover:text-white">Grille Tarifaire</a></li>
                    <li><a href="#" class="hover:text-white">API Docs</a></li>
                    <li><a href="#" class="hover:text-white">Outil d'expédition en masse</a></li>
                </ul>
            </div>
            <div>
                <h3 class="text-white font-bold mb-4 uppercase text-sm tracking-wider">Support</h3>
                <ul class="space-y-2 text-sm">
                    <li><a href="#" class="hover:text-white">Centre d'Aide</a></li>
                    <li><a href="#" class="hover:text-white">Contacter les Ventes</a></li>
                    <li><a href="#" class="hover:text-white">Signaler un Problème</a></li>
                </ul>
            </div>
            <div>
                <h3 class="text-white font-bold mb-4 uppercase text-sm tracking-wider">Suivez-nous</h3>
                <div class="flex space-x-4">
                   <div class="w-8 h-8 bg-gray-700 rounded-full"></div>
                   <div class="w-8 h-8 bg-gray-700 rounded-full"></div>
                   <div class="w-8 h-8 bg-gray-700 rounded-full"></div>
                </div>
                <p class="text-xs mt-4">© 2025 Jumia. Tous droits réservés.</p>
            </div>
        </div>
    </footer>
</div>

<style>
    .font-roboto { font-family: 'Roboto', sans-serif; }
    .jumia-orange { color: #F68B1E; }
    .bg-jumia-orange { background-color: #F68B1E; }
</style>
