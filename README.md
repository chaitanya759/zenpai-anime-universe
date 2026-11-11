# zenpai-anime-universe
ZENPAI — Your ultimate destination for anime fans. Discover trending series, characters, news and a vibrant community.
```html
<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ZENPAI — Modern Anime Hub</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        zenNeon: '#00f3ff',
                        zenPurple: '#8b5cf6',
                        zenPink: '#ec4899',
                        zenDark: '#0b0f19',
                        zenCard: '#151c2c',
                        zenCardHover: '#1e293b'
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    },
                    animation: {
                        'pulse-glow': 'pulseGlow 3s infinite alternate',
                        'float': 'float 4s ease-in-out infinite',
                    },
                    keyframes: {
                        pulseGlow: {
                            '0%': { boxShadow: '0 0 15px rgba(0,243,255,0.2)' },
                            '100%': { boxShadow: '0 0 30px rgba(139,92,246,0.6)' }
                        },
                        float: {
                            '0%, 100%': { transform: 'translateY(0px)' },
                            '50%': { transform: 'translateY(-8px)' }
                        }
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome & Google Fonts -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #0b0f19;
            color: #e2e8f0;
            overflow-x: hidden;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #0b0f19;
        }
        ::-webkit-scrollbar-thumb {
            background: #1e293b;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #8b5cf6;
        }

        /* Glassmorphism utility */
        .glass {
            background: rgba(21, 28, 44, 0.75);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.08);
        }

        .glass-nav {
            background: rgba(11, 15, 25, 0.85);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border-bottom: 1px solid rgba(255, 255, 255, 0.06);
        }

        .neon-border {
            box-shadow: 0 0 15px rgba(0, 243, 255, 0.25);
            border: 1px solid rgba(0, 243, 255, 0.4);
        }

        .neon-text-glow {
            text-shadow: 0 0 12px rgba(0, 243, 255, 0.6), 0 0 25px rgba(139, 92, 246, 0.4);
        }

        /* Card Overlay effect */
        .card-gradient-overlay {
            background: linear-gradient(180deg, rgba(0, 0, 0, 0) 0%, rgba(11, 15, 25, 0.95) 85%);
        }
    </style>
</head>
<body class="min-h-screen flex flex-col selection:bg-zenNeon selection:text-black">

    <!-- Toast Notification Container -->
    <div id="toastContainer" class="fixed bottom-6 right-6 z-50 flex flex-col gap-3 pointer-events-none"></div>

    <!-- NAVBAR -->
    <header class="sticky top-0 z-40 glass-nav transition-all duration-300">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-20">
                
                <!-- Logo -->
                <div class="flex items-center gap-3 cursor-pointer" onclick="resetFiltersAndHome()">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-cyan-500 via-purple-600 to-pink-500 p-0.5 shadow-lg shadow-purple-500/30">
                        <div class="w-full h-full bg-zenDark rounded-[10px] flex items-center justify-center">
                            <i class="fa-solid fa-bolt text-zenNeon text-xl animate-pulse"></i>
                        </div>
                    </div>
                    <span class="text-2xl font-black tracking-wider text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-500 neon-text-glow">
                        ZENPAI
                    </span>
                </div>

                <!-- Desktop Navigation Links -->
                <nav class="hidden md:flex items-center space-x-8 text-sm font-semibold">
                    <a href="#hero" class="text-white hover:text-zenNeon transition-colors flex items-center gap-2">
                        <i class="fa-solid fa-fire text-pink-500"></i> Featured
                    </a>
                    <a href="#catalog" class="text-gray-300 hover:text-zenNeon transition-colors">Browse All</a>
                    <a href="#top-rated" onclick="filterByTopRated()" class="text-gray-300 hover:text-zenNeon transition-colors">Top Rated</a>
                    <a href="#genres" onclick="scrollToGenres()" class="text-gray-300 hover:text-zenNeon transition-colors">Genres</a>
                </nav>

                <!-- Action Controls (Search, Watchlist Button) -->
                <div class="flex items-center space-x-4">
                    <!-- Global Search Quick Toggle -->
                    <button onclick="focusSearchInput()" class="p-2.5 rounded-xl text-gray-300 hover:text-white hover:bg-white/10 transition" title="Quick Search">
                        <i class="fa-solid fa-magnifying-glass text-lg"></i>
                    </button>

                    <!-- Watchlist Drawer Toggle Button -->
                    <button onclick="toggleWatchlistDrawer()" class="relative p-2.5 rounded-xl bg-purple-600/20 border border-purple-500/30 text-purple-300 hover:bg-purple-600/30 hover:text-white transition group flex items-center gap-2">
                        <i class="fa-solid fa-bookmark group-hover:scale-110 transition-transform"></i>
                        <span class="hidden sm:inline text-sm font-medium">Watchlist</span>
                        <span id="watchlistBadge" class="absolute -top-1.5 -right-1.5 bg-gradient-to-r from-pink-500 to-purple-600 text-white text-[11px] font-bold w-5 h-5 rounded-full flex items-center justify-center border-2 border-zenDark">0</span>
                    </button>

                    <!-- Mobile Hamburger Menu Button -->
                    <button onclick="toggleMobileMenu()" class="md:hidden p-2.5 rounded-xl text-gray-300 hover:text-white hover:bg-white/10">
                        <i class="fa-solid fa-bars text-xl"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Navigation Menu -->
        <div id="mobileMenu" class="hidden md:hidden border-t border-white/10 bg-zenDark/95 px-4 pt-3 pb-6 space-y-3">
            <a href="#hero" onclick="toggleMobileMenu()" class="block py-2 text-white hover:text-zenNeon font-medium">Featured Spotlight</a>
            <a href="#catalog" onclick="toggleMobileMenu()" class="block py-2 text-gray-300 hover:text-zenNeon font-medium">Browse Catalog</a>
            <a href="#top-rated" onclick="filterByTopRated(); toggleMobileMenu();" class="block py-2 text-gray-300 hover:text-zenNeon font-medium">Top Rated Anime</a>
            <button onclick="toggleWatchlistDrawer(); toggleMobileMenu();" class="w-full text-left py-2 text-purple-400 font-medium flex items-center justify-between">
                <span>View Watchlist</span>
                <span id="mobileWatchlistBadge" class="bg-purple-600 text-white text-xs px-2 py-0.5 rounded-full">0</span>
            </button>
        </div>
    </header>

    <!-- HERO SECTION -->
    <section id="hero" class="relative min-h-[550px] lg:min-h-[620px] flex items-center justify-center overflow-hidden py-12">
        <!-- Background Ambient Glow & Banner -->
        <div class="absolute inset-0 z-0">
            <img id="heroBgImg" src="https://images.unsplash.com/photo-1578632767115-351597cf2477?auto=format&fit=crop&w=1600&q=80" alt="Anime Hero" class="w-full h-full object-cover object-center opacity-30 filter blur-sm scale-105 transition-all duration-700">
            <div class="absolute inset-0 bg-gradient-to-t from-zenDark via-zenDark/80 to-transparent"></div>
            <div class="absolute inset-0 bg-gradient-to-r from-zenDark via-transparent to-zenDark/90"></div>
            <div class="absolute top-1/4 left-1/3 w-96 h-96 bg-cyan-500/10 rounded-full filter blur-[120px] pointer-events-none"></div>
            <div class="absolute bottom-10 right-1/4 w-96 h-96 bg-purple-600/15 rounded-full filter blur-[140px] pointer-events-none"></div>
        </div>

        <div class="relative z-10 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 w-full">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8 items-center">
                
                <!-- Hero Content Left -->
                <div class="lg:col-span-7 space-y-6 text-center lg:text-left">
                    <div class="inline-flex items-center gap-2 px-3.5 py-1.5 rounded-full bg-cyan-500/10 border border-cyan-500/30 text-cyan-400 text-xs font-semibold tracking-wider uppercase">
                        <span class="w-2 h-2 rounded-full bg-cyan-400 animate-ping"></span>
                        Season Highlight • 2026 Edition
                    </div>
                    
                    <h1 id="heroTitle" class="text-4xl sm:text-6xl font-black text-white leading-tight tracking-tight">
                        CYBERPUNK: <span class="text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 via-purple-400 to-pink-500">EDGERUNNERS</span>
                    </h1>

                    <p id="heroSynopsis" class="text-gray-300 text-base sm:text-lg max-w-2xl line-clamp-3 leading-relaxed font-normal">
                        A street kid trying to survive in a technology and body modification-obsessed city of the future. Having everything to lose, he chooses to stay alive by becoming an edgerunner.
                    </p>

                    <div class="flex flex-wrap items-center justify-center lg:justify-start gap-4 pt-2">
                        <button id="heroWatchBtn" onclick="openAnimeModal(featuredSpotlightId)" class="px-8 py-3.5 rounded-xl bg-gradient-to-r from-cyan-500 to-purple-600 hover:from-cyan-400 hover:to-purple-500 text-white font-bold shadow-lg shadow-cyan-500/25 hover:shadow-cyan-500/40 transform hover:-translate-y-0.5 transition flex items-center gap-3">
                            <i class="fa-solid fa-play text-sm"></i> Watch Preview
                        </button>
                        <button id="heroAddBtn" onclick="toggleWatchlistHero()" class="px-7 py-3.5 rounded-xl glass hover:bg-white/10 text-white font-semibold border border-white/15 transition flex items-center gap-2">
                            <i id="heroAddIcon" class="fa-solid fa-plus text-purple-400"></i>
                            <span id="heroAddText">Add to Watchlist</span>
                        </button>
                    </div>

                    <!-- Hero Quick Stats -->
                    <div class="flex items-center justify-center lg:justify-start space-x-6 pt-4 text-xs sm:text-sm text-gray-400">
                        <span class="flex items-center gap-1.5"><i class="fa-solid fa-star text-yellow-400"></i> <strong class="text-white" id="heroRating">8.6</strong>/10</span>
                        <span>•</span>
                        <span class="flex items-center gap-1.5"><i class="fa-solid fa-tv text-cyan-400"></i> <span id="heroEpisodes">10 Episodes</span></span>
                        <span>•</span>
                        <span class="px-2.5 py-0.5 rounded bg-white/10 text-gray-200" id="heroGenre">Sci-Fi, Action</span>
                    </div>
                </div>

                <!-- Hero Spotlight Card Right -->
                <div class="lg:col-span-5 flex justify-center lg:justify-end">
                    <div class="relative w-72 sm:w-80 rounded-2xl overflow-hidden glass p-3 border border-white/10 shadow-2xl group hover:border-cyan-500/50 transition-all duration-500 animate-float">
                        <div class="relative h-96 rounded-xl overflow-hidden">
                            <img id="heroCardImg" src="https://images.unsplash.com/photo-1578632767115-351597cf2477?auto=format&fit=crop&w=800&q=80" alt="Spotlight Anime Cover" class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-700">
                            <div class="absolute inset-0 bg-gradient-to-t from-zenDark via-transparent to-transparent"></div>
                            
                            <span class="absolute top-3 left-3 bg-red-600/90 backdrop-blur-md text-white text-xs font-bold px-2.5 py-1 rounded-md tracking-wider">
                                HOT NOW
                            </span>

                            <div class="absolute bottom-3 left-3 right-3 text-left">
                                <span class="text-xs text-cyan-400 font-semibold uppercase tracking-wider">Next Spotlight</span>
                                <h4 class="text-white font-bold text-lg truncate" id="heroCardTitle">Demon Slayer: Swordsmith</h4>
                                <div class="flex justify-between items-center mt-2">
                                    <button onclick="cycleHeroAnime()" class="text-xs text-purple-300 hover:text-white flex items-center gap-1 bg-purple-900/50 px-2.5 py-1 rounded-lg border border-purple-500/30">
                                        Next Banner <i class="fa-solid fa-arrow-right"></i>
                                    </button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    </section>

    <!-- SEARCH & FILTER CONTROLS SECTION -->
    <section id="catalog" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8 w-full">
        
        <!-- Controls Box -->
        <div class="glass p-5 sm:p-6 rounded-2xl space-y-5 border border-white/10 shadow-xl">
            
            <div class="flex flex-col md:flex-row gap-4 items-center justify-between">
                <!-- Search Input Bar -->
                <div class="relative w-full md:w-96">
                    <i class="fa-solid fa-magnifying-glass absolute left-4 top-1/2 -translate-y-1/2 text-gray-400"></i>
                    <input type="text" id="searchInput" oninput="handleSearch()" placeholder="Search anime title or keywords..." 
                        class="w-full bg-zenDark/80 border border-white/10 rounded-xl pl-11 pr-10 py-3 text-sm text-white placeholder-gray-400 focus:outline-none focus:border-cyan-500 focus:ring-1 focus:ring-cyan-500 transition">
                    <button id="clearSearchBtn" onclick="clearSearch()" class="hidden absolute right-3 top-1/2 -translate-y-1/2 text-gray-400 hover:text-white p-1">
                        <i class="fa-solid fa-xmark"></i>
                    </button>
                </div>

                <!-- Sorting & Quick Filters -->
                <div class="flex items-center gap-3 w-full md:w-auto justify-between md:justify-end">
                    <div class="flex items-center gap-2">
                        <label for="sortSelect" class="text-xs text-gray-400 font-medium whitespace-nowrap">Sort By:</label>
                        <select id="sortSelect" onchange="handleSort()" class="bg-zenDark border border-white/10 text-xs sm:text-sm text-gray-200 rounded-xl px-3 py-2.5 focus:outline-none focus:border-cyan-500">
                            <option value="popularity">Popularity</option>
                            <option value="rating">Highest Rated</option>
                            <option value="title">Alphabetical</option>
                            <option value="episodes">Episode Count</option>
                        </select>
                    </div>

                    <button onclick="resetFilters()" class="text-xs text-purple-400 hover:text-purple-300 font-semibold px-3 py-2 rounded-lg hover:bg-purple-900/30 transition flex items-center gap-1">
                        <i class="fa-solid fa-rotate-right"></i> Reset
                    </button>
                </div>
            </div>

            <!-- Genre Pills Filter -->
            <div class="pt-2 border-t border-white/5">
                <div class="text-xs text-gray-400 font-medium mb-2.5">Filter by Genre:</div>
                <div id="genreContainer" class="flex flex-wrap gap-2">
                    <!-- Genre buttons rendered via JS -->
                </div>
            </div>
        </div>
    </section>

    <!-- ANIME CATALOG GRID SECTION -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 w-full flex-grow">
        <div class="flex justify-between items-center mb-6">
            <div>
                <h2 class="text-2xl font-bold text-white flex items-center gap-2">
                    <i class="fa-solid fa-film text-cyan-400"></i> Anime Collection
                </h2>
                <p id="resultCountText" class="text-xs text-gray-400 mt-0.5">Showing all available titles</p>
            </div>
        </div>

        <!-- Anime Card Grid -->
        <div id="animeGrid" class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-4 sm:gap-6">
            <!-- Anime Cards standard template inserted dynamically -->
        </div>

        <!-- Empty Results Message -->
        <div id="noResults" class="hidden py-16 text-center space-y-4">
            <div class="w-16 h-16 mx-auto rounded-full bg-white/5 flex items-center justify-center text-gray-500 text-2xl">
                <i class="fa-solid fa-ghost"></i>
            </div>
            <h3 class="text-lg font-semibold text-gray-300">No Anime Found</h3>
            <p class="text-sm text-gray-500 max-w-sm mx-auto">Try broadening your search terms or clearing selected genre filters.</p>
            <button onclick="resetFilters()" class="px-5 py-2.5 rounded-xl bg-purple-600 text-white text-xs font-semibold hover:bg-purple-500 transition">
                Reset All Filters
            </button>
        </div>
    </main>

    <!-- FOOTER -->
    <footer class="mt-20 border-t border-white/10 glass">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-10">
            <div class="flex flex-col md:flex-row items-center justify-between gap-6">
                
                <div class="flex items-center gap-3">
                    <div class="w-8 h-8 rounded-lg bg-gradient-to-tr from-cyan-500 to-purple-600 flex items-center justify-center">
                        <i class="fa-solid fa-bolt text-white text-sm"></i>
                    </div>
                    <span class="text-xl font-black text-transparent bg-clip-text bg-gradient-to-r from-cyan-400 to-purple-500">ZENPAI</span>
                    <span class="text-xs text-gray-500 border-l border-gray-700 pl-3">Ultimate Anime Hub</span>
                </div>

                <div class="flex items-center space-x-6 text-xs text-gray-400">
                    <a href="#hero" class="hover:text-cyan-400 transition">Home</a>
                    <a href="#catalog" class="hover:text-cyan-400 transition">Catalog</a>
                    <a href="javascript:void(0)" onclick="toggleWatchlistDrawer()" class="hover:text-cyan-400 transition">My Watchlist</a>
                    <a href="https://github.com" target="_blank" class="hover:text-cyan-400 transition"><i class="fa-brands fa-g
