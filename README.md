# food-tracker
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sustainable Food Tracker</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
        }
    </style>
</head>
<body class="min-h-screen bg-gradient-to-br from-green-50 to-blue-50">
    <div id="app" class="p-4 md:p-8">
        <div class="max-w-7xl mx-auto">
            <!-- Header -->
            <div class="text-center mb-8">
                <h1 class="text-4xl md:text-5xl font-bold text-gray-800 mb-2">
                    🍃 Sustainable Food Tracker
                </h1>
                <p class="text-gray-600 text-lg">Compare prices and sustainability for smarter shopping</p>
            </div>

            <!-- Search and Filter -->
            <div class="bg-white rounded-xl shadow-lg p-6 mb-6">
                <div class="flex flex-col gap-4">
                    <div class="flex flex-col md:flex-row gap-4">
                        <div class="flex-1 relative">
                            <input
                                type="text"
                                id="searchInput"
                                placeholder="🔍 Search products..."
                                class="w-full pl-10 pr-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 focus:border-transparent"
                            />
                        </div>
                        <select id="sortSelect" class="px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-green-500 bg-white">
                            <option value="price">Sort by: Best Price</option>
                            <option value="sustainability">Sort by: Most Sustainable</option>
                        </select>
                    </div>

                    <div class="flex flex-wrap gap-2" id="categoryButtons"></div>
                </div>
            </div>

            <!-- Products Grid -->
            <div id="productsGrid" class="grid grid-cols-1 lg:grid-cols-2 gap-6"></div>

            <!-- No Results Message -->
            <div id="noResults" class="text-center py-12 hidden">
                <p class="text-gray-500 text-lg">No products found matching your filters.</p>
            </div>
        </div>
    </div>

    <script>
        // Product Data
        const products = [
            {
                id: 1,
                name: 'Organic Milk 1L',
                category: 'Dairy',
                sustainability: {
                    organic: true,
                    locallySourced: true,
                    carbonFootprint: 'Low',
                    packaging: 'Recyclable'
                },
                prices: [
                    { shop: 'Tesco', price: 1.25, unit: 'per litre', sustainabilityScore: 7 },
                    { shop: 'SuperValu', price: 1.35, unit: 'per litre', sustainabilityScore: 8 },
                    { shop: 'Dunnes', price: 1.20, unit: 'per litre', sustainabilityScore: 7 },
                    { shop: 'Lidl', price: 1.15, unit: 'per litre', sustainabilityScore: 6 }
                ]
            },
            {
                id: 2,
                name: 'White Bread 800g',
                category: 'Bakery',
                sustainability: {
                    organic: false,
                    locallySourced: true,
                    carbonFootprint: 'Low',
                    packaging: 'Paper'
                },
                prices: [
                    { shop: 'Tesco', price: 0.95, unit: 'per loaf', sustainabilityScore: 6 },
                    { shop: 'SuperValu', price: 1.10, unit: 'per loaf', sustainabilityScore: 8 },
                    { shop: 'Dunnes', price: 0.89, unit: 'per loaf', sustainabilityScore: 7 },
                    { shop: 'Lidl', price: 0.75, unit: 'per loaf', sustainabilityScore: 5 }
                ]
            },
            {
                id: 3,
                name: 'Chicken Breast 500g',
                category: 'Meat',
                sustainability: {
                    organic: false,
                    locallySourced: true,
                    carbonFootprint: 'Medium',
                    packaging: 'Recyclable'
                },
                prices: [
                    { shop: 'Tesco', price: 4.50, unit: 'per 500g', sustainabilityScore: 5 },
                    { shop: 'SuperValu', price: 4.75, unit: 'per 500g', sustainabilityScore: 7 },
                    { shop: 'Dunnes', price: 4.25, unit: 'per 500g', sustainabilityScore: 6 },
                    { shop: 'Lidl', price: 3.99, unit: 'per 500g', sustainabilityScore: 4 }
                ]
            },
            {
                id: 4,
                name: 'Bananas 1kg',
                category: 'Produce',
                sustainability: {
                    organic: true,
                    locallySourced: false,
                    carbonFootprint: 'High',
                    packaging: 'None'
                },
                prices: [
                    { shop: 'Tesco', price: 1.49, unit: 'per kg', sustainabilityScore: 6 },
                    { shop: 'SuperValu', price: 1.69, unit: 'per kg', sustainabilityScore: 7 },
                    { shop: 'Dunnes', price: 1.39, unit: 'per kg', sustainabilityScore: 6 },
                    { shop: 'Lidl', price: 1.29, unit: 'per kg', sustainabilityScore: 5 }
                ]
            },
            {
                id: 5,
                name: 'Pasta 500g',
                category: 'Pantry',
                sustainability: {
                    organic: true,
                    locallySourced: false,
                    carbonFootprint: 'Low',
                    packaging: 'Recyclable'
                },
                prices: [
                    { shop: 'Tesco', price: 0.79, unit: 'per pack', sustainabilityScore: 7 },
                    { shop: 'SuperValu', price: 0.89, unit: 'per pack', sustainabilityScore: 8 },
                    { shop: 'Dunnes', price: 0.75, unit: 'per pack', sustainabilityScore: 6 },
                    { shop: 'Lidl', price: 0.65, unit: 'per pack', sustainabilityScore: 5 }
                ]
            },
            {
                id: 6,
                name: 'Irish Cheddar 200g',
                category: 'Dairy',
                sustainability: {
                    organic: false,
                    locallySourced: true,
                    carbonFootprint: 'Medium',
                    packaging: 'Recyclable'
                },
                prices: [
                    { shop: 'Tesco', price: 2.50, unit: 'per pack', sustainabilityScore: 7 },
                    { shop: 'SuperValu', price: 2.75, unit: 'per pack', sustainabilityScore: 8 },
                    { shop: 'Dunnes', price: 2.45, unit: 'per pack', sustainabilityScore: 7 },
                    { shop: 'Lidl', price: 2.20, unit: 'per pack', sustainabilityScore: 6 }
                ]
            },
            {
                id: 7,
                name: 'Organic Carrots 1kg',
                category: 'Produce',
                sustainability: {
                    organic: true,
                    locallySourced: true,
                    carbonFootprint: 'Low',
                    packaging: 'None'
                },
                prices: [
                    { shop: 'Tesco', price: 0.99, unit: 'per kg', sustainabilityScore: 9 },
                    { shop: 'SuperValu', price: 1.15, unit: 'per kg', sustainabilityScore: 9 },
                    { shop: 'Dunnes', price: 0.95, unit: 'per kg', sustainabilityScore: 8 },
                    { shop: 'Lidl', price: 0.85, unit: 'per kg', sustainabilityScore: 8 }
                ]
            },
            {
                id: 8,
                name: 'Brown Rice 1kg',
                category: 'Pantry',
                sustainability: {
                    organic: true,
                    locallySourced: false,
                    carbonFootprint: 'Low',
                    packaging: 'Recyclable'
                },
                prices: [
                    { shop: 'Tesco', price: 2.20, unit: 'per kg', sustainabilityScore: 7 },
                    { shop: 'SuperValu', price: 2.45, unit: 'per kg', sustainabilityScore: 8 },
                    { shop: 'Dunnes', price: 2.10, unit: 'per kg', sustainabilityScore: 7 },
                    { shop: 'Lidl', price: 1.95, unit: 'per kg', sustainabilityScore: 6 }
                ]
            }
        ];

        // State
        let state = {
            searchTerm: '',
            selectedCategory: 'All',
            sortBy: 'price',
            filterOrganic: false,
            filterLocal: false
        };

        const categories = ['All', 'Dairy', 'Bakery', 'Meat', 'Produce', 'Pantry'];

        // Helper Functions
        function getCheapestPrice(prices) {
            return Math.min(...prices.map(p => p.price));
        }

        function getBestOption(prices, sortBy) {
            if (sortBy === 'price') {
                return prices.reduce((best, current) => 
                    current.price < best.price ? current : best
                );
            } else {
                return prices.reduce((best, current) => 
                    current.sustainabilityScore > best.sustainabilityScore ? current : best
                );
            }
        }

        function getSavings(prices) {
            const cheapest = getCheapestPrice(prices);
            const expensive = Math.max(...prices.map(p => p.price));
            return ((expensive - cheapest) / expensive * 100).toFixed(0);
        }

        function getOverallSustainabilityScore(product) {
            const avgShopScore = product.prices.reduce((sum, p) => sum + p.sustainabilityScore, 0) / product.prices.length;
            let productScore = 5;
            if (product.sustainability.organic) productScore += 2;
            if (product.sustainability.locallySourced) productScore += 2;
            if (product.sustainability.carbonFootprint === 'Low') productScore += 1;
            if (product.sustainability.packaging === 'None' || product.sustainability.packaging === 'Recyclable') productScore += 1;
            return Math.min(10, Math.round((avgShopScore + productScore) / 2));
        }

        function getSustainabilityColor(score) {
            if (score >= 8) return 'text-green-600 bg-green-100';
            if (score >= 6) return 'text-yellow-600 bg-yellow-100';
            return 'text-orange-600 bg-orange-100';
        }

        function getCarbonColor(footprint) {
            if (footprint === 'Low') return 'text-green-600';
            if (footprint === 'Medium') return 'text-yellow-600';
            return 'text-red-600';
        }

        function filterProducts() {
            return products.filter(product => {
                const matchesSearch = product.name.toLowerCase().includes(state.searchTerm.toLowerCase());
                const matchesCategory = state.selectedCategory === 'All' || product.category === state.selectedCategory;
                const matchesOrganic = !state.filterOrganic || product.sustainability.organic;
                const matchesLocal = !state.filterLocal || product.sustainability.locallySourced;
                return matchesSearch && matchesCategory && matchesOrganic && matchesLocal;
            });
        }

        // Render Functions
        function renderCategoryButtons() {
            const container = document.getElementById('categoryButtons');
            container.innerHTML = categories.map(cat => `
                <button 
                    onclick="selectCategory('${cat}')"
                    class="px-4 py-2 rounded-lg font-medium transition-colors ${
                        state.selectedCategory === cat
                            ? 'bg-green-600 text-white'
                            : 'bg-gray-100 text-gray-700 hover:bg-gray-200'
                    }"
                >
                    ${cat}
                </button>
            `).join('') + `
                <button 
                    onclick="toggleOrganic()"
                    class="px-4 py-2 rounded-lg font-medium transition-colors flex items-center gap-2 ${
                        state.filterOrganic
                            ? 'bg-green-600 text-white'
                            : 'bg-gray-100 text-gray-700 hover:bg-gray-200'
                    }"
                >
                    🍃 Organic
                </button>
                <button 
                    onclick="toggleLocal()"
                    class="px-4 py-2 rounded-lg font-medium transition-colors flex items-center gap-2 ${
                        state.filterLocal
                            ? 'bg-blue-600 text-white'
                            : 'bg-gray-100 text-gray-700 hover:bg-gray-200'
                    }"
                >
                    🚚 Local
                </button>
            `;
        }

        function renderProducts() {
            const filtered = filterProducts();
            const container = document.getElementById('productsGrid');
            const noResults = document.getElementById('noResults');

            if (filtered.length === 0) {
                container.innerHTML = '';
                noResults.classList.remove('hidden');
                return;
            }

            noResults.classList.add('hidden');
            
            container.innerHTML = filtered.map(product => {
                const bestOption = getBestOption(product.prices, state.sortBy);
                const savings = getSavings(product.prices);
                const sustainabilityScore = getOverallSustainabilityScore(product);
                const otherPrices = product.prices
                    .filter(p => p.shop !== bestOption.shop)
                    .sort((a, b) => state.sortBy === 'price' ? a.price - b.price : b.sustainabilityScore - a.sustainabilityScore);

                return `
                    <div class="bg-white rounded-xl shadow-lg overflow-hidden hover:shadow-xl transition-shadow">
                        <div class="bg-gradient-to-r from-green-500 to-blue-500 p-4">
                            <div class="flex justify-between items-start">
                                <div>
                                    <h3 class="text-xl font-bold text-white mb-2">${product.name}</h3>
                                    <div class="flex gap-2 flex-wrap">
                                        <span class="inline-block bg-white bg-opacity-30 text-white px-3 py-1 rounded-full text-sm">
                                            ${product.category}
                                        </span>
                                        ${product.sustainability.organic ? `
                                            <span class="inline-flex items-center gap-1 bg-green-600 bg-opacity-80 text-white px-3 py-1 rounded-full text-sm">
                                                🍃 Organic
                                            </span>
                                        ` : ''}
                                        ${product.sustainability.locallySourced ? `
                                            <span class="inline-flex items-center gap-1 bg-blue-600 bg-opacity-80 text-white px-3 py-1 rounded-full text-sm">
                                                🚚 Local
                                            </span>
                                        ` : ''}
                                    </div>
                                </div>
                                <div class="bg-white bg-opacity-20 backdrop-blur-sm px-3 py-2 rounded-lg">
                                    <div class="flex items-center gap-1 text-white">
                                        <span class="font-bold">${savings}%</span>
                                    </div>
                                    <div class="text-xs text-white opacity-90">savings</div>
                                </div>
                            </div>
                        </div>

                        <div class="p-4 bg-gradient-to-r from-green-50 to-blue-50 border-b">
                            <div class="flex items-center justify-between mb-3">
                                <div class="flex items-center gap-2">
                                    <span class="font-semibold text-gray-700">🏆 Sustainability Score</span>
                                </div>
                                <div class="px-3 py-1 rounded-full font-bold ${getSustainabilityColor(sustainabilityScore)}">
                                    ${sustainabilityScore}/10
                                </div>
                            </div>
                            <div class="grid grid-cols-2 gap-2 text-sm">
                                <div class="flex items-center gap-2">
                                    <span class="text-gray-600">Carbon: <span class="font-semibold ${getCarbonColor(product.sustainability.carbonFootprint)}">${product.sustainability.carbonFootprint}</span></span>
                                </div>
                                <div class="text-gray-600">
                                    Packaging: <span class="font-semibold text-gray-800">${product.sustainability.packaging}</span>
                                </div>
                            </div>
                        </div>

                        <div class="p-4">
                            <div class="mb-3 p-3 border-2 rounded-lg ${
                                state.sortBy === 'price' 
                                    ? 'bg-green-50 border-green-200'
                                    : 'bg-blue-50 border-blue-200'
                            }">
                                <div class="flex items-center justify-between mb-2">
                                    <div class="flex items-center gap-2">
                                        <span class="font-semibold text-gray-700">🏪 ${bestOption.shop}</span>
                                    </div>
                                    <div class="text-right">
                                        <div class="text-2xl font-bold ${state.sortBy === 'price' ? 'text-green-600' : 'text-blue-600'}">
                                            €${bestOption.price.toFixed(2)}
                                        </div>
                                        <div class="text-xs text-gray-500">${bestOption.unit}</div>
                                    </div>
                                </div>
                                <div class="flex items-center justify-between">
                                    <div class="text-xs font-medium ${state.sortBy === 'price' ? 'text-green-700' : 'text-blue-700'}">
                                        ${state.sortBy === 'price' ? '✓ Best Price' : '✓ Most Sustainable'}
                                    </div>
                                    <div class="px-2 py-1 rounded text-xs font-semibold ${getSustainabilityColor(bestOption.sustainabilityScore)}">
                                        Eco Score: ${bestOption.sustainabilityScore}/10
                                    </div>
                                </div>
                            </div>

                            <div class="space-y-2">
                                ${otherPrices.map(price => `
                                    <div class="flex items-center justify-between p-2 bg-gray-50 rounded-lg">
                                        <div class="flex items-center gap-2">
                                            <span class="text-gray-700">🏪 ${price.shop}</span>
                                            <div class="px-2 py-1 rounded text-xs font-semibold ${getSustainabilityColor(price.sustainabilityScore)}">
                                                ${price.sustainabilityScore}/10
                                            </div>
                                        </div>
                                        <div class="text-right">
                                            <div class="font-semibold text-gray-800">€${price.price.toFixed(2)}</div>
                                            <div class="text-xs text-red-500">
                                                +€${(price.price - bestOption.price).toFixed(2)}
                                            </div>
                                        </div>
                                    </div>
                                `).join('')}
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        // Event Handlers
        function selectCategory(category) {
            state.selectedCategory = category;
            renderCategoryButtons();
            renderProducts();
        }

        function toggleOrganic() {
            state.filterOrganic = !state.filterOrganic;
            renderCategoryButtons();
            renderProducts();
        }

        function toggleLocal() {
            state.filterLocal = !state.filterLocal;
            renderCategoryButtons();
            renderProducts();
        }

        // Initialize
        document.getElementById('searchInput').addEventListener('input', (e) => {
            state.searchTerm = e.target.value;
            renderProducts();
        });

        document.getElementById('sortSelect').addEventListener('change', (e) => {
            state.sortBy = e.target.value;
            renderProducts();
        });

        renderCategoryButtons();
        renderProducts();
    </script>
</body>
</html>
