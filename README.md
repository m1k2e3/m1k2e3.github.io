
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Complete Periodic Table Explorer | Glassmorphism</title>
    <style>
        :root {
            --primary-color: rgba(16, 14, 41, 0.85);
            --secondary-color: rgba(255, 255, 255, 0.1);
            --text-color: #ffffff;
            --highlight-color: rgba(98, 0, 255, 0.5);
            --border-color: rgba(255, 255, 255, 0.2);
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0f0524 0%, #1a0933 50%, #2a0b4a 100%);
            color: var(--text-color);
            min-height: 100vh;
            padding: 20px;
            overflow-x: hidden;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: var(--primary-color);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 15px;
            border: 1px solid var(--border-color);
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
        }

        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
            background: linear-gradient(90deg, #ffffff, #a5a5a5);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .subtitle {
            font-size: 1rem;
            opacity: 0.8;
        }

        .periodic-table {
            display: grid;
            grid-template-columns: repeat(18, 1fr);
            grid-gap: 5px;
            margin-bottom: 30px;
        }

        .element {
            position: relative;
            padding: 10px 5px;
            background: var(--primary-color);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-radius: 8px;
            border: 1px solid var(--border-color);
            box-shadow: 0 4px 16px rgba(0, 0, 0, 0.2);
            transition: all 0.3s ease;
            cursor: pointer;
            text-align: center;
            height: 80px;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .element:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.4);
        }

        .element.active {
            transform: scale(1.05);
            background: var(--highlight-color);
            z-index: 10;
        }

        .element .number {
            font-size: 0.7rem;
            position: absolute;
            top: 5px;
            left: 5px;
            opacity: 0.8;
        }

        .element .symbol {
            font-size: 1.5rem;
            font-weight: bold;
            margin-top: 10px;
        }

        .element .name {
            font-size: 0.6rem;
            opacity: 0.8;
            margin-top: 5px;
        }

        .element .mass {
            font-size: 0.6rem;
            position: absolute;
            bottom: 5px;
            right: 5px;
            opacity: 0.8;
        }

        /* Element category colors */
        .alkali-metal { background: rgba(255, 100, 100, 0.3) !important; }
        .alkaline-earth { background: rgba(255, 180, 100, 0.3) !important; }
        .transition-metal { background: rgba(255, 255, 100, 0.3) !important; }
        .post-transition-metal { background: rgba(100, 255, 100, 0.3) !important; }
        .metalloid { background: rgba(100, 255, 180, 0.3) !important; }
        .nonmetal { background: rgba(100, 180, 255, 0.3) !important; }
        .halogen { background: rgba(180, 100, 255, 0.3) !important; }
        .noble-gas { background: rgba(255, 100, 255, 0.3) !important; }
        .lanthanide { background: rgba(255, 100, 180, 0.3) !important; }
        .actinide { background: rgba(180, 100, 100, 0.3) !important; }
        .unknown { background: rgba(150, 150, 150, 0.3) !important; }

        /* Empty cells for periodic table layout */
        .empty {
            visibility: hidden;
        }

        /* Element details panel */
        .element-details {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 90%;
            max-width: 600px;
            max-height: 80vh;
            background: var(--primary-color);
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            border-radius: 20px;
            border: 1px solid var(--border-color);
            box-shadow: 0 15px 50px rgba(0, 0, 0, 0.4);
            padding: 30px;
            z-index: 100;
            overflow-y: auto;
            display: none;
        }

        .element-details.active {
            display: block;
        }

        .element-details-header {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
        }

        .element-details-symbol {
            font-size: 3rem;
            font-weight: bold;
            margin-right: 20px;
            min-width: 60px;
            text-align: center;
        }

        .element-details-name {
            font-size: 1.8rem;
            margin-bottom: 5px;
        }

        .element-details-number {
            opacity: 0.7;
            font-size: 1rem;
        }

        .element-details-category {
            display: inline-block;
            padding: 3px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            margin-top: 5px;
            background: var(--secondary-color);
        }

        .element-details-body {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .element-details-properties, .element-details-uses {
            background: var(--secondary-color);
            padding: 15px;
            border-radius: 10px;
        }

        .element-details-properties h3, .element-details-uses h3 {
            margin-bottom: 10px;
            font-size: 1.2rem;
            color: #ffffff;
        }

        .property {
            margin-bottom: 8px;
            font-size: 0.9rem;
        }

        .property-name {
            font-weight: bold;
            opacity: 0.8;
        }

        .close-btn {
            position: absolute;
            top: 20px;
            right: 20px;
            background: none;
            border: none;
            color: var(--text-color);
            font-size: 1.5rem;
            cursor: pointer;
            opacity: 0.7;
            transition: opacity 0.3s;
        }

        .close-btn:hover {
            opacity: 1;
        }

        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            backdrop-filter: blur(5px);
            -webkit-backdrop-filter: blur(5px);
            z-index: 50;
            display: none;
        }

        .overlay.active {
            display: block;
        }

        /* Legend */
        .legend {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
            margin-bottom: 20px;
            justify-content: center;
        }

        .legend-item {
            display: flex;
            align-items: center;
            font-size: 0.8rem;
            padding: 5px 10px;
            border-radius: 20px;
            background: var(--secondary-color);
        }

        .legend-color {
            width: 15px;
            height: 15px;
            border-radius: 50%;
            margin-right: 5px;
        }

        /* Search and filter */
        .controls {
            display: flex;
            justify-content: center;
            margin-bottom: 20px;
            gap: 15px;
            flex-wrap: wrap;
        }

        .search-box {
            padding: 10px 15px;
            border-radius: 30px;
            border: 1px solid var(--border-color);
            background: var(--primary-color);
            color: var(--text-color);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            width: 300px;
            max-width: 100%;
            outline: none;
        }

        .search-box::placeholder {
            color: rgba(255, 255, 255, 0.6);
        }

        .category-filter {
            padding: 10px 15px;
            border-radius: 30px;
            border: 1px solid var(--border-color);
            background: var(--primary-color);
            color: var(--text-color);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            outline: none;
            cursor: pointer;
        }

        .category-filter option {
            background: var(--primary-color);
            color: var(--text-color);
        }

        /* Responsive adjustments */
        @media (max-width: 768px) {
            .periodic-table {
                grid-template-columns: repeat(9, 1fr);
            }
            
            .element-details-body {
                grid-template-columns: 1fr;
            }
        }

        @media (max-width: 480px) {
            .periodic-table {
                grid-template-columns: repeat(5, 1fr);
            }
            
            .element {
                height: 70px;
            }
            
            .element .symbol {
                font-size: 1.2rem;
            }
            
            .element .name {
                font-size: 0.5rem;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Complete Periodic Table Explorer</h1>
            <p class="subtitle">All 118 elements with properties and uses - Glassmorphism Style</p>
        </header>

        <div class="controls">
            <input type="text" class="search-box" placeholder="Search elements by name, symbol, or number...">
            <select class="category-filter">
                <option value="all">All Categories</option>
                <option value="alkali-metal">Alkali Metals</option>
                <option value="alkaline-earth">Alkaline Earth Metals</option>
                <option value="transition-metal">Transition Metals</option>
                <option value="post-transition-metal">Post-Transition Metals</option>
                <option value="metalloid">Metalloids</option>
                <option value="nonmetal">Nonmetals</option>
                <option value="halogen">Halogens</option>
                <option value="noble-gas">Noble Gases</option>
                <option value="lanthanide">Lanthanides</option>
                <option value="actinide">Actinides</option>
            </select>
        </div>

        <div class="legend">
            <div class="legend-item">
                <div class="legend-color alkali-metal"></div>
                <span>Alkali Metals</span>
            </div>
            <div class="legend-item">
                <div class="legend-color alkaline-earth"></div>
                <span>Alkaline Earth</span>
            </div>
            <div class="legend-item">
                <div class="legend-color transition-metal"></div>
                <span>Transition Metals</span>
            </div>
            <div class="legend-item">
                <div class="legend-color post-transition-metal"></div>
                <span>Post-Transition</span>
            </div>
            <div class="legend-item">
                <div class="legend-color metalloid"></div>
                <span>Metalloids</span>
            </div>
            <div class="legend-item">
                <div class="legend-color nonmetal"></div>
                <span>Nonmetals</span>
            </div>
            <div class="legend-item">
                <div class="legend-color halogen"></div>
                <span>Halogens</span>
            </div>
            <div class="legend-item">
                <div class="legend-color noble-gas"></div>
                <span>Noble Gases</span>
            </div>
            <div class="legend-item">
                <div class="legend-color lanthanide"></div>
                <span>Lanthanides</span>
            </div>
            <div class="legend-item">
                <div class="legend-color actinide"></div>
                <span>Actinides</span>
            </div>
        </div>

        <div class="periodic-table" id="periodicTable">
            <!-- Elements will be inserted here by JavaScript -->
        </div>
    </div>

    <div class="overlay" id="overlay"></div>

    <div class="element-details" id="elementDetails">
        <button class="close-btn" id="closeBtn">&times;</button>
        <div class="element-details-header">
            <div class="element-details-symbol" id="detailSymbol">H</div>
            <div>
                <h2 class="element-details-name" id="detailName">Hydrogen</h2>
                <div class="element-details-number" id="detailNumber">Atomic Number: 1</div>
                <div class="element-details-category" id="detailCategory">Nonmetal</div>
            </div>
        </div>
        <div class="element-details-body">
            <div class="element-details-properties">
                <h3>Properties</h3>
                <div id="detailProperties">
                    <!-- Properties will be inserted here -->
                </div>
            </div>
            <div class="element-details-uses">
                <h3>Uses</h3>
                <div id="detailUses">
                    <!-- Uses will be inserted here -->
                </div>
            </div>
        </div>
    </div>

    <script>
        // Complete element data (1-118)
        const elements = [
            // Period 1
            {
                number: 1,
                symbol: "H",
                name: "Hydrogen",
                mass: "1.008",
                category: "nonmetal",
                group: 1,
                period: 1,
                properties: {
                    "Atomic Mass": "1.008 u",
                    "Phase": "Gas",
                    "Density": "0.00008988 g/cm³",
                    "Melting Point": "-259.16 °C",
                    "Boiling Point": "-252.87 °C",
                    "Electronegativity": "2.20",
                    "Discovered": "1766 by Henry Cavendish"
                },
                uses: [
                    "Ammonia production for fertilizers",
                    "Rocket fuel for space exploration",
                    "Hydrogenation of fats and oils",
                    "Coolant in power generators",
                    "Potential clean energy source in fuel cells"
                ]
            },
            {
                number: 2,
                symbol: "He",
                name: "Helium",
                mass: "4.0026",
                category: "noble-gas",
                group: 18,
                period: 1,
                properties: {
                    "Atomic Mass": "4.0026 u",
                    "Phase": "Gas",
                    "Density": "0.0001785 g/cm³",
                    "Melting Point": "-272.20 °C (at 2.5 MPa)",
                    "Boiling Point": "-268.93 °C",
                    "Electronegativity": "N/A",
                    "Discovered": "1868 by Pierre Janssen and Norman Lockyer"
                },
                uses: [
                    "Cryogenics for superconducting magnets (MRI machines)",
                    "Filling balloons and airships",
                    "Shielding gas in welding",
                    "Pressurizing liquid-fueled rockets",
                    "Leak detection in high-vacuum equipment"
                ]
            },
            // Period 2
            {
                number: 3,
                symbol: "Li",
                name: "Lithium",
                mass: "6.94",
                category: "alkali-metal",
                group: 1,
                period: 2,
                properties: {
                    "Atomic Mass": "6.94 u",
                    "Phase": "Solid",
                    "Density": "0.534 g/cm³",
                    "Melting Point": "180.54 °C",
                    "Boiling Point": "1342 °C",
                    "Electronegativity": "0.98",
                    "Discovered": "1817 by Johan August Arfwedson"
                },
                uses: [
                    "Lithium-ion batteries for electronics and electric vehicles",
                    "Mood-stabilizing drugs (bipolar disorder treatment)",
                    "Heat-resistant glass and ceramics",
                    "Lightweight alloys for aerospace",
                    "Nuclear fusion reactions"
                ]
            },
            {
                number: 4,
                symbol: "Be",
                name: "Beryllium",
                mass: "9.0122",
                category: "alkaline-earth",
                group: 2,
                period: 2,
                properties: {
                    "Atomic Mass": "9.0122 u",
                    "Phase": "Solid",
                    "Density": "1.85 g/cm³",
                    "Melting Point": "1287 °C",
                    "Boiling Point": "2469 °C",
                    "Electronegativity": "1.57",
                    "Discovered": "1798 by Louis Nicolas Vauquelin"
                },
                uses: [
                    "X-ray windows and radiation shielding",
                    "High-performance alloys for aerospace",
                    "Gyroscopes and inertial guidance systems",
                    "Nuclear reactors as neutron reflectors",
                    "Precision instruments and mirrors"
                ]
            },
            {
                number: 5,
                symbol: "B",
                name: "Boron",
                mass: "10.81",
                category: "metalloid",
                group: 13,
                period: 2,
                properties: {
                    "Atomic Mass": "10.81 u",
                    "Phase": "Solid",
                    "Density": "2.34 g/cm³",
                    "Melting Point": "2076 °C",
                    "Boiling Point": "3927 °C",
                    "Electronegativity": "2.04",
                    "Discovered": "1808 by Humphry Davy and others"
                },
                uses: [
                    "Borosilicate glass (heat-resistant cookware)",
                    "Semiconductor dopant",
                    "Neutron absorber in nuclear reactors",
                    "Rocket propellant igniter",
                    "Fiberglass and insulation materials"
                ]
            },
            {
                number: 6,
                symbol: "C",
                name: "Carbon",
                mass: "12.011",
                category: "nonmetal",
                group: 14,
                period: 2,
                properties: {
                    "Atomic Mass": "12.011 u",
                    "Phase": "Solid",
                    "Density": "2.267 g/cm³ (graphite), 3.513 g/cm³ (diamond)",
                    "Melting Point": "3500 °C (sublimation)",
                    "Boiling Point": "4827 °C",
                    "Electronegativity": "2.55",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Basis of all organic life",
                    "Steel production (carbon content)",
                    "Graphite for pencils and lubricants",
                    "Diamonds for jewelry and cutting tools",
                    "Carbon fiber for lightweight materials"
                ]
            },
            {
                number: 7,
                symbol: "N",
                name: "Nitrogen",
                mass: "14.007",
                category: "nonmetal",
                group: 15,
                period: 2,
                properties: {
                    "Atomic Mass": "14.007 u",
                    "Phase": "Gas",
                    "Density": "0.0012506 g/cm³",
                    "Melting Point": "-210.00 °C",
                    "Boiling Point": "-195.79 °C",
                    "Electronegativity": "3.04",
                    "Discovered": "1772 by Daniel Rutherford"
                },
                uses: [
                    "Ammonia production for fertilizers",
                    "Inert atmosphere for food packaging",
                    "Liquid nitrogen for cryogenics",
                    "Explosives and propellants",
                    "Preservation of biological samples"
                ]
            },
            {
                number: 8,
                symbol: "O",
                name: "Oxygen",
                mass: "15.999",
                category: "nonmetal",
                group: 16,
                period: 2,
                properties: {
                    "Atomic Mass": "15.999 u",
                    "Phase": "Gas",
                    "Density": "0.001429 g/cm³",
                    "Melting Point": "-218.79 °C",
                    "Boiling Point": "-182.95 °C",
                    "Electronegativity": "3.44",
                    "Discovered": "1774 by Joseph Priestley and Carl Wilhelm Scheele"
                },
                uses: [
                    "Essential for respiration in most living organisms",
                    "Medical oxygen therapy",
                    "Steel production (basic oxygen process)",
                    "Water treatment and purification",
                    "Rocket propellant oxidizer"
                ]
            },
            {
                number: 9,
                symbol: "F",
                name: "Fluorine",
                mass: "18.998",
                category: "halogen",
                group: 17,
                period: 2,
                properties: {
                    "Atomic Mass": "18.998 u",
                    "Phase": "Gas",
                    "Density": "0.001696 g/cm³",
                    "Melting Point": "-219.67 °C",
                    "Boiling Point": "-188.11 °C",
                    "Electronegativity": "3.98",
                    "Discovered": "1886 by Henri Moissan"
                },
                uses: [
                    "Toothpaste additive (sodium fluoride)",
                    "Teflon (polytetrafluoroethylene) production",
                    "Uranium enrichment (UF₆)",
                    "Refrigerants (CFCs, HFCs)",
                    "Etching glass and semiconductors"
                ]
            },
            {
                number: 10,
                symbol: "Ne",
                name: "Neon",
                mass: "20.180",
                category: "noble-gas",
                group: 18,
                period: 2,
                properties: {
                    "Atomic Mass": "20.180 u",
                    "Phase": "Gas",
                    "Density": "0.0008999 g/cm³",
                    "Melting Point": "-248.59 °C",
                    "Boiling Point": "-246.05 °C",
                    "Electronegativity": "N/A",
                    "Discovered": "1898 by William Ramsay and Morris Travers"
                },
                uses: [
                    "Neon signs and advertising lights",
                    "High-voltage indicators and lightning arrestors",
                    "Cryogenic refrigerant",
                    "Gas lasers",
                    "Television tubes"
                ]
            },
            // Period 3
            {
                number: 11,
                symbol: "Na",
                name: "Sodium",
                mass: "22.990",
                category: "alkali-metal",
                group: 1,
                period: 3,
                properties: {
                    "Atomic Mass": "22.990 u",
                    "Phase": "Solid",
                    "Density": "0.968 g/cm³",
                    "Melting Point": "97.72 °C",
                    "Boiling Point": "883 °C",
                    "Electronegativity": "0.93",
                    "Discovered": "1807 by Humphry Davy"
                },
                uses: [
                    "Table salt (sodium chloride)",
                    "Street lighting (sodium vapor lamps)",
                    "Coolant in nuclear reactors",
                    "Manufacture of soaps and detergents",
                    "Metallurgical reducing agent"
                ]
            },
            {
                number: 12,
                symbol: "Mg",
                name: "Magnesium",
                mass: "24.305",
                category: "alkaline-earth",
                group: 2,
                period: 3,
                properties: {
                    "Atomic Mass": "24.305 u",
                    "Phase": "Solid",
                    "Density": "1.738 g/cm³",
                    "Melting Point": "650 °C",
                    "Boiling Point": "1090 °C",
                    "Electronegativity": "1.31",
                    "Discovered": "1755 by Joseph Black"
                },
                uses: [
                    "Lightweight alloys for aerospace and automotive",
                    "Flare and fireworks (bright white light)",
                    "Antacid and laxative medications",
                    "Plant nutrient (chlorophyll component)",
                    "Sacrificial anode for corrosion protection"
                ]
            },
            {
                number: 13,
                symbol: "Al",
                name: "Aluminum",
                mass: "26.982",
                category: "post-transition-metal",
                group: 13,
                period: 3,
                properties: {
                    "Atomic Mass": "26.982 u",
                    "Phase": "Solid",
                    "Density": "2.70 g/cm³",
                    "Melting Point": "660.32 °C",
                    "Boiling Point": "2519 °C",
                    "Electronegativity": "1.61",
                    "Discovered": "1825 by Hans Christian Ørsted"
                },
                uses: [
                    "Beverage cans and food packaging",
                    "Aircraft and vehicle construction",
                    "Electrical transmission lines",
                    "Construction materials (windows, doors)",
                    "Heat sinks for electronics"
                ]
            },
            {
                number: 14,
                symbol: "Si",
                name: "Silicon",
                mass: "28.085",
                category: "metalloid",
                group: 14,
                period: 3,
                properties: {
                    "Atomic Mass": "28.085 u",
                    "Phase": "Solid",
                    "Density": "2.329 g/cm³",
                    "Melting Point": "1414 °C",
                    "Boiling Point": "3265 °C",
                    "Electronegativity": "1.90",
                    "Discovered": "1824 by Jöns Jacob Berzelius"
                },
                uses: [
                    "Semiconductors and computer chips",
                    "Solar cells and photovoltaic panels",
                    "Glass and ceramics manufacturing",
                    "Silicones for sealants and lubricants",
                    "Construction materials (concrete, bricks)"
                ]
            },
            {
                number: 15,
                symbol: "P",
                name: "Phosphorus",
                mass: "30.974",
                category: "nonmetal",
                group: 15,
                period: 3,
                properties: {
                    "Atomic Mass": "30.974 u",
                    "Phase": "Solid",
                    "Density": "1.823 g/cm³ (white), 2.69 g/cm³ (red)",
                    "Melting Point": "44.15 °C (white)",
                    "Boiling Point": "280.5 °C (white)",
                    "Electronegativity": "2.19",
                    "Discovered": "1669 by Hennig Brand"
                },
                uses: [
                    "Fertilizers (phosphate)",
                    "Matches and incendiary devices",
                    "Steel production (deoxidizer)",
                    "Detergents and water softeners",
                    "Biological molecules (DNA, ATP)"
                ]
            },
            {
                number: 16,
                symbol: "S",
                name: "Sulfur",
                mass: "32.06",
                category: "nonmetal",
                group: 16,
                period: 3,
                properties: {
                    "Atomic Mass": "32.06 u",
                    "Phase": "Solid",
                    "Density": "2.07 g/cm³",
                    "Melting Point": "115.21 °C",
                    "Boiling Point": "444.61 °C",
                    "Electronegativity": "2.58",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Sulfuric acid production (industrial chemical)",
                    "Vulcanization of rubber",
                    "Fungicides and pesticides",
                    "Gunpowder and matches",
                    "Pharmaceuticals (antibiotics)"
                ]
            },
            {
                number: 17,
                symbol: "Cl",
                name: "Chlorine",
                mass: "35.45",
                category: "halogen",
                group: 17,
                period: 3,
                properties: {
                    "Atomic Mass": "35.45 u",
                    "Phase": "Gas",
                    "Density": "0.003214 g/cm³",
                    "Melting Point": "-101.5 °C",
                    "Boiling Point": "-34.04 °C",
                    "Electronegativity": "3.16",
                    "Discovered": "1774 by Carl Wilhelm Scheele"
                },
                uses: [
                    "Water purification and disinfection",
                    "PVC (polyvinyl chloride) production",
                    "Bleaching paper and textiles",
                    "Production of solvents and pesticides",
                    "Pharmaceuticals and disinfectants"
                ]
            },
            {
                number: 18,
                symbol: "Ar",
                name: "Argon",
                mass: "39.948",
                category: "noble-gas",
                group: 18,
                period: 3,
                properties: {
                    "Atomic Mass": "39.948 u",
                    "Phase": "Gas",
                    "Density": "0.0017837 g/cm³",
                    "Melting Point": "-189.34 °C",
                    "Boiling Point": "-185.85 °C",
                    "Electronegativity": "N/A",
                    "Discovered": "1894 by Lord Rayleigh and William Ramsay"
                },
                uses: [
                    "Inert shielding gas for welding",
                    "Incandescent and fluorescent lighting",
                    "Preservation of historical documents",
                    "Window insulation (double glazing)",
                    "Protective atmosphere for crystal growth"
                ]
            },
            // Period 4
            {
                number: 19,
                symbol: "K",
                name: "Potassium",
                mass: "39.098",
                category: "alkali-metal",
                group: 1,
                period: 4,
                properties: {
                    "Atomic Mass": "39.098 u",
                    "Phase": "Solid",
                    "Density": "0.862 g/cm³",
                    "Melting Point": "63.38 °C",
                    "Boiling Point": "759 °C",
                    "Electronegativity": "0.82",
                    "Discovered": "1807 by Humphry Davy"
                },
                uses: [
                    "Fertilizers (potash)",
                    "Soap and detergent production",
                    "Electrolyte in batteries",
                    "Regulating blood pressure in humans",
                    "Glass manufacturing"
                ]
            },
            {
                number: 20,
                symbol: "Ca",
                name: "Calcium",
                mass: "40.078",
                category: "alkaline-earth",
                group: 2,
                period: 4,
                properties: {
                    "Atomic Mass": "40.078 u",
                    "Phase": "Solid",
                    "Density": "1.54 g/cm³",
                    "Melting Point": "842 °C",
                    "Boiling Point": "1484 °C",
                    "Electronegativity": "1.00",
                    "Discovered": "1808 by Humphry Davy"
                },
                uses: [
                    "Bone and teeth formation in organisms",
                    "Cement and mortar production",
                    "Dietary supplement",
                    "Reducing agent in metal extraction",
                    "Deoxidizer in steel production"
                ]
            },
            // Transition metals (21-30)
            {
                number: 21,
                symbol: "Sc",
                name: "Scandium",
                mass: "44.956",
                category: "transition-metal",
                group: 3,
                period: 4,
                properties: {
                    "Atomic Mass": "44.956 u",
                    "Phase": "Solid",
                    "Density": "2.985 g/cm³",
                    "Melting Point": "1541 °C",
                    "Boiling Point": "2836 °C",
                    "Electronegativity": "1.36",
                    "Discovered": "1879 by Lars Fredrik Nilson"
                },
                uses: [
                    "Aluminum-scandium alloys for aerospace",
                    "High-intensity stadium lighting",
                    "Sports equipment (bikes, baseball bats)",
                    "Fuel cells and catalysis",
                    "Trace element in agricultural fertilizers"
                ]
            },
            {
                number: 22,
                symbol: "Ti",
                name: "Titanium",
                mass: "47.867",
                category: "transition-metal",
                group: 4,
                period: 4,
                properties: {
                    "Atomic Mass": "47.867 u",
                    "Phase": "Solid",
                    "Density": "4.506 g/cm³",
                    "Melting Point": "1668 °C",
                    "Boiling Point": "3287 °C",
                    "Electronegativity": "1.54",
                    "Discovered": "1791 by William Gregor"
                },
                uses: [
                    "Aircraft and spacecraft components",
                    "Medical implants and prosthetics",
                    "Paints and pigments (titanium white)",
                    "Jewelry and watches",
                    "Desalination plants and chemical processing"
                ]
            },
            {
                number: 23,
                symbol: "V",
                name: "Vanadium",
                mass: "50.942",
                category: "transition-metal",
                group: 5,
                period: 4,
                properties: {
                    "Atomic Mass": "50.942 u",
                    "Phase": "Solid",
                    "Density": "6.11 g/cm³",
                    "Melting Point": "1910 °C",
                    "Boiling Point": "3407 °C",
                    "Electronegativity": "1.63",
                    "Discovered": "1801 by Andrés Manuel del Río"
                },
                uses: [
                    "Steel alloy additive for strength",
                    "Vanadium redox batteries for energy storage",
                    "Catalyst for sulfuric acid production",
                    "Pigments and ceramics",
                    "Possible role in glucose metabolism"
                ]
            },
            {
                number: 24,
                symbol: "Cr",
                name: "Chromium",
                mass: "51.996",
                category: "transition-metal",
                group: 6,
                period: 4,
                properties: {
                    "Atomic Mass": "51.996 u",
                    "Phase": "Solid",
                    "Density": "7.15 g/cm³",
                    "Melting Point": "1907 °C",
                    "Boiling Point": "2671 °C",
                    "Electronegativity": "1.66",
                    "Discovered": "1797 by Louis Nicolas Vauquelin"
                },
                uses: [
                    "Stainless steel production",
                    "Chrome plating for decoration and corrosion resistance",
                    "Leather tanning",
                    "Pigments (chrome yellow, chrome green)",
                    "Wood preservative"
                ]
            },
            {
                number: 25,
                symbol: "Mn",
                name: "Manganese",
                mass: "54.938",
                category: "transition-metal",
                group: 7,
                period: 4,
                properties: {
                    "Atomic Mass": "54.938 u",
                    "Phase": "Solid",
                    "Density": "7.21 g/cm³",
                    "Melting Point": "1246 °C",
                    "Boiling Point": "2061 °C",
                    "Electronegativity": "1.55",
                    "Discovered": "1774 by Johan Gottlieb Gahn"
                },
                uses: [
                    "Steel production (deoxidizer and alloying agent)",
                    "Dry cell batteries (alkaline and zinc-carbon)",
                    "Fertilizers and animal feed additive",
                    "Glass decolorizing and coloring",
                    "Essential trace element in biology"
                ]
            },
            {
                number: 26,
                symbol: "Fe",
                name: "Iron",
                mass: "55.845",
                category: "transition-metal",
                group: 8,
                period: 4,
                properties: {
                    "Atomic Mass": "55.845 u",
                    "Phase": "Solid",
                    "Density": "7.874 g/cm³",
                    "Melting Point": "1538 °C",
                    "Boiling Point": "2862 °C",
                    "Electronegativity": "1.83",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Steel production (construction, vehicles)",
                    "Hemoglobin in blood (oxygen transport)",
                    "Magnets and electromagnets",
                    "Industrial catalysts",
                    "Dietary supplement"
                ]
            },
            {
                number: 27,
                symbol: "Co",
                name: "Cobalt",
                mass: "58.933",
                category: "transition-metal",
                group: 9,
                period: 4,
                properties: {
                    "Atomic Mass": "58.933 u",
                    "Phase": "Solid",
                    "Density": "8.86 g/cm³",
                    "Melting Point": "1495 °C",
                    "Boiling Point": "2927 °C",
                    "Electronegativity": "1.88",
                    "Discovered": "1735 by Georg Brandt"
                },
                uses: [
                    "Lithium-ion battery cathodes",
                    "Superalloys for jet engines",
                    "Pigments (cobalt blue glass and ceramics)",
                    "Vitamin B12 (cobalamin)",
                    "Radiation therapy (cobalt-60)"
                ]
            },
            {
                number: 28,
                symbol: "Ni",
                name: "Nickel",
                mass: "58.693",
                category: "transition-metal",
                group: 10,
                period: 4,
                properties: {
                    "Atomic Mass": "58.693 u",
                    "Phase": "Solid",
                    "Density": "8.908 g/cm³",
                    "Melting Point": "1455 °C",
                    "Boiling Point": "2913 °C",
                    "Electronegativity": "1.91",
                    "Discovered": "1751 by Axel Fredrik Cronstedt"
                },
                uses: [
                    "Stainless steel and corrosion-resistant alloys",
                    "Coinage (nickel coins)",
                    "Rechargeable batteries (NiCd, NiMH)",
                    "Electroplating for decorative finishes",
                    "Catalysts in hydrogenation reactions"
                ]
            },
            {
                number: 29,
                symbol: "Cu",
                name: "Copper",
                mass: "63.546",
                category: "transition-metal",
                group: 11,
                period: 4,
                properties: {
                    "Atomic Mass": "63.546 u",
                    "Phase": "Solid",
                    "Density": "8.96 g/cm³",
                    "Melting Point": "1084.62 °C",
                    "Boiling Point": "2562 °C",
                    "Electronegativity": "1.90",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Electrical wiring and electronics",
                    "Plumbing and roofing materials",
                    "Antimicrobial surfaces",
                    "Copper alloys (brass, bronze)",
                    "Nutritional supplement"
                ]
            },
            {
                number: 30,
                symbol: "Zn",
                name: "Zinc",
                mass: "65.38",
                category: "transition-metal",
                group: 12,
                period: 4,
                properties: {
                    "Atomic Mass": "65.38 u",
                    "Phase": "Solid",
                    "Density": "7.134 g/cm³",
                    "Melting Point": "419.53 °C",
                    "Boiling Point": "907 °C",
                    "Electronegativity": "1.65",
                    "Discovered": "1746 by Andreas Sigismund Marggraf"
                },
                uses: [
                    "Galvanizing steel for corrosion protection",
                    "Zinc-carbon and alkaline batteries",
                    "Die casting for automotive parts",
                    "Sunscreen and skin creams",
                    "Essential nutrient for immune function"
                ]
            },
            // Post-transition metals (31-32)
            {
                number: 31,
                symbol: "Ga",
                name: "Gallium",
                mass: "69.723",
                category: "post-transition-metal",
                group: 13,
                period: 4,
                properties: {
                    "Atomic Mass": "69.723 u",
                    "Phase": "Solid",
                    "Density": "5.907 g/cm³",
                    "Melting Point": "29.76 °C",
                    "Boiling Point": "2204 °C",
                    "Electronegativity": "1.81",
                    "Discovered": "1875 by Paul-Émile Lecoq de Boisbaudran"
                },
                uses: [
                    "Semiconductors (gallium arsenide for LEDs)",
                    "High-temperature thermometers",
                    "Liquid mirrors for telescopes",
                    "Medical imaging (gallium-67 scans)",
                    "Neutrino detection"
                ]
            },
            {
                number: 32,
                symbol: "Ge",
                name: "Germanium",
                mass: "72.630",
                category: "metalloid",
                group: 14,
                period: 4,
                properties: {
                    "Atomic Mass": "72.630 u",
                    "Phase": "Solid",
                    "Density": "5.323 g/cm³",
                    "Melting Point": "938.25 °C",
                    "Boiling Point": "2833 °C",
                    "Electronegativity": "2.01",
                    "Discovered": "1886 by Clemens Winkler"
                },
                uses: [
                    "Fiber optics and infrared optics",
                    "Semiconductors and transistors",
                    "Solar cell applications",
                    "Polymerization catalysts",
                    "Night vision devices"
                ]
            },
            // Nonmetals (33-34)
            {
                number: 33,
                symbol: "As",
                name: "Arsenic",
                mass: "74.922",
                category: "metalloid",
                group: 15,
                period: 4,
                properties: {
                    "Atomic Mass": "74.922 u",
                    "Phase": "Solid",
                    "Density": "5.727 g/cm³",
                    "Melting Point": "816.80 °C (at 28 atm)",
                    "Boiling Point": "614 °C (sublimation)",
                    "Electronegativity": "2.18",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Semiconductor dopant",
                    "Wood preservative (chromated copper arsenate)",
                    "Insecticides and herbicides",
                    "Gallium arsenide for electronics",
                    "Historical medicine (now recognized as toxic)"
                ]
            },
            {
                number: 34,
                symbol: "Se",
                name: "Selenium",
                mass: "78.971",
                category: "nonmetal",
                group: 16,
                period: 4,
                properties: {
                    "Atomic Mass": "78.971 u",
                    "Phase": "Solid",
                    "Density": "4.809 g/cm³",
                    "Melting Point": "221 °C",
                    "Boiling Point": "685 °C",
                    "Electronegativity": "2.55",
                    "Discovered": "1817 by Jöns Jacob Berzelius"
                },
                uses: [
                    "Photocopiers and laser printers",
                    "Glass manufacturing (red and clear glass)",
                    "Nutritional supplement (antioxidant)",
                    "Solar cells",
                    "Photoelectric cells"
                ]
            },
            // Halogen (35)
            {
                number: 35,
                symbol: "Br",
                name: "Bromine",
                mass: "79.904",
                category: "halogen",
                group: 17,
                period: 4,
                properties: {
                    "Atomic Mass": "79.904 u",
                    "Phase": "Liquid",
                    "Density": "3.1028 g/cm³",
                    "Melting Point": "-7.2 °C",
                    "Boiling Point": "58.8 °C",
                    "Electronegativity": "2.96",
                    "Discovered": "1826 by Antoine Jérôme Balard"
                },
                uses: [
                    "Flame retardants",
                    "Photographic film (silver bromide)",
                    "Water purification",
                    "Pharmaceuticals (sedatives)",
                    "Drilling fluids"
                ]
            },
            // Noble gas (36)
            {
                number: 36,
                symbol: "Kr",
                name: "Krypton",
                mass: "83.798",
                category: "noble-gas",
                group: 18,
                period: 4,
                properties: {
                    "Atomic Mass": "83.798 u",
                    "Phase": "Gas",
                    "Density": "0.003733 g/cm³",
                    "Melting Point": "-157.36 °C",
                    "Boiling Point": "-153.22 °C",
                    "Electronegativity": "3.00",
                    "Discovered": "1898 by William Ramsay and Morris Travers"
                },
                uses: [
                    "Energy-efficient fluorescent lights",
                    "Photography flashes",
                    "Lasers (krypton ion laser)",
                    "Insulating windows",
                    "Measurement standard (krypton-86 wavelength)"
                ]
            },
            // Period 5 (elements 37-54)
            {
                number: 37,
                symbol: "Rb",
                name: "Rubidium",
                mass: "85.468",
                category: "alkali-metal",
                group: 1,
                period: 5,
                properties: {
                    "Atomic Mass": "85.468 u",
                    "Phase": "Solid",
                    "Density": "1.532 g/cm³",
                    "Melting Point": "39.31 °C",
                    "Boiling Point": "688 °C",
                    "Electronegativity": "0.82",
                    "Discovered": "1861 by Robert Bunsen and Gustav Kirchhoff"
                },
                uses: [
                    "Atomic clocks (rubidium frequency standard)",
                    "Vacuum tubes as getter",
                    "Fireworks (purple color)",
                    "Medical imaging (rubidium-82 PET scans)",
                    "Specialty glasses"
                ]
            },
            {
                number: 38,
                symbol: "Sr",
                name: "Strontium",
                mass: "87.62",
                category: "alkaline-earth",
                group: 2,
                period: 5,
                properties: {
                    "Atomic Mass": "87.62 u",
                    "Phase": "Solid",
                    "Density": "2.64 g/cm³",
                    "Melting Point": "777 °C",
                    "Boiling Point": "1382 °C",
                    "Electronegativity": "0.95",
                    "Discovered": "1790 by Adair Crawford"
                },
                uses: [
                    "Fireworks and flares (red color)",
                    "Ferrite magnets",
                    "Radioactive strontium-90 in nuclear batteries",
                    "Toothpaste for sensitive teeth",
                    "Glass for color television tubes"
                ]
            },
            // Transition metals (39-48)
            {
                number: 39,
                symbol: "Y",
                name: "Yttrium",
                mass: "88.906",
                category: "transition-metal",
                group: 3,
                period: 5,
                properties: {
                    "Atomic Mass": "88.906 u",
                    "Phase": "Solid",
                    "Density": "4.472 g/cm³",
                    "Melting Point": "1526 °C",
                    "Boiling Point": "3336 °C",
                    "Electronegativity": "1.22",
                    "Discovered": "1794 by Johan Gadolin"
                },
                uses: [
                    "Yttrium aluminum garnet (YAG) lasers",
                    "Red phosphors in color TV tubes",
                    "Superconductors (yttrium barium copper oxide)",
                    "Strengthening agent in alloys",
                    "Cancer treatment (yttrium-90)"
                ]
            },
            {
                number: 40,
                symbol: "Zr",
                name: "Zirconium",
                mass: "91.224",
                category: "transition-metal",
                group: 4,
                period: 5,
                properties: {
                    "Atomic Mass": "91.224 u",
                    "Phase": "Solid",
                    "Density": "6.506 g/cm³",
                    "Melting Point": "1855 °C",
                    "Boiling Point": "4409 °C",
                    "Electronegativity": "1.33",
                    "Discovered": "1789 by Martin Heinrich Klaproth"
                },
                uses: [
                    "Nuclear reactor cladding (zircaloy)",
                    "Abrasive and refractory applications",
                    "Surgical instruments",
                    "Gemstones (cubic zirconia)",
                    "Anti-corrosion alloys"
                ]
            },
            {
                number: 41,
                symbol: "Nb",
                name: "Niobium",
                mass: "92.906",
                category: "transition-metal",
                group: 5,
                period: 5,
                properties: {
                    "Atomic Mass": "92.906 u",
                    "Phase": "Solid",
                    "Density": "8.57 g/cm³",
                    "Melting Point": "2477 °C",
                    "Boiling Point": "4744 °C",
                    "Electronegativity": "1.60",
                    "Discovered": "1801 by Charles Hatchett"
                },
                uses: [
                    "Superconducting magnets (niobium-titanium)",
                    "Jet engines and rocket subassemblies",
                    "Jewelry (hypoallergenic properties)",
                    "Superalloys for pipelines",
                    "Nuclear industry"
                ]
            },
            {
                number: 42,
                symbol: "Mo",
                name: "Molybdenum",
                mass: "95.95",
                category: "transition-metal",
                group: 6,
                period: 5,
                properties: {
                    "Atomic Mass": "95.95 u",
                    "Phase": "Solid",
                    "Density": "10.22 g/cm³",
                    "Melting Point": "2623 °C",
                    "Boiling Point": "4639 °C",
                    "Electronegativity": "2.16",
                    "Discovered": "1778 by Carl Wilhelm Scheele"
                },
                uses: [
                    "Alloying agent in steel (increases strength)",
                    "Lubricant (molybdenum disulfide)",
                    "Electrodes in glass furnaces",
                    "Missile and aircraft parts",
                    "Essential trace element in biology"
                ]
            },
            {
                number: 43,
                symbol: "Tc",
                name: "Technetium",
                mass: "[98]",
                category: "transition-metal",
                group: 7,
                period: 5,
                properties: {
                    "Atomic Mass": "[98] u",
                    "Phase": "Solid",
                    "Density": "11 g/cm³",
                    "Melting Point": "2157 °C",
                    "Boiling Point": "4265 °C",
                    "Electronegativity": "1.90",
                    "Discovered": "1937 by Carlo Perrier and Emilio Segrè"
                },
                uses: [
                    "Medical imaging (technetium-99m)",
                    "Corrosion inhibitor for steel",
                    "Gamma ray source",
                    "Superconductor research",
                    "Industrial radiography"
                ]
            },
            {
                number: 44,
                symbol: "Ru",
                name: "Ruthenium",
                mass: "101.07",
                category: "transition-metal",
                group: 8,
                period: 5,
                properties: {
                    "Atomic Mass": "101.07 u",
                    "Phase": "Solid",
                    "Density": "12.45 g/cm³",
                    "Melting Point": "2334 °C",
                    "Boiling Point": "4150 °C",
                    "Electronegativity": "2.20",
                    "Discovered": "1844 by Karl Ernst Claus"
                },
                uses: [
                    "Hardening agent for platinum and palladium",
                    "Electrical contacts (wear resistance)",
                    "Solar cell technology",
                    "Jewelry plating",
                    "Catalysts for ammonia production"
                ]
            },
            {
                number: 45,
                symbol: "Rh",
                name: "Rhodium",
                mass: "102.91",
                category: "transition-metal",
                group: 9,
                period: 5,
                properties: {
                    "Atomic Mass": "102.91 u",
                    "Phase": "Solid",
                    "Density": "12.41 g/cm³",
                    "Melting Point": "1964 °C",
                    "Boiling Point": "3695 °C",
                    "Electronegativity": "2.28",
                    "Discovered": "1803 by William Hyde Wollaston"
                },
                uses: [
                    "Catalytic converters in automobiles",
                    "Jewelry plating (rhodium finish)",
                    "Electrical contacts",
                    "Fountain pen nibs",
                    "Neutron detectors"
                ]
            },
            {
                number: 46,
                symbol: "Pd",
                name: "Palladium",
                mass: "106.42",
                category: "transition-metal",
                group: 10,
                period: 5,
                properties: {
                    "Atomic Mass": "106.42 u",
                    "Phase": "Solid",
                    "Density": "12.02 g/cm³",
                    "Melting Point": "1554.9 °C",
                    "Boiling Point": "2963 °C",
                    "Electronegativity": "2.20",
                    "Discovered": "1803 by William Hyde Wollaston"
                },
                uses: [
                    "Catalytic converters",
                    "Jewelry (white gold alternative)",
                    "Dental alloys",
                    "Hydrogen purification",
                    "Electronics (multilayer ceramic capacitors)"
                ]
            },
            {
                number: 47,
                symbol: "Ag",
                name: "Silver",
                mass: "107.87",
                category: "transition-metal",
                group: 11,
                period: 5,
                properties: {
                    "Atomic Mass": "107.87 u",
                    "Phase": "Solid",
                    "Density": "10.49 g/cm³",
                    "Melting Point": "961.78 °C",
                    "Boiling Point": "2162 °C",
                    "Electronegativity": "1.93",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Jewelry and silverware",
                    "Photography (silver halides)",
                    "Electrical contacts and conductors",
                    "Antibacterial applications",
                    "Solar panels"
                ]
            },
            {
                number: 48,
                symbol: "Cd",
                name: "Cadmium",
                mass: "112.41",
                category: "transition-metal",
                group: 12,
                period: 5,
                properties: {
                    "Atomic Mass": "112.41 u",
                    "Phase": "Solid",
                    "Density": "8.65 g/cm³",
                    "Melting Point": "321.07 °C",
                    "Boiling Point": "767 °C",
                    "Electronegativity": "1.69",
                    "Discovered": "1817 by Friedrich Stromeyer"
                },
                uses: [
                    "Nickel-cadmium batteries",
                    "Pigments (cadmium yellow)",
                    "Electroplating for corrosion resistance",
                    "Nuclear reactor control rods",
                    "TV phosphors"
                ]
            },
            // Post-transition metals (49-50)
            {
                number: 49,
                symbol: "In",
                name: "Indium",
                mass: "114.82",
                category: "post-transition-metal",
                group: 13,
                period: 5,
                properties: {
                    "Atomic Mass": "114.82 u",
                    "Phase": "Solid",
                    "Density": "7.31 g/cm³",
                    "Melting Point": "156.60 °C",
                    "Boiling Point": "2072 °C",
                    "Electronegativity": "1.78",
                    "Discovered": "1863 by Ferdinand Reich and Hieronymous Richter"
                },
                uses: [
                    "Indium tin oxide (ITO) for touch screens",
                    "Low-melting-point alloys",
                    "Semiconductor doping",
                    "Solders and thermal interface materials",
                    "Nuclear reactor control rods"
                ]
            },
            {
                number: 50,
                symbol: "Sn",
                name: "Tin",
                mass: "118.71",
                category: "post-transition-metal",
                group: 14,
                period: 5,
                properties: {
                    "Atomic Mass": "118.71 u",
                    "Phase": "Solid",
                    "Density": "7.287 g/cm³ (white), 5.769 g/cm³ (gray)",
                    "Melting Point": "231.93 °C",
                    "Boiling Point": "2602 °C",
                    "Electronegativity": "1.96",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Tin plating for food cans",
                    "Solder alloys",
                    "Bronze (copper-tin alloy)",
                    "Organotin compounds as stabilizers",
                    "Glass manufacturing (float glass process)"
                ]
            },
            // Metalloid (51)
            {
                number: 51,
                symbol: "Sb",
                name: "Antimony",
                mass: "121.76",
                category: "metalloid",
                group: 15,
                period: 5,
                properties: {
                    "Atomic Mass": "121.76 u",
                    "Phase": "Solid",
                    "Density": "6.697 g/cm³",
                    "Melting Point": "630.63 °C",
                    "Boiling Point": "1587 °C",
                    "Electronegativity": "2.05",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Flame retardants",
                    "Lead-acid batteries",
                    "Semiconductor industry",
                    "Alloys (pewter, type metal)",
                    "Medicinal applications (antimony compounds)"
                ]
            },
            // Nonmetal (52)
            {
                number: 52,
                symbol: "Te",
                name: "Tellurium",
                mass: "127.60",
                category: "metalloid",
                group: 16,
                period: 5,
                properties: {
                    "Atomic Mass": "127.60 u",
                    "Phase": "Solid",
                    "Density": "6.24 g/cm³",
                    "Melting Point": "449.51 °C",
                    "Boiling Point": "988 °C",
                    "Electronegativity": "2.10",
                    "Discovered": "1782 by Franz-Joseph Müller von Reichenstein"
                },
                uses: [
                    "Alloying agent in steel and copper",
                    "Solar panels (cadmium telluride)",
                    "Thermoelectric devices",
                    "Rubber vulcanization",
                    "Optical data storage"
                ]
            },
            // Halogen (53)
            {
                number: 53,
                symbol: "I",
                name: "Iodine",
                mass: "126.90",
                category: "halogen",
                group: 17,
                period: 5,
                properties: {
                    "Atomic Mass": "126.90 u",
                    "Phase": "Solid",
                    "Density": "4.933 g/cm³",
                    "Melting Point": "113.7 °C",
                    "Boiling Point": "184.3 °C",
                    "Electronegativity": "2.66",
                    "Discovered": "1811 by Bernard Courtois"
                },
                uses: [
                    "Disinfectants and antiseptics",
                    "Thyroid hormone production",
                    "X-ray contrast media",
                    "Photographic chemicals",
                    "Nutritional supplement"
                ]
            },
            // Noble gas (54)
            {
                number: 54,
                symbol: "Xe",
                name: "Xenon",
                mass: "131.29",
                category: "noble-gas",
                group: 18,
                period: 5,
                properties: {
                    "Atomic Mass": "131.29 u",
                    "Phase": "Gas",
                    "Density": "0.005887 g/cm³",
                    "Melting Point": "-111.75 °C",
                    "Boiling Point": "-108.12 °C",
                    "Electronegativity": "2.60",
                    "Discovered": "1898 by William Ramsay and Morris Travers"
                },
                uses: [
                    "High-intensity lamps (xenon arc lamps)",
                    "Anesthetic in medicine",
                    "Ion propulsion for spacecraft",
                    "Nuclear energy (detecting neutrinos)",
                    "Medical imaging (xenon CT scans)"
                ]
            },
            // Period 6 (elements 55-86)
            {
                number: 55,
                symbol: "Cs",
                name: "Cesium",
                mass: "132.91",
                category: "alkali-metal",
                group: 1,
                period: 6,
                properties: {
                    "Atomic Mass": "132.91 u",
                    "Phase": "Solid",
                    "Density": "1.873 g/cm³",
                    "Melting Point": "28.44 °C",
                    "Boiling Point": "671 °C",
                    "Electronegativity": "0.79",
                    "Discovered": "1860 by Robert Bunsen and Gustav Kirchhoff"
                },
                uses: [
                    "Atomic clocks (cesium standard)",
                    "Drilling fluids in oil industry",
                    "Photoelectric cells",
                    "Catalyst in hydrogenation",
                    "Vacuum tubes"
                ]
            },
            {
                number: 56,
                symbol: "Ba",
                name: "Barium",
                mass: "137.33",
                category: "alkaline-earth",
                group: 2,
                period: 6,
                properties: {
                    "Atomic Mass": "137.33 u",
                    "Phase": "Solid",
                    "Density": "3.594 g/cm³",
                    "Melting Point": "727 °C",
                    "Boiling Point": "1870 °C",
                    "Electronegativity": "0.89",
                    "Discovered": "1808 by Humphry Davy"
                },
                uses: [
                    "X-ray imaging (barium swallow)",
                    "Fireworks (green color)",
                    "Drilling fluids",
                    "Glass and ceramics",
                    "Rat poison"
                ]
            },
            // Lanthanides (57-71)
            {
                number: 57,
                symbol: "La",
                name: "Lanthanum",
                mass: "138.91",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "138.91 u",
                    "Phase": "Solid",
                    "Density": "6.162 g/cm³",
                    "Melting Point": "920 °C",
                    "Boiling Point": "3464 °C",
                    "Electronegativity": "1.10",
                    "Discovered": "1839 by Carl Gustaf Mosander"
                },
                uses: [
                    "Camera and telescope lenses (lanthanum oxide glass)",
                    "Hydrogen storage in batteries",
                    "Petroleum refining catalysts",
                    "Carbon arc lamps for studio lighting",
                    "Alloying agent in mischmetal"
                ]
            },
            {
                number: 58,
                symbol: "Ce",
                name: "Cerium",
                mass: "140.12",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "140.12 u",
                    "Phase": "Solid",
                    "Density": "6.770 g/cm³",
                    "Melting Point": "795 °C",
                    "Boiling Point": "3443 °C",
                    "Electronegativity": "1.12",
                    "Discovered": "1803 by Jöns Jacob Berzelius and Wilhelm Hisinger"
                },
                uses: [
                    "Catalytic converters in automobiles",
                    "Self-cleaning ovens (cerium oxide)",
                    "Glass polishing compounds",
                    "Flints for lighters",
                    "Yellow pigment in ceramics"
                ]
            },
            {
                number: 59,
                symbol: "Pr",
                name: "Praseodymium",
                mass: "140.91",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "140.91 u",
                    "Phase": "Solid",
                    "Density": "6.773 g/cm³",
                    "Melting Point": "935 °C",
                    "Boiling Point": "3520 °C",
                    "Electronegativity": "1.13",
                    "Discovered": "1885 by Carl Auer von Welsbach"
                },
                uses: [
                    "Coloring for glasses and enamels (yellow-green)",
                    "Didymium glass for welder's goggles",
                    "Alloys for aircraft engines",
                    "Permanent magnets",
                    "Carbon arc lighting"
                ]
            },
            {
                number: 60,
                symbol: "Nd",
                name: "Neodymium",
                mass: "144.24",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "144.24 u",
                    "Phase": "Solid",
                    "Density": "7.007 g/cm³",
                    "Melting Point": "1024 °C",
                    "Boiling Point": "3074 °C",
                    "Electronegativity": "1.14",
                    "Discovered": "1885 by Carl Auer von Welsbach"
                },
                uses: [
                    "Neodymium magnets (strongest permanent magnets)",
                    "Laser crystals (Nd:YAG lasers)",
                    "Coloring for glass (purple)",
                    "Electric motors in hybrid cars",
                    "Microphones and headphones"
                ]
            },
            {
                number: 61,
                symbol: "Pm",
                name: "Promethium",
                mass: "[145]",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "[145] u",
                    "Phase": "Solid",
                    "Density": "7.26 g/cm³",
                    "Melting Point": "1042 °C",
                    "Boiling Point": "3000 °C",
                    "Electronegativity": "1.13",
                    "Discovered": "1945 by Jacob A. Marinsky, Lawrence E. Glendenin, and Charles D. Coryell"
                },
                uses: [
                    "Luminous paint (promethium-147)",
                    "Nuclear batteries",
                    "Thickness gauges",
                    "Portable X-ray sources",
                    "Research applications"
                ]
            },
            {
                number: 62,
                symbol: "Sm",
                name: "Samarium",
                mass: "150.36",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "150.36 u",
                    "Phase": "Solid",
                    "Density": "7.52 g/cm³",
                    "Melting Point": "1072 °C",
                    "Boiling Point": "1900 °C",
                    "Electronegativity": "1.17",
                    "Discovered": "1879 by Paul-Émile Lecoq de Boisbaudran"
                },
                uses: [
                    "Samarium-cobalt magnets",
                    "Neutron absorber in nuclear reactors",
                    "Cancer treatment (samarium-153)",
                    "Carbon arc lighting",
                    "Infrared absorbing glass"
                ]
            },
            {
                number: 63,
                symbol: "Eu",
                name: "Europium",
                mass: "151.96",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "151.96 u",
                    "Phase": "Solid",
                    "Density": "5.244 g/cm³",
                    "Melting Point": "822 °C",
                    "Boiling Point": "1597 °C",
                    "Electronegativity": "1.20",
                    "Discovered": "1901 by Eugène-Anatole Demarçay"
                },
                uses: [
                    "Red and blue phosphors in TV screens",
                    "Euro banknotes (anti-counterfeiting)",
                    "Fluorescent lamps",
                    "Nuclear reactor control rods",
                    "Quantum memory storage research"
                ]
            },
            {
                number: 64,
                symbol: "Gd",
                name: "Gadolinium",
                mass: "157.25",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "157.25 u",
                    "Phase": "Solid",
                    "Density": "7.90 g/cm³",
                    "Melting Point": "1312 °C",
                    "Boiling Point": "3273 °C",
                    "Electronegativity": "1.20",
                    "Discovered": "1880 by Jean Charles Galissard de Marignac"
                },
                uses: [
                    "MRI contrast agents",
                    "Neutron absorber in nuclear reactors",
                    "Magnetocaloric refrigeration",
                    "Data storage technology",
                    "X-ray and gamma ray detection"
                ]
            },
            {
                number: 65,
                symbol: "Tb",
                name: "Terbium",
                mass: "158.93",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "158.93 u",
                    "Phase": "Solid",
                    "Density": "8.23 g/cm³",
                    "Melting Point": "1356 °C",
                    "Boiling Point": "3230 °C",
                    "Electronegativity": "1.20",
                    "Discovered": "1843 by Carl Gustaf Mosander"
                },
                uses: [
                    "Green phosphors in fluorescent lamps",
                    "Terfenol-D (magnetostrictive alloy)",
                    "Solid-state devices",
                    "Fuel cells",
                    "Sonar systems"
                ]
            },
            {
                number: 66,
                symbol: "Dy",
                name: "Dysprosium",
                mass: "162.50",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "162.50 u",
                    "Phase": "Solid",
                    "Density": "8.540 g/cm³",
                    "Melting Point": "1407 °C",
                    "Boiling Point": "2567 °C",
                    "Electronegativity": "1.22",
                    "Discovered": "1886 by Paul-Émile Lecoq de Boisbaudran"
                },
                uses: [
                    "Neodymium-based magnets (additive)",
                    "Nuclear reactor control rods",
                    "Data storage applications",
                    "Dysprosium-cadmium chalcogenides",
                    "High-intensity lighting"
                ]
            },
            {
                number: 67,
                symbol: "Ho",
                name: "Holmium",
                mass: "164.93",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "164.93 u",
                    "Phase": "Solid",
                    "Density": "8.79 g/cm³",
                    "Melting Point": "1461 °C",
                    "Boiling Point": "2720 °C",
                    "Electronegativity": "1.23",
                    "Discovered": "1878 by Marc Delafontaine and Jacques-Louis Soret"
                },
                uses: [
                    "Colorants for cubic zirconia and glass",
                    "Nuclear control rods",
                    "Solid-state lasers",
                    "Microwave equipment",
                    "Calibration of gamma ray spectrometers"
                ]
            },
            {
                number: 68,
                symbol: "Er",
                name: "Erbium",
                mass: "167.26",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "167.26 u",
                    "Phase": "Solid",
                    "Density": "9.066 g/cm³",
                    "Melting Point": "1529 °C",
                    "Boiling Point": "2868 °C",
                    "Electronegativity": "1.24",
                    "Discovered": "1843 by Carl Gustaf Mosander"
                },
                uses: [
                    "Fiber optic amplifiers (erbium-doped fiber)",
                    "Pink coloring for glass and porcelain",
                    "Nuclear technology (neutron absorber)",
                    "Laser applications in medicine",
                    "Metallurgical uses"
                ]
            },
            {
                number: 69,
                symbol: "Tm",
                name: "Thulium",
                mass: "168.93",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "168.93 u",
                    "Phase": "Solid",
                    "Density": "9.32 g/cm³",
                    "Melting Point": "1545 °C",
                    "Boiling Point": "1950 °C",
                    "Electronegativity": "1.25",
                    "Discovered": "1879 by Per Teodor Cleve"
                },
                uses: [
                    "Portable X-ray devices",
                    "Doped fiber amplifiers",
                    "Lasers for surgery",
                    "Euro banknotes (anti-counterfeiting)",
                    "High-temperature superconductors"
                ]
            },
            {
                number: 70,
                symbol: "Yb",
                name: "Ytterbium",
                mass: "173.05",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "173.05 u",
                    "Phase": "Solid",
                    "Density": "6.90 g/cm³",
                    "Melting Point": "824 °C",
                    "Boiling Point": "1196 °C",
                    "Electronegativity": "1.10",
                    "Discovered": "1878 by Jean Charles Galissard de Marignac"
                },
                uses: [
                    "Stainless steel improvement",
                    "Infrared lasers",
                    "Atomic clocks (most precise)",
                    "Gamma ray source",
                    "Stress gauges"
                ]
            },
            {
                number: 71,
                symbol: "Lu",
                name: "Lutetium",
                mass: "174.97",
                category: "lanthanide",
                group: 3,
                period: 6,
                properties: {
                    "Atomic Mass": "174.97 u",
                    "Phase": "Solid",
                    "Density": "9.841 g/cm³",
                    "Melting Point": "1652 °C",
                    "Boiling Point": "3402 °C",
                    "Electronegativity": "1.27",
                    "Discovered": "1907 by Georges Urbain and Carl Auer von Welsbach"
                },
                uses: [
                    "PET scan detectors (lutetium oxyorthosilicate)",
                    "Catalyst in petroleum refining",
                    "Cancer treatment (lutetium-177)",
                    "High-refractive-index glass",
                    "LED phosphors"
                ]
            },
            // Transition metals (72-80)
            {
                number: 72,
                symbol: "Hf",
                name: "Hafnium",
                mass: "178.49",
                category: "transition-metal",
                group: 4,
                period: 6,
                properties: {
                    "Atomic Mass": "178.49 u",
                    "Phase": "Solid",
                    "Density": "13.31 g/cm³",
                    "Melting Point": "2233 °C",
                    "Boiling Point": "4603 °C",
                    "Electronegativity": "1.30",
                    "Discovered": "1923 by Dirk Coster and George de Hevesy"
                },
                uses: [
                    "Nuclear reactor control rods",
                    "Microprocessors (hafnium-based gate dielectrics)",
                    "Plasma cutting tips",
                    "Alloys with iron, titanium, and niobium",
                    "Gas scavenger in vacuum tubes"
                ]
            },
            {
                number: 73,
                symbol: "Ta",
                name: "Tantalum",
                mass: "180.95",
                category: "transition-metal",
                group: 5,
                period: 6,
                properties: {
                    "Atomic Mass": "180.95 u",
                    "Phase": "Solid",
                    "Density": "16.69 g/cm³",
                    "Melting Point": "3017 °C",
                    "Boiling Point": "5458 °C",
                    "Electronegativity": "1.50",
                    "Discovered": "1802 by Anders Gustaf Ekeberg"
                },
                uses: [
                    "Electrolytic capacitors in electronics",
                    "Surgical implants and instruments",
                    "Chemical process equipment",
                    "Superalloys for jet engines",
                    "Mobile phones and computers"
                ]
            },
            {
                number: 74,
                symbol: "W",
                name: "Tungsten",
                mass: "183.84",
                category: "transition-metal",
                group: 6,
                period: 6,
                properties: {
                    "Atomic Mass": "183.84 u",
                    "Phase": "Solid",
                    "Density": "19.25 g/cm³",
                    "Melting Point": "3422 °C",
                    "Boiling Point": "5930 °C",
                    "Electronegativity": "2.36",
                    "Discovered": "1781 by Carl Wilhelm Scheele"
                },
                uses: [
                    "Incandescent light bulb filaments",
                    "Cutting tools (tungsten carbide)",
                    "Armor-piercing ammunition",
                    "Radiation shielding",
                    "Electrodes in TIG welding"
                ]
            },
            {
                number: 75,
                symbol: "Re",
                name: "Rhenium",
                mass: "186.21",
                category: "transition-metal",
                group: 7,
                period: 6,
                properties: {
                    "Atomic Mass": "186.21 u",
                    "Phase": "Solid",
                    "Density": "21.02 g/cm³",
                    "Melting Point": "3186 °C",
                    "Boiling Point": "5596 °C",
                    "Electronegativity": "1.90",
                    "Discovered": "1925 by Walter Noddack, Ida Tacke, and Otto Berg"
                },
                uses: [
                    "Superalloys for jet engine parts",
                    "Catalysts for petroleum refining",
                    "Electrical contacts",
                    "Filaments for mass spectrographs",
                    "Thermocouples"
                ]
            },
            {
                number: 76,
                symbol: "Os",
                name: "Osmium",
                mass: "190.23",
                category: "transition-metal",
                group: 8,
                period: 6,
                properties: {
                    "Atomic Mass": "190.23 u",
                    "Phase": "Solid",
                    "Density": "22.59 g/cm³",
                    "Melting Point": "3033 °C",
                    "Boiling Point": "5012 °C",
                    "Electronegativity": "2.20",
                    "Discovered": "1803 by Smithson Tennant"
                },
                uses: [
                    "Alloys for fountain pen tips and electrical contacts",
                    "Fingerprint detection",
                    "Osmium tetroxide staining in microscopy",
                    "Phonograph needles",
                    "Instrument pivots"
                ]
            },
            {
                number: 77,
                symbol: "Ir",
                name: "Iridium",
                mass: "192.22",
                category: "transition-metal",
                group: 9,
                period: 6,
                properties: {
                    "Atomic Mass": "192.22 u",
                    "Phase": "Solid",
                    "Density": "22.56 g/cm³",
                    "Melting Point": "2466 °C",
                    "Boiling Point": "4428 °C",
                    "Electronegativity": "2.20",
                    "Discovered": "1803 by Smithson Tennant"
                },
                uses: [
                    "Spark plugs for aircraft engines",
                    "Standard meter bar (historical)",
                    "Crucibles for crystal growth",
                    "Radioisotope thermoelectric generators",
                    "Deep-water pipes"
                ]
            },
            {
                number: 78,
                symbol: "Pt",
                name: "Platinum",
                mass: "195.08",
                category: "transition-metal",
                group: 10,
                period: 6,
                properties: {
                    "Atomic Mass": "195.08 u",
                    "Phase": "Solid",
                    "Density": "21.45 g/cm³",
                    "Melting Point": "1768.3 °C",
                    "Boiling Point": "3825 °C",
                    "Electronegativity": "2.28",
                    "Discovered": "1735 by Antonio de Ulloa"
                },
                uses: [
                    "Catalytic converters in automobiles",
                    "Jewelry",
                    "Laboratory equipment",
                    "Chemotherapy drugs (cisplatin)",
                    "Electrical contacts"
                ]
            },
            {
                number: 79,
                symbol: "Au",
                name: "Gold",
                mass: "196.97",
                category: "transition-metal",
                group: 11,
                period: 6,
                properties: {
                    "Atomic Mass": "196.97 u",
                    "Phase": "Solid",
                    "Density": "19.32 g/cm³",
                    "Melting Point": "1064.18 °C",
                    "Boiling Point": "2856 °C",
                    "Electronegativity": "2.54",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Jewelry and decoration",
                    "Financial standard (gold reserves)",
                    "Electronics (connectors and contacts)",
                    "Dental alloys",
                    "Spacecraft radiation shielding"
                ]
            },
            {
                number: 80,
                symbol: "Hg",
                name: "Mercury",
                mass: "200.59",
                category: "transition-metal",
                group: 12,
                period: 6,
                properties: {
                    "Atomic Mass": "200.59 u",
                    "Phase": "Liquid",
                    "Density": "13.534 g/cm³",
                    "Melting Point": "-38.83 °C",
                    "Boiling Point": "356.73 °C",
                    "Electronegativity": "2.00",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Thermometers and barometers",
                    "Fluorescent lamps",
                    "Dental amalgams",
                    "Industrial catalysts",
                    "Gold and silver extraction"
                ]
            },
            // Post-transition metals (81-83)
            {
                number: 81,
                symbol: "Tl",
                name: "Thallium",
                mass: "204.38",
                category: "post-transition-metal",
                group: 13,
                period: 6,
                properties: {
                    "Atomic Mass": "204.38 u",
                    "Phase": "Solid",
                    "Density": "11.85 g/cm³",
                    "Melting Point": "304 °C",
                    "Boiling Point": "1473 °C",
                    "Electronegativity": "1.62",
                    "Discovered": "1861 by William Crookes"
                },
                uses: [
                    "Rat poison and insecticides (historical)",
                    "Infrared optics",
                    "Low-temperature thermometers",
                    "Semiconductor research",
                    "Medical imaging (thallium stress test)"
                ]
            },
            {
                number: 82,
                symbol: "Pb",
                name: "Lead",
                mass: "207.2",
                category: "post-transition-metal",
                group: 14,
                period: 6,
                properties: {
                    "Atomic Mass": "207.2 u",
                    "Phase": "Solid",
                    "Density": "11.34 g/cm³",
                    "Melting Point": "327.46 °C",
                    "Boiling Point": "1749 °C",
                    "Electronegativity": "2.33",
                    "Discovered": "Ancient times"
                },
                uses: [
                    "Lead-acid batteries",
                    "Radiation shielding",
                    "Construction (roofing, pipes)",
                    "Solders",
                    "Weights and ballast"
                ]
            },
            {
                number: 83,
                symbol: "Bi",
                name: "Bismuth",
                mass: "208.98",
                category: "post-transition-metal",
                group: 15,
                period: 6,
                properties: {
                    "Atomic Mass": "208.98 u",
                    "Phase": "Solid",
                    "Density": "9.78 g/cm³",
                    "Melting Point": "271.5 °C",
                    "Boiling Point": "1564 °C",
                    "Electronegativity": "2.02",
                    "Discovered": "1753 by Claude François Geoffroy"
                },
                uses: [
                    "Pepto-Bismol (bismuth subsalicylate)",
                    "Lead replacement in alloys",
                    "Fire detection systems",
                    "Cosmetics and pigments",
                    "Nuclear reactor coolant"
                ]
            },
            // Metalloid (84)
            {
                number: 84,
                symbol: "Po",
                name: "Polonium",
                mass: "[209]",
                category: "metalloid",
                group: 16,
                period: 6,
                properties: {
                    "Atomic Mass": "[209] u",
                    "Phase": "Solid",
                    "Density": "9.196 g/cm³",
                    "Melting Point": "254 °C",
                    "Boiling Point": "962 °C",
                    "Electronegativity": "2.00",
                    "Discovered": "1898 by Marie and Pierre Curie"
                },
                uses: [
                    "Static eliminators (industrial)",
                    "Neutron sources (mixed with beryllium)",
                    "Spacecraft heat sources",
                    "Research applications",
                    "Nuclear weapons initiators"
                ]
            },
            // Halogen (85)
            {
                number: 85,
                symbol: "At",
                name: "Astatine",
                mass: "[210]",
                category: "halogen",
                group: 17,
                period: 6,
                properties: {
                    "Atomic Mass": "[210] u",
                    "Phase": "Solid",
                    "Density": "7 g/cm³ (estimated)",
                    "Melting Point": "302 °C",
                    "Boiling Point": "337 °C",
                    "Electronegativity": "2.20",
                    "Discovered": "1940 by Dale R. Corson, Kenneth Ross MacKenzie, and Emilio Segrè"
                },
                uses: [
                    "Potential cancer treatment (targeted alpha therapy)",
                    "Scientific research",
                    "Radioactive tracer",
                    "Theoretical applications",
                    "Isotope production"
                ]
            },
            // Noble gas (86)
            {
                number: 86,
                symbol: "Rn",
                name: "Radon",
                mass: "[222]",
                category: "noble-gas",
                group: 18,
                period: 6,
                properties: {
                    "Atomic Mass": "[222] u",
                    "Phase": "Gas",
                    "Density": "0.00973 g/cm³",
                    "Melting Point": "-71 °C",
                    "Boiling Point": "-61.7 °C",
                    "Electronegativity": "2.20",
                    "Discovered": "1900 by Friedrich Ernst Dorn"
                },
                uses: [
                    "Cancer treatment (radon seeds)",
                    "Earthquake prediction research",
                    "Radiation therapy (historical)",
                    "Industrial radiography",
                    "Scientific research"
                ]
            },
            // Period 7 (elements 87-118)
            {
                number: 87,
                symbol: "Fr",
                name: "Francium",
                mass: "[223]",
                category: "alkali-metal",
                group: 1,
                period: 7,
                properties: {
                    "Atomic Mass": "[223] u",
                    "Phase": "Solid",
                    "Density": "2.48 g/cm³ (estimated)",
                    "Melting Point": "27 °C",
                    "Boiling Point": "677 °C",
                    "Electronegativity": "0.70",
                    "Discovered": "1939 by Marguerite Perey"
                },
                uses: [
                    "Scientific research",
                    "Potential diagnostic aid in medicine",
                    "Theoretical applications",
                    "Isotope production",
                    "No commercial uses"
                ]
            },
            {
                number: 88,
                symbol: "Ra",
                name: "Radium",
                mass: "[226]",
                category: "alkaline-earth",
                group: 2,
                period: 7,
                properties: {
                    "Atomic Mass": "[226] u",
                    "Phase": "Solid",
                    "Density": "5.5 g/cm³",
                    "Melting Point": "700 °C",
                    "Boiling Point": "1737 °C",
                    "Electronegativity": "0.89",
                    "Discovered": "1898 by Marie and Pierre Curie"
                },
                uses: [
                    "Luminous paint (historical)",
                    "Cancer treatment (radium-223)",
                    "Neutron source (mixed with beryllium)",
                    "Industrial radiography (historical)",
                    "Scientific research"
                ]
            },
            // Actinides (89-103)
            {
                number: 89,
                symbol: "Ac",
                name: "Actinium",
                mass: "[227]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[227] u",
                    "Phase": "Solid",
                    "Density": "10.07 g/cm³",
                    "Melting Point": "1050 °C",
                    "Boiling Point": "3200 °C (estimated)",
                    "Electronegativity": "1.10",
                    "Discovered": "1899 by André-Louis Debierne"
                },
                uses: [
                    "Neutron source (actinium-beryllium)",
                    "Radiation therapy (actinium-225)",
                    "Scientific research",
                    "Radioactive thermoelectric generators",
                    "Trace element in uranium ores"
                ]
            },
            {
                number: 90,
                symbol: "Th",
                name: "Thorium",
                mass: "232.04",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "232.04 u",
                    "Phase": "Solid",
                    "Density": "11.72 g/cm³",
                    "Melting Point": "1750 °C",
                    "Boiling Point": "4788 °C",
                    "Electronegativity": "1.30",
                    "Discovered": "1828 by Jöns Jacob Berzelius"
                },
                uses: [
                    "Potential nuclear fuel (thorium reactors)",
                    "Gas mantles for camping lanterns",
                    "Alloying agent in magnesium",
                    "High-temperature ceramics",
                    "Radiation shielding"
                ]
            },
            {
                number: 91,
                symbol: "Pa",
                name: "Protactinium",
                mass: "231.04",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "231.04 u",
                    "Phase": "Solid",
                    "Density": "15.37 g/cm³",
                    "Melting Point": "1568 °C",
                    "Boiling Point": "4027 °C",
                    "Electronegativity": "1.50",
                    "Discovered": "1913 by Kasimir Fajans and Oswald Helmuth Göhring"
                },
                uses: [
                    "Scientific research",
                    "Radiometric dating",
                    "Theoretical applications",
                    "Nuclear physics studies",
                    "No commercial uses"
                ]
            },
            {
                number: 92,
                symbol: "U",
                name: "Uranium",
                mass: "238.03",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "238.03 u",
                    "Phase": "Solid",
                    "Density": "19.1 g/cm³",
                    "Melting Point": "1132 °C",
                    "Boiling Point": "4131 °C",
                    "Electronegativity": "1.38",
                    "Discovered": "1789 by Martin Heinrich Klaproth"
                },
                uses: [
                    "Nuclear fuel for power generation",
                    "Military applications (nuclear weapons)",
                    "Armor-piercing ammunition",
                    "Radioactive dating (uranium-lead method)",
                    "Counterweights in aircraft"
                ]
            },
            {
                number: 93,
                symbol: "Np",
                name: "Neptunium",
                mass: "[237]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[237] u",
                    "Phase": "Solid",
                    "Density": "20.45 g/cm³",
                    "Melting Point": "644 °C",
                    "Boiling Point": "3902 °C",
                    "Electronegativity": "1.36",
                    "Discovered": "1940 by Edwin McMillan and Philip H. Abelson"
                },
                uses: [
                    "Neutron detection equipment",
                    "Scientific research",
                    "Potential nuclear fuel",
                    "Radioisotope thermoelectric generators",
                    "No commercial uses"
                ]
            },
            {
                number: 94,
                symbol: "Pu",
                name: "Plutonium",
                mass: "[244]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[244] u",
                    "Phase": "Solid",
                    "Density": "19.84 g/cm³",
                    "Melting Point": "639.4 °C",
                    "Boiling Point": "3228 °C",
                    "Electronegativity": "1.28",
                    "Discovered": "1940 by Glenn T. Seaborg, Arthur Wahl, Joseph W. Kennedy, and Edwin McMillan"
                },
                uses: [
                    "Nuclear weapons",
                    "Radioisotope thermoelectric generators (spacecraft)",
                    "Nuclear reactor fuel",
                    "Scientific research",
                    "Neutron sources"
                ]
            },
            {
                number: 95,
                symbol: "Am",
                name: "Americium",
                mass: "[243]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[243] u",
                    "Phase": "Solid",
                    "Density": "12 g/cm³",
                    "Melting Point": "1176 °C",
                    "Boiling Point": "2607 °C",
                    "Electronegativity": "1.30",
                    "Discovered": "1944 by Glenn T. Seaborg, Ralph A. James, Leon O. Morgan, and Albert Ghiorso"
                },
                uses: [
                    "Smoke detectors (americium-241)",
                    "Neutron sources",
                    "Industrial gauges",
                    "Scientific research",
                    "Potential space batteries"
                ]
            },
            {
                number: 96,
                symbol: "Cm",
                name: "Curium",
                mass: "[247]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[247] u",
                    "Phase": "Solid",
                    "Density": "13.51 g/cm³",
                    "Melting Point": "1345 °C",
                    "Boiling Point": "3110 °C",
                    "Electronegativity": "1.30",
                    "Discovered": "1944 by Glenn T. Seaborg, Ralph A. James, and Albert Ghiorso"
                },
                uses: [
                    "Spacecraft power sources",
                    "Scientific research",
                    "X-ray spectrometers",
                    "Potential thermoelectric generators",
                    "No commercial uses"
                ]
            },
            {
                number: 97,
                symbol: "Bk",
                name: "Berkelium",
                mass: "[247]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[247] u",
                    "Phase": "Solid",
                    "Density": "14.78 g/cm³",
                    "Melting Point": "986 °C",
                    "Boiling Point": "2627 °C",
                    "Electronegativity": "1.30",
                    "Discovered": "1949 by Glenn T. Seaborg, Stanley G. Thompson, and Albert Ghiorso"
                },
                uses: [
                    "Scientific research",
                    "Synthesis of heavier elements",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications"
                ]
            },
            {
                number: 98,
                symbol: "Cf",
                name: "Californium",
                mass: "[251]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[251] u",
                    "Phase": "Solid",
                    "Density": "15.1 g/cm³",
                    "Melting Point": "900 °C",
                    "Boiling Point": "1470 °C",
                    "Electronegativity": "1.30",
                    "Discovered": "1950 by Glenn T. Seaborg, Stanley G. Thompson, Kenneth Street Jr., and Albert Ghiorso"
                },
                uses: [
                    "Neutron source for material analysis",
                    "Cancer treatment (californium-252)",
                    "Moisture gauges",
                    "Portable metal detectors",
                    "Nuclear reactor startup"
                ]
            },
            {
                number: 99,
                symbol: "Es",
                name: "Einsteinium",
                mass: "[252]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[252] u",
                    "Phase": "Solid",
                    "Density": "8.84 g/cm³",
                    "Melting Point": "860 °C",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "1.30",
                    "Discovered": "1952 by Albert Ghiorso and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Synthesis of heavier elements",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications"
                ]
            },
            {
                number: 100,
                symbol: "Fm",
                name: "Fermium",
                mass: "[257]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[257] u",
                    "Phase": "Solid",
                    "Density": "Unknown",
                    "Melting Point": "1527 °C",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "1.30",
                    "Discovered": "1952 by Albert Ghiorso and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 101,
                symbol: "Md",
                name: "Mendelevium",
                mass: "[258]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[258] u",
                    "Phase": "Solid",
                    "Density": "Unknown",
                    "Melting Point": "827 °C",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "1.30",
                    "Discovered": "1955 by Albert Ghiorso, Glenn T. Seaborg, and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 102,
                symbol: "No",
                name: "Nobelium",
                mass: "[259]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[259] u",
                    "Phase": "Solid",
                    "Density": "Unknown",
                    "Melting Point": "827 °C",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "1.30",
                    "Discovered": "1966 by Georgy Flerov and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 103,
                symbol: "Lr",
                name: "Lawrencium",
                mass: "[266]",
                category: "actinide",
                group: 3,
                period: 7,
                properties: {
                    "Atomic Mass": "[266] u",
                    "Phase": "Solid",
                    "Density": "Unknown",
                    "Melting Point": "1627 °C",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "1.30",
                    "Discovered": "1961 by Albert Ghiorso and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            // Transition metals (104-112)
            {
                number: 104,
                symbol: "Rf",
                name: "Rutherfordium",
                mass: "[267]",
                category: "transition-metal",
                group: 4,
                period: 7,
                properties: {
                    "Atomic Mass": "[267] u",
                    "Phase": "Solid (predicted)",
                    "Density": "23 g/cm³ (predicted)",
                    "Melting Point": "2100 °C (predicted)",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1964 by Georgy Flerov and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 105,
                symbol: "Db",
                name: "Dubnium",
                mass: "[268]",
                category: "transition-metal",
                group: 5,
                period: 7,
                properties: {
                    "Atomic Mass": "[268] u",
                    "Phase": "Solid (predicted)",
                    "Density": "29 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1967 by Georgy Flerov and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 106,
                symbol: "Sg",
                name: "Seaborgium",
                mass: "[269]",
                category: "transition-metal",
                group: 6,
                period: 7,
                properties: {
                    "Atomic Mass": "[269] u",
                    "Phase": "Solid (predicted)",
                    "Density": "35 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1974 by Albert Ghiorso and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 107,
                symbol: "Bh",
                name: "Bohrium",
                mass: "[270]",
                category: "transition-metal",
                group: 7,
                period: 7,
                properties: {
                    "Atomic Mass": "[270] u",
                    "Phase": "Solid (predicted)",
                    "Density": "37 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1981 by Peter Armbruster and Gottfried Münzenberg"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 108,
                symbol: "Hs",
                name: "Hassium",
                mass: "[269]",
                category: "transition-metal",
                group: 8,
                period: 7,
                properties: {
                    "Atomic Mass": "[269] u",
                    "Phase": "Solid (predicted)",
                    "Density": "41 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1984 by Peter Armbruster and Gottfried Münzenberg"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 109,
                symbol: "Mt",
                name: "Meitnerium",
                mass: "[278]",
                category: "transition-metal",
                group: 9,
                period: 7,
                properties: {
                    "Atomic Mass": "[278] u",
                    "Phase": "Solid (predicted)",
                    "Density": "37.4 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1982 by Peter Armbruster and Gottfried Münzenberg"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 110,
                symbol: "Ds",
                name: "Darmstadtium",
                mass: "[281]",
                category: "transition-metal",
                group: 10,
                period: 7,
                properties: {
                    "Atomic Mass": "[281] u",
                    "Phase": "Solid (predicted)",
                    "Density": "34.8 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1994 by Sigurd Hofmann and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 111,
                symbol: "Rg",
                name: "Roentgenium",
                mass: "[282]",
                category: "transition-metal",
                group: 11,
                period: 7,
                properties: {
                    "Atomic Mass": "[282] u",
                    "Phase": "Solid (predicted)",
                    "Density": "28.7 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1994 by Sigurd Hofmann and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 112,
                symbol: "Cn",
                name: "Copernicium",
                mass: "[285]",
                category: "transition-metal",
                group: 12,
                period: 7,
                properties: {
                    "Atomic Mass": "[285] u",
                    "Phase": "Gas (predicted)",
                    "Density": "23.7 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1996 by Sigurd Hofmann and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            // Post-transition metals (113-116)
            {
                number: 113,
                symbol: "Nh",
                name: "Nihonium",
                mass: "[286]",
                category: "post-transition-metal",
                group: 13,
                period: 7,
                properties: {
                    "Atomic Mass": "[286] u",
                    "Phase": "Solid (predicted)",
                    "Density": "16 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "2003 by Kosuke Morita and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 114,
                symbol: "Fl",
                name: "Flerovium",
                mass: "[289]",
                category: "post-transition-metal",
                group: 14,
                period: 7,
                properties: {
                    "Atomic Mass": "[289] u",
                    "Phase": "Gas (predicted)",
                    "Density": "14 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "1999 by Yuri Oganessian and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 115,
                symbol: "Mc",
                name: "Moscovium",
                mass: "[290]",
                category: "post-transition-metal",
                group: 15,
                period: 7,
                properties: {
                    "Atomic Mass": "[290] u",
                    "Phase": "Solid (predicted)",
                    "Density": "13.5 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "2003 by Yuri Oganessian and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            {
                number: 116,
                symbol: "Lv",
                name: "Livermorium",
                mass: "[293]",
                category: "post-transition-metal",
                group: 16,
                period: 7,
                properties: {
                    "Atomic Mass": "[293] u",
                    "Phase": "Solid (predicted)",
                    "Density": "12.9 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "2000 by Yuri Oganessian and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            // Halogen (117)
            {
                number: 117,
                symbol: "Ts",
                name: "Tennessine",
                mass: "[294]",
                category: "halogen",
                group: 17,
                period: 7,
                properties: {
                    "Atomic Mass": "[294] u",
                    "Phase": "Solid (predicted)",
                    "Density": "7.2 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "2010 by Yuri Oganessian and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            },
            // Noble gas (118)
            {
                number: 118,
                symbol: "Og",
                name: "Oganesson",
                mass: "[294]",
                category: "noble-gas",
                group: 18,
                period: 7,
                properties: {
                    "Atomic Mass": "[294] u",
                    "Phase": "Gas (predicted)",
                    "Density": "5.0 g/cm³ (predicted)",
                    "Melting Point": "Unknown",
                    "Boiling Point": "Unknown",
                    "Electronegativity": "Unknown",
                    "Discovered": "2002 by Yuri Oganessian and colleagues"
                },
                uses: [
                    "Scientific research",
                    "Nuclear physics studies",
                    "No commercial uses",
                    "Theoretical applications",
                    "Synthesis of heavier elements"
                ]
            }
        ];

        // DOM elements
        const periodicTable = document.getElementById('periodicTable');
        const elementDetails = document.getElementById('elementDetails');
        const overlay = document.getElementById('overlay');
        const closeBtn = document.getElementById('closeBtn');
        const searchBox = document.querySelector('.search-box');
        const categoryFilter = document.querySelector('.category-filter');

        // Initialize the periodic table
        function initPeriodicTable() {
            // Clear any existing elements
            periodicTable.innerHTML = '';

            // Create empty cells for the periodic table layout
            // This is a simplified layout that won't perfectly match the standard periodic table
            // but will give a reasonable approximation
            
            // Periods 1-3
            for (let period = 1; period <= 3; period++) {
                // Groups 1-2 (alkali and alkaline earth metals)
                for (let group = 1; group <= 2; group++) {
                    addElementToTable(period, group);
                }
                
                // Groups 13-18 (p-block)
                for (let group = 13; group <= 18; group++) {
                    addElementToTable(period, group);
                }
            }
            
            // Periods 4-7 (including transition metals, lanthanides, actinides)
            for (let period = 4; period <= 7; period++) {
                // Groups 1-18
                for (let group = 1; group <= 18; group++) {
                    // Skip some positions to create gaps in the table
                    if (period === 6 && group >= 3 && group <= 12) continue;
                    if (period === 7 && group >= 3 && group <= 12) continue;
                    
                    addElementToTable(period, group);
                }
            }
            
            // Lanthanides and actinides (placed separately at the bottom)
            // Lanthanides (period 6, group 3)
            for (let i = 57; i <= 71; i++) {
                addElementToTable(6, 3, i);
            }
            
            // Actinides (period 7, group 3)
            for (let i = 89; i <= 103; i++) {
                addElementToTable(7, 3, i);
            }
        }

        // Add an element to the periodic table
        function addElementToTable(period, group, number) {
            // Find the element by number or by period and group
            let element;
            if (number) {
                element = elements.find(el => el.number === number);
            } else {
                element = elements.find(el => el.period === period && el.group === group);
            }
            
            const elementDiv = document.createElement('div');
            
            if (element) {
                elementDiv.className = `element ${element.category}`;
                elementDiv.innerHTML = `
                    <div class="number">${element.number}</div>
                    <div class="symbol">${element.symbol}</div>
                    <div class="name">${element.name}</div>
                    <div class="mass">${element.mass}</div>
                `;
                
                // Add click event to show details
                elementDiv.addEventListener('click', () => showElementDetails(element));
            } else {
                // Empty cell for layout purposes
                elementDiv.className = 'empty';
            }
            
            periodicTable.appendChild(elementDiv);
        }

        // Show element details
        function showElementDetails(element) {
            // Set header information
            document.getElementById('detailSymbol').textContent = element.symbol;
            document.getElementById('detailName').textContent = element.name;
            document.getElementById('detailNumber').textContent = `Atomic Number: ${element.number}`;
            document.getElementById('detailCategory').textContent = formatCategory(element.category);
            
            // Set properties
            const propertiesDiv = document.getElementById('detailProperties');
            propertiesDiv.innerHTML = '';
            
            for (const [key, value] of Object.entries(element.properties)) {
                const propertyDiv = document.createElement('div');
                propertyDiv.className = 'property';
                propertyDiv.innerHTML = `<span class="property-name">${key}:</span> ${value}`;
                propertiesDiv.appendChild(propertyDiv);
            }
            
            // Set uses
            const usesDiv = document.getElementById('detailUses');
            usesDiv.innerHTML = '';
            
            element.uses.forEach(use => {
                const useDiv = document.createElement('div');
                useDiv.className = 'property';
                useDiv.innerHTML = `• ${use}`;
                usesDiv.appendChild(useDiv);
            });
            
            // Show the details panel and overlay
            elementDetails.classList.add('active');
            overlay.classList.add('active');
        }

        // Format category for display
        function formatCategory(category) {
            const formatted = category.replace(/-/g, ' ');
            return formatted.charAt(0).toUpperCase() + formatted.slice(1);
        }

        // Close element details
        function closeElementDetails() {
            elementDetails.classList.remove('active');
            overlay.classList.remove('active');
        }

        // Filter elements based on search and category
        function filterElements() {
            const searchTerm = searchBox.value.toLowerCase();
            const category = categoryFilter.value;
            
            document.querySelectorAll('.element').forEach(el => {
                const elementNumber = parseInt(el.querySelector('.number').textContent);
                const element = elements.find(e => e.number === elementNumber);
                
                const matchesSearch = element.name.toLowerCase().includes(searchTerm) || 
                                    element.symbol.toLowerCase().includes(searchTerm) || 
                                    element.number.toString().includes(searchTerm);
                
                const matchesCategory = category === 'all' || element.category === category;
                
                if (matchesSearch && matchesCategory) {
                    el.style.display = 'flex';
                } else {
                    el.style.display = 'none';
                }
            });
        }

        // Event listeners
        closeBtn.addEventListener('click', closeElementDetails);
        overlay.addEventListener('click', closeElementDetails);
        searchBox.addEventListener('input', filterElements);
        categoryFilter.addEventListener('change', filterElements);

        // Initialize the app
        initPeriodicTable();
    </script>
</body>
</html>
