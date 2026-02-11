<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Browse and find upcoming programs">
    <title>Browse Programs - Recreation Academy</title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; background: #f5f5f5; }
        
        /* HEADER */
        header { background: white; padding: 20px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .header-content { max-width: 1000px; margin: 0 auto; text-align: center; }
        header h1 { color: #1a365d; margin-bottom: 8px; }
        header p { color: #6b7280; }
        
        /* MAIN CONTENT */
        main { padding: 40px 20px; }
        
        /* FIND YOUR PROGRAM SECTION */
        .fp-section { padding: 24px 24px; background: #fff; font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif; }
        .fp-container { max-width: 1000px; margin: 0 auto; }
        .fp-header { text-align: center; margin-bottom: 48px; }
        .fp-header h2 { font-size: 36px; font-weight: 700; color: #1a365d; margin-bottom: 16px; }
        .fp-header p { font-size: 16px; color: #4b5563; }
        .fp-types-label { text-align: center; font-size: 12px; font-weight: 600; color: #6b7280; text-transform: uppercase; letter-spacing: 0.05em; margin-bottom: 16px; }
        .fp-types-list { display: flex; flex-wrap: wrap; justify-content: center; gap: 12px; margin-bottom: 32px; }
        .fp-type-badge { display: flex; align-items: center; gap: 8px; background: #f9fafb; border: 1px solid #e5e7eb; border-radius: 9999px; padding: 8px 16px; }
        .fp-type-badge svg { width: 16px; height: 16px; color: #35BDE8; fill: none; stroke: currentColor; stroke-width: 2; }
        .fp-type-badge span { font-size: 14px; font-weight: 600; color: #1a365d; }
        .fp-box { background: #fff; border: 2px solid #35BDE8; border-radius: 24px; padding: 24px; margin-bottom: 24px; }
        .fp-box h3 { font-size: 20px; font-weight: 700; color: #1a365d; margin-bottom: 8px; }
        .fp-box > p { font-size: 14px; color: #4b5563; margin-bottom: 24px; }
        .fp-dropdown-wrap { position: relative; margin-bottom: 24px; }
        .fp-dropdown-btn { width: 100%; display: flex; align-items: center; justify-content: space-between; background: #f9fafb; border: 2px solid #e5e7eb; border-radius: 12px; padding: 16px 20px; text-align: left; cursor: pointer; transition: border-color 0.2s; font-size: 15px; }
        .fp-dropdown-btn:hover { border-color: #35BDE8; }
        .fp-dropdown-btn:disabled { opacity: 0.5; cursor: not-allowed; }
        .fp-dropdown-btn .label { color: #6b7280; }
        .fp-dropdown-btn .label.selected { color: #1a365d; font-weight: 600; }
        .fp-dropdown-btn .chevron { width: 20px; height: 20px; color: #9ca3af; transition: transform 0.2s; }
        .fp-dropdown-btn.open .chevron { transform: rotate(180deg); }
        .fp-dropdown-menu { position: absolute; z-index: 30; width: 100%; margin-top: 8px; background: #fff; border: 2px solid #e5e7eb; border-radius: 12px; box-shadow: 0 10px 40px rgba(0,0,0,0.15); max-height: 320px; overflow-y: auto; display: none; }
        .fp-dropdown-menu.open { display: block; }
        .fp-season-header { padding: 8px 16px; background: #f9fafb; border-bottom: 1px solid #e5e7eb; position: sticky; top: 0; }
        .fp-season-header span { font-size: 12px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.05em; color: #6b7280; }
        .fp-season-header span.current { color: #35BDE8; }
        .fp-dropdown-item { width: 100%; text-align: left; padding: 12px 20px; border: none; background: none; cursor: pointer; border-bottom: 1px solid #f3f4f6; font-size: 15px; }
        .fp-dropdown-item:hover { background: rgba(53,189,232,0.1); }
        .fp-dropdown-item span { font-weight: 500; color: #1a365d; }
        .fp-dropdown-empty { padding: 12px 20px; color: #9ca3af; font-style: italic; font-size: 14px; }
        .fp-loading { display: none; flex-direction: column; align-items: center; justify-content: center; padding: 48px 0; }
        .fp-loading.show { display: flex; }
        .fp-loading img { width: 80px; height: 80px; animation: fpFade 1s ease-in-out infinite; }
        .fp-loading p { color: #6b7280; margin-top: 16px; font-size: 14px; }
        @keyframes fpFade { 0%, 100% { opacity: 0.3; } 50% { opacity: 1; } }
        .fp-results { display: none; }
        .fp-results.show { display: block; }
        .fp-results-header { display: flex; flex-direction: column; gap: 16px; margin-bottom: 16px; }
        .fp-results-summary { font-size: 14px; color: #6b7280; }
        .fp-results-summary strong { color: #1a365d; }
        .fp-view-toggle { display: flex; background: #f3f4f6; border-radius: 8px; padding: 4px; }
        .fp-view-toggle button { display: flex; align-items: center; gap: 6px; padding: 6px 12px; border-radius: 6px; font-size: 14px; font-weight: 500; border: none; cursor: pointer; background: transparent; color: #6b7280; }
        .fp-view-toggle button:hover { color: #1a365d; }
        .fp-view-toggle button.active { background: #fff; color: #1a365d; box-shadow: 0 1px 3px rgba(0,0,0,0.1); }
        .fp-view-toggle button svg { width: 16px; height: 16px; }
        .fp-card { background: #f9fafb; border: 2px solid #f3f4f6; border-radius: 16px; padding: 20px; margin-bottom: 16px; transition: border-color 0.2s; }
        .fp-card:hover { border-color: #35BDE8; }
        .fp-card-inner { display: flex; flex-direction: column; gap: 16px; }
        .fp-card h4 { font-size: 18px; font-weight: 700; color: #1a365d; margin-bottom: 4px; }
        .fp-card .address { font-size: 14px; color: #6b7280; margin-bottom: 12px; }
        .fp-card .sessions-label { font-size: 12px; font-weight: 600; color: #9ca3af; text-transform: uppercase; margin-bottom: 8px; }
        .fp-card .session { display: flex; flex-wrap: wrap; align-items: center; gap: 8px; font-size: 14px; color: #4b5563; margin-bottom: 6px; }
        .fp-card .session svg { width: 14px; height: 14px; color: #35BDE8; }
        .fp-card .session .dot { color: #9ca3af; }
        .fp-register-btn { display: inline-flex; align-items: center; gap: 8px; background: #35BDE8; color: #fff; font-weight: 700; padding: 12px 24px; border-radius: 9999px; font-size: 14px; text-decoration: none; transition: background 0.2s; white-space: nowrap; }
        .fp-register-btn:hover { background: #2da8d1; color: #fff; }
        .fp-register-btn svg { width: 16px; height: 16px; }
        .fp-no-results { text-align: center; padding: 32px 0; display: none; }
        .fp-no-results.show { display: block; }
        .fp-no-results p { color: #6b7280; }
        .fp-note { text-align: center; font-size: 14px; color: #6b7280; font-style: italic; max-width: 600px; margin: 0 auto; }
        .fp-note .ast { color: #35BDE8; font-style: normal; font-weight: 700; }
        
        /* FOOTER */
        footer { background: #1a365d; color: white; text-align: center; padding: 20px; margin-top: 40px; }
        footer a { color: #35BDE8; text-decoration: none; }
        footer a:hover { text-decoration: underline; }
        
        @media (min-width: 768px) {
            .fp-header h2 { font-size: 48px; }
            .fp-box { padding: 32px; }
            .fp-results-header { flex-direction: row; align-items: center; justify-content: space-between; }
            .fp-card-inner { flex-direction: row; align-items: flex-start; justify-content: space-between; }
        }
    </style>
</head>
<body>
    <header>
        <div class="header-content">
            <h1>Program Finder</h1>
            <p>Discover and register for programs near you</p>
        </div>
    </header>

    <main>
        <section class="fp-section" id="find-program">
            <div class="fp-container">
                <div class="fp-header">
                    <h2>Browse Upcoming Programs</h2>
                    <p>Select a program to see all locations and dates</p>
                </div>

                <p class="fp-types-label">Program Types We Offer</p>
                <div class="fp-types-list">
                    <div class="fp-type-badge">
                        <svg viewBox="0 0 24 24"><path d="M3.5 21 L14 3"/><path d="M20.5 21 L10 3"/><path d="M15.5 21 L12 15l-3.5 6"/><path d="M2 21h20"/></svg>
                        <span>School Break Camps</span>
                    </div>
                    <div class="fp-type-badge">
                        <svg viewBox="0 0 24 24"><path d="M4 19.5v-15A2.5 2.5 0 0 1 6.5 2H20v20H6.5a2.5 2.5 0 0 1 0-5H20"/></svg>
                        <span>Enrichment Clubs</span>
                    </div>
                    <div class="fp-type-badge">
                        <svg viewBox="0 0 24 24"><path d="M6 9H4.5a2.5 2.5 0 0 1 0-5H6"/><path d="M18 9h1.5a2.5 2.5 0 0 0 0-5H18"/><path d="M4 22h16"/><path d="M10 14.66V17c0 .55-.47.98-.97 1.21C7.85 18.75 7 20.24 7 22"/><path d="M14 14.66V17c0 .55.47.98.97 1.21C16.15 18.75 17 20.24 17 22"/><path d="M18 2H6v7a6 6 0 0 0 12 0V2Z"/></svg>
                        <span>Sports Clinics</span>
                    </div>
                    <div class="fp-type-badge">
                        <svg viewBox="0 0 24 24"><path d="M5.8 11.3 L2 22l10.7-3.79"/><path d="M4 3h.01"/><path d="M22 8h.01"/><path d="M15 2h.01"/><path d="M22 20h.01"/><path d="m22 2-2.24.75a2.9 2.9 0 0 0-1.96 3.12c.1.86-.57 1.63-1.45 1.63h-.38c-.86 0-1.6.6-1.76 1.44L14 10"/><path d="m22 13-.82-.33c-.86-.34-1.82.2-1.98 1.11c-.11.7-.72 1.22-1.43 1.22H17"/><path d="m11 2 .33.82c.34.86-.2 1.82-1.11 1.98C9.52 4.9 9 5.52 9 6.23V7"/><path d="M11 13c1.93 1.93 2.83 4.17 2 5-.83.83-3.07-.07-5-2-1.93-1.93-2.83-4.17-2-5 .83-.83 3.07.07 5 2Z"/></svg>
                        <span>Special Events</span>
                    </div>
                    <div class="fp-type-badge">
                        <svg viewBox="0 0 24 24"><path d="M8 6v6"/><path d="M15 6v6"/><path d="M2 12h19.6"/><path d="M18 18h3s.5-1.7.8-2.8c.1-.4.2-.8.2-1.2 0-.4-.1-.8-.2-1.2l-1.4-5C20.1 6.8 19.1 6 18 6H4a2 2 0 0 0-2 2v10h3"/><circle cx="7" cy="18" r="2"/><path d="M9 18h5"/><circle cx="16" cy="18" r="2"/></svg>
                        <span>Mobile Field Trips</span>
                    </div>
                </div>

                <div class="fp-box">
                    <h3>Find Programs by Type</h3>
                    <p>Select a program to see all locations and dates</p>
                    
                    <div class="fp-dropdown-wrap">
                        <button class="fp-dropdown-btn" id="fpDropdownBtn" disabled>
                            <span class="label" id="fpDropdownLabel">Loading programs...</span>
                            <svg class="chevron" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="m6 9 6 6 6-6"/></svg>
                        </button>
                        <div class="fp-dropdown-menu" id="fpDropdownMenu"></div>
                    </div>

                    <div class="fp-loading" id="fpLoading">
                        <img src="https://us.chat-img.sintra.ai/dab438fd-0c39-41b0-b266-97242bae1c44/b95979ff-dd69-4854-b232-ace76cc0c2ea/High_Quality_EMBLEM_ONLY_Logo.1.png" alt="Loading...">
                        <p>Finding programs...</p>
                    </div>

                    <div class="fp-results" id="fpResults">
                        <div class="fp-results-header">
                            <p class="fp-results-summary" id="fpSummary"></p>
                            <div class="fp-view-toggle" id="fpViewToggle">
                                <button class="active" data-view="location">
                                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M6 22V4a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v18Z"/><path d="M6 12H4a2 2 0 0 0-2 2v6a2 2 0 0 0 2 2h2"/><path d="M18 9h2a2 2 0 0 1 2 2v9a2 2 0 0 1-2 2h-2"/><path d="M10 6h4"/><path d="M10 10h4"/><path d="M10 14h4"/><path d="M10 18h4"/></svg>
                                    By Partner
                                </button>
                                <button data-view="week">
                                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><line x1="8" x2="21" y1="6" y2="6"/><line x1="8" x2="21" y1="12" y2="12"/><line x1="8" x2="21" y1="18" y2="18"/><line x1="3" x2="3.01" y1="6" y2="6"/><line x1="3" x2="3.01" y1="12" y2="12"/><line x1="3" x2="3.01" y1="18" y2="18"/></svg>
                                    By Week
                                </button>
                            </div>
                        </div>
                        <div id="fpResultsList"></div>
                    </div>

                    <div class="fp-no-results" id="fpNoResults">
                        <p>No locations found for this program.</p>
                    </div>
                </div>

                <p class="fp-note">
                    <span class="ast">*</span> You can also browse our partner locations below.<br>
                    Please note: Some partner locations manage their own registration systems and offer programs other than ours, so search for "Recreation Academy" to find our offerings.
                </p>
            </div>
        </section>
    </main>

    <footer>
        <p>&copy; 2026 Recreation Academy. All rights reserved. | <a href="#">Contact Us</a></p>
    </footer>

    <!-- PROGRAM FINDER JAVASCRIPT -->
    <script>
        // ========== CONFIGURATION ==========
        const SPREADSHEET_URL = "https://docs.google.com/spreadsheets/d/e/2PACX-1vRmVxFuvyhCg_tUIoSVWj_-GL5ysER-vh2ULEgZ8Xo2yFpOr_8RSNkxzECng4FAID2xOqaVlm3nvj7S/pub?gid=0&single=true&output=csv";

        const PARTNER_LINKS = {
            "Kirkwood Parks & Recreation": "https://www.care.com/connect/recacademystl/providers?locations%5B%5D=1127493",
            "All Saints Catholic Parish and School": "https://www.care.com/connect/recacademystl/providers?locations%5B%5D=1112824",
            "Shrewsbury Parks & Recreation": "https://www.care.com/connect/recacademystl/",
            "Beal Center": "https://www.care.com/connect/recacademystl/providers?locations%5B%5D=1258442",
            "Afton Community Center": "https://www.care.com/connect/recacademystl/",
            "Kennedy Recreation Complex": "https://www.care.com/connect/recacademystl/",
            "New City School": "https://newcityschool.campbrainregistration.com/",
            "Rohan Woods School": "https://rohanwoods.org/",
            "Community School": "https://communityschool.campbrainregistration.com/",
            "St. Louis Public Library": "https://slpl.bibliocommons.com/v2/events?audiences=5769993d3ed177083d000012",
            "Lindbergh Community Education": "https://lindberghschools.ce.eleyo.com/courses/category/64/enrichment",
            "Lindbergh Athletics Department": "https://lindberghschools.ce.eleyo.com/courses/category/64/enrichment",
            "Ladue School District": "https://ladueschools.arux.app/",
            "Brentwood Parks and recreation": "https://www.brentwoodmo.org/1792/Programs-Athletics"
        };

        const SEASON_EMOJIS = { Winter: "❄️", Spring: "🌸", Summer: "☀️", Fall: "🍂", "Year-Round": "📅" };

        // ========== GLOBAL STATE ==========
        let allPrograms = [];
        let programsBySeason = {};
        let currentSeason = "";
        let orderedSeasons = [];
        let selectedProgram = "";
        let filteredPrograms = [];
        let groupedPrograms = [];
        let viewMode = "location";

        // ========== HELPER FUNCTIONS ==========
        function getCurrentSeason() {
            const month = new Date().getMonth();
            if (month >= 2 && month <= 4) return "Spring";
            if (month >= 5 && month <= 7) return "Summer";
            if (month >= 8 && month <= 10) return "Fall";
            return "Winter";
        }

        function getSeasonOrder(current) {
            const seasons = ["Winter", "Spring", "Summer", "Fall"];
            const idx = seasons.indexOf(current);
            return [...seasons.slice(idx), ...seasons.slice(0, idx)];
        }

        function getSeasonFromDate(date) {
            if (!date) return "Year-Round";
            const month = date.getMonth();
            const day = date.getDate();
            if (month === 4 && day >= 25) return "Summer";
            if (month >= 5 && month <= 7) return "Summer";
            if (month >= 2 && month <= 4) return "Spring";
            if (month >= 8 && month <= 10) return "Fall";
            return "Winter";
        }

        function parseDate(dateStr) {
            if (!dateStr) return null;
            const cleaned = dateStr.trim();
            const parsed = new Date(cleaned);
            if (!isNaN(parsed.getTime())) return parsed;
            const parts = cleaned.match(/(\d{1,2})\/(\d{1,2})\/(\d{2,4})/);
            if (parts) {
                const year = parts[3].length === 2 ? 2000 + parseInt(parts[3]) : parseInt(parts[3]);
                return new Date(year, parseInt(parts[1]) - 1, parseInt(parts[2]));
            }
            return null;
        }

        function parseCSVLine(line) {
            const result = [];
            let current = "";
            let inQuotes = false;
            for (let i = 0; i < line.length; i++) {
                const char = line[i];
                if (char === '"') inQuotes = !inQuotes;
                else if (char === "," && !inQuotes) { result.push(current); current = ""; }
                else current += char;
            }
            result.push(current);
            return result;
        }

        function getPartnerLink(location) {
            if (PARTNER_LINKS[location]) return PARTNER_LINKS[location];
            for (const [key, value] of Object.entries(PARTNER_LINKS)) {
                if (location.toLowerCase().includes(key.toLowerCase()) || key.toLowerCase().includes(location.toLowerCase())) {
                    return value;
                }
            }
            return "https://www.care.com/connect/recacademystl/";
        }

        function groupByLocation(programs) {
            const grouped = {};
            programs.forEach(p => {
                if (!grouped[p.location]) {
                    grouped[p.location] = { location: p.location, address: p.address, sessions: [] };
                }
                grouped[p.location].sessions.push({ dates: p.dates, day: p.day, time: p.time, startDate: p.startDate });
            });
            Object.values(grouped).forEach(g => {
                g.sessions.sort((a, b) => {
                    if (!a.startDate) return 1;
                    if (!b.startDate) return -1;
                    return a.startDate.getTime() - b.startDate.getTime();
                });
            });
            return Object.values(grouped).sort((a, b) => a.location.localeCompare(b.location));
        }

        // ========== DOM ELEMENTS ==========
        const dropdownBtn = document.getElementById("fpDropdownBtn");
        const dropdownLabel = document.getElementById("fpDropdownLabel");
        const dropdownMenu = document.getElementById("fpDropdownMenu");
        const loadingEl = document.getElementById("fpLoading");
        const resultsEl = document.getElementById("fpResults");
        const summaryEl = document.getElementById("fpSummary");
        const resultsListEl = document.getElementById("fpResultsList");
        const noResultsEl = document.getElementById("fpNoResults");
        const viewToggle = document.getElementById("fpViewToggle");

        // ========== FETCH & PARSE DATA ==========
        async function fetchPrograms() {
            try {
                const response = await fetch(SPREADSHEET_URL);
                const csvText = await response.text();
                const lines = csvText.split("\n");
                const parsedPrograms = [];
                const typesBySeason = { Winter: new Set(), Spring: new Set(), Summer: new Set(), Fall: new Set(), "Year-Round": new Set() };

                for (let i = 1; i < lines.length; i++) {
                    if (!lines[i].trim()) continue;
                    const values = parseCSVLine(lines[i]);
                    const hideProgram = values[20]?.trim().toUpperCase();
                    if (hideProgram === "TRUE") continue;
                    const programType = values[2]?.trim();
                    const startDateStr = values[3]?.trim();
                    const startDate = parseDate(startDateStr);
                    if (programType && programType !== "" && programType !== "SUMMER") {
                        const programSeason = getSeasonFromDate(startDate);
                        typesBySeason[programSeason].add(programType);
                        parsedPrograms.push({
                            location: values[0]?.trim() || "",
                            address: values[1]?.trim() || "",
                            programType,
                            dates: values[4]?.trim() || "",
                            day: values[5]?.trim() || "",
                            time: values[6]?.trim() || "",
                            startDate
                        });
                    }
                }

                allPrograms = parsedPrograms;
                for (const [s, types] of Object.entries(typesBySeason)) {
                    programsBySeason[s] = Array.from(types).sort();
                }

                dropdownBtn.disabled = false;
                dropdownLabel.textContent = "Select a Program Type";
                buildDropdownMenu();
            } catch (error) {
                console.error("Error fetching programs:", error);
                dropdownLabel.textContent = "Error loading programs";
            }
        }

        // ========== BUILD DROPDOWN ==========
        function buildDropdownMenu() {
            dropdownMenu.innerHTML = "";
            orderedSeasons.forEach(season => {
                const header = document.createElement("div");
                header.className = "fp-season-header";
                const isCurrent = season === currentSeason;
                header.innerHTML = `<span class="${isCurrent ? 'current' : ''}">${SEASON_EMOJIS[season]} ${season}${isCurrent ? " (Current)" : ""}</span>`;
                dropdownMenu.appendChild(header);

                const programs = programsBySeason[season] || [];
                if (programs.length > 0) {
                    programs.forEach(prog => {
                        const item = document.createElement("button");
                        item.className = "fp-dropdown-item";
                        item.innerHTML = `<span>${prog}</span>`;
                        item.addEventListener("click", () => selectProgram(prog));
                        dropdownMenu.appendChild(item);
                    });
                } else {
                    const empty = document.createElement("div");
                    empty.className = "fp-dropdown-empty";
                    empty.textContent = "Updates coming soon";
                    dropdownMenu.appendChild(empty);
                }
            });
        }

        // ========== DROPDOWN TOGGLE ==========
        dropdownBtn.addEventListener("click", () => {
            const isOpen = dropdownMenu.classList.contains("open");
            dropdownMenu.classList.toggle("open", !isOpen);
            dropdownBtn.classList.toggle("open", !isOpen);
        });

        document.addEventListener("click", (e) => {
            if (!dropdownBtn.contains(e.target) && !dropdownMenu.contains(e.target)) {
                dropdownMenu.classList.remove("open");
                dropdownBtn.classList.remove("open");
            }
        });

        // ========== SELECT PROGRAM ==========
        function selectProgram(programType) {
            selectedProgram = programType;
            dropdownLabel.textContent = programType;
            dropdownLabel.classList.add("selected");
            dropdownMenu.classList.remove("open");
            dropdownBtn.classList.remove("open");

            resultsEl.classList.remove("show");
            noResultsEl.classList.remove("show");
            loadingEl.classList.add("show");

            setTimeout(() => {
                filteredPrograms = allPrograms.filter(p => p.programType === programType);
                filteredPrograms.sort((a, b) => {
                    if (!a.startDate) return 1;
                    if (!b.startDate) return -1;
                    return a.startDate.getTime() - b.startDate.getTime();
                });
                groupedPrograms = groupByLocation(filteredPrograms);
                viewMode = "location";
                loadingEl.classList.remove("show");
                renderResults();
            }, 600);
        }

        // ========== VIEW TOGGLE ==========
        viewToggle.addEventListener("click", (e) => {
            const btn = e.target.closest("button");
            if (!btn) return;
            const newView = btn.dataset.view;
            if (newView && newView !== viewMode) {
                viewMode = newView;
                viewToggle.querySelectorAll("button").forEach(b => b.classList.remove("active"));
                btn.classList.add("active");
                renderResults();
            }
        });

        // ========== RENDER RESULTS ==========
        function renderResults() {
            if (filteredPrograms.length === 0) {
                noResultsEl.classList.add("show");
                resultsEl.classList.remove("show");
                return;
            }

            noResultsEl.classList.remove("show");
            resultsEl.classList.add("show");

            // Summary text
            const total = filteredPrograms.length;
            const locations = groupedPrograms.length;
            if (total === 1) {
                summaryEl.innerHTML = `<strong>1</strong> session for <strong>${selectedProgram}</strong>`;
            } else if (locations === 1) {
                summaryEl.innerHTML = `<strong>${total}</strong> sessions at <strong>1</strong> location for <strong>${selectedProgram}</strong>`;
            } else {
                summaryEl.innerHTML = `<strong>${total}</strong> sessions across <strong>${locations}</strong> locations for <strong>${selectedProgram}</strong>`;
            }

            // Show/hide toggle if only 1 result
            viewToggle.style.display = total > 1 ? "flex" : "none";

            // Render cards
            resultsListEl.innerHTML = "";
            if (viewMode === "location") {
                groupedPrograms.forEach(group => {
                    resultsListEl.innerHTML += renderLocationCard(group);
                });
            } else {
                filteredPrograms.forEach(prog => {
                    resultsListEl.innerHTML += renderWeekCard(prog);
                });
            }
        }

        function renderLocationCard(group) {
            const sessionsHtml = group.sessions.map(s => `
                <div class="session">
                    <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect width="18" height="18" x="3" y="4" rx="2" ry="2"/><line x1="16" x2="16" y1="2" y2="6"/><line x1="8" x2="8" y1="2" y2="6"/><line x1="3" x2="21" y1="10" y2="10"/></svg>
                    ${s.dates}
                    ${s.day ? `<span class="dot">•</span>${s.day}` : ""}
                    ${s.time ? `<span class="dot">•</span><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>${s.time}` : ""}
                </div>
            `).join("");

            return `
                <div class="fp-card">
                    <div class="fp-card-inner">
                        <div style="flex:1">
                            <h4>${group.location}</h4>
                            ${group.address ? `<p class="address">${group.address}</p>` : ""}
                            <p class="sessions-label">Available Sessions:</p>
                            ${sessionsHtml}
                        </div>
                        <a class="fp-register-btn" href="${getPartnerLink(group.location)}" target="_blank">
                            Register
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" x2="21" y1="14" y2="3"/></svg>
                        </a>
                    </div>
                </div>
            `;
        }

        function renderWeekCard(prog) {
            return `
                <div class="fp-card">
                    <div class="fp-card-inner">
                        <div style="flex:1">
                            <h4>${prog.location}</h4>
                            ${prog.address ? `<p class="address">${prog.address}</p>` : ""}
                            <div class="session">
                                ${prog.dates ? `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><rect width="18" height="18" x="3" y="4" rx="2"/><line x1="16" x2="16" y1="2" y2="6"/><line x1="8" x2="8" y1="2" y2="6"/><line x1="3" x2="21" y1="10" y2="10"/></svg>${prog.dates}` : ""}
                                ${prog.day ? `<span class="dot">•</span>${prog.day}` : ""}
                                ${prog.time ? `<span class="dot">•</span><svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>${prog.time}` : ""}
                            </div>
                        </div>
                        <a class="fp-register-btn" href="${getPartnerLink(prog.location)}" target="_blank">
                            Register
                            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"/><polyline points="15 3 21 3 21 9"/><line x1="10" x2="21" y1="14" y2="3"/></svg>
                        </a>
                    </div>
                </div>
            `;
        }

        // ========== INITIALIZE ==========
        currentSeason = getCurrentSeason();
        orderedSeasons = getSeasonOrder(currentSeason);
        fetchPrograms();
    </script>
</body>
</html>
