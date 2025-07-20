<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>The Evolution to Agentic Company</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutrals & Slate Blue -->
    <!-- Application Structure Plan: A single-page, narrative-driven application that guides users through the three evolutionary stages of a company (Digitalized, Digital, Agentic) using interactive tabs. It then visually and textually highlights the "Leapfrog" opportunity for Digitalized companies to jump directly to the Agentic stage, using an HTML/CSS diagram and interactive accordions for details. A new timeline section illustrates key AI technology developments from November 2022 to AGI, connecting them to the "leapfrogging" strategy and optimal timing, with specific expert assumptions for 2025+. The experience is complemented by a dynamic Chart.js scatter plot visualizing the business business value vs. technological maturity of each stage. All LLM-related features have been removed for simplified deployment. New features include a language selector for English, German, Portuguese, and Chinese (Simplified). -->
    <!-- Visualization & Content Choices:
        - **Company Stages**: Report Info -> Comparison of three company types. Goal -> Inform and compare. Viz/Presentation -> Interactive Tabs (HTML/CSS/JS) with icons for clarity. Interaction -> Clickable tabs to reveal details, focusing user attention. Justification -> Breaks down information into digestible parts, encourages active exploration over passive reading. Library/Method -> Vanilla JS.
        - **Leapfrog Opportunity**: Report Info -> The strategic jump from Digitalized to Agentic. Goal -> Explain a core strategic concept. Viz/Presentation -> A non-SVG diagram built with styled HTML divs and an animated arrow effect. Key points ("Why possible?", "What's needed?") are in interactive accordions. Interaction -> Clicking accordion headers expands details. Justification -> Makes the abstract "Leapfrog" concept tangible and visually impactful. Library/Method -> Vanilla JS & Tailwind CSS.
        - **AI Evolution Timeline**: Report Info -> Fundamental AI technology development from Nov 2022 to AGI, now with expert assumptions for 2025+. Goal -> Illustrate timing for leapfrogging and the accelerating pace of AI. Viz/Presentation -> HTML/CSS-based vertical timeline with dates, events, and relevance. Interaction -> Static display with clear visual flow. Justification -> Provides context for the rapid pace of AI development and the urgency of the agentic shift, emphasizing future capabilities. Library/Method -> HTML/CSS/Tailwind.
        - **Maturity Model**: Report Info -> Implicit relationship between tech investment and business value. Goal -> Provide an analytical, data-driven perspective. Viz/Presentation -> Interactive scatter plot. Interaction -> Hovering over data points shows tooltips with summaries. Justification -> Reinforces the core message quantitatively and adds another layer of engagement. Library/Method -> Chart.js on Canvas.
        - **Language Selector**: Report Info -> Multi-language support. Goal -> Enhance accessibility and user experience. Viz/Presentation -> Dropdown menu. Interaction -> Selecting an option changes all visible text. Justification -> Allows users to consume content in their preferred language. Library/Method -> Vanilla JS.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8f7f4;
            color: #1f2937;
        }
        .tab-active {
            background-color: #374151;
            color: #ffffff;
            border-color: #374151;
        }
        .tab-inactive {
            background-color: #ffffff;
            color: #4b5563;
            border-color: #d1d5db;
        }
        .accordion-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.5s ease-in-out;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 800px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 500px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 450px;
            }
        }
        .leapfrog-arrow div {
            transition: all 0.5s ease-in-out;
        }
        .leapfrog-arrow:hover .arrow-body {
            width: 100%;
        }
        .leapfrog-arrow:hover .arrow-head {
            transform: translateX(5px);
            border-color: #2563eb;
        }
        .timeline-item {
            position: relative;
            padding: 20px 0;
        }
        .timeline-item::before {
            content: '';
            position: absolute;
            width: 10px;
            height: 10px;
            background-color: #60a5fa; /* Blue dot */
            border-radius: 50%;
            z-index: 1;
            top: 50%;
            transform: translateY(-50%);
        }
        .timeline-line {
            position: absolute;
            width: 2px;
            background-color: #d1d5db; /* Light gray line */
            top: 0;
            bottom: 0;
        }
        /* Mobile: stacked, line on left */
        .timeline-item::before {
            left: -6px; /* Position dot on the line */
        }
        .timeline-line {
            left: 0;
        }
        .timeline-content {
            padding-left: 30px; /* Space for line and dots */
        }
        /* Desktop: alternating left/right, line in center */
        @media (min-width: 768px) {
            .timeline-line {
                left: 50%;
                transform: translateX(-1px);
            }
            .timeline-item {
                width: 50%;
                padding: 20px 40px; /* Space for line and dots */
            }
            .timeline-item.left {
                left: 0;
                text-align: right;
            }
            .timeline-item.right {
                left: 50%;
                text-align: left;
            }
            .timeline-item.left::before {
                left: calc(100% - 34px); /* Dot on the right of left item */
            }
            .timeline-item.right::before {
                left: -6px; /* Dot on the left of right item */
            }
            .timeline-item.left .timeline-content {
                padding-right: 30px;
                padding-left: 0;
            }
            .timeline-item.right .timeline-content {
                padding-left: 30px;
                padding-right: 0;
            }
        }
    </style>
</head>
<body class="antialiased">

    <div class="container mx-auto px-4 py-8 md:py-16">

        <header class="text-center mb-12 md:mb-20">
            <h1 data-lang-key="main_title" class="text-4xl md:text-5xl font-bold text-gray-800 mb-4"></h1>
            <p data-lang-key="main_subtitle" class="text-lg md:text-xl text-gray-600 max-w-3xl mx-auto"></p>
            
            <div class="mt-6">
                <label for="language-select" class="sr-only">Select Language</label>
                <select id="language-select" class="p-2 border border-gray-300 rounded-md shadow-sm text-gray-700 focus:ring-blue-500 focus:border-blue-500">
                    <option value="en">English</option>
                    <option value="de">Deutsch</option>
                    <option value="pt">Português</option>
                    <option value="zh">中文 (简体)</option>
                </select>
            </div>
        </header>

        <main>
            <!-- Section 1: The Three Stages -->
            <section id="stages" class="mb-16 md:mb-24">
                <h2 data-lang-key="stages_title" class="text-3xl font-bold text-center text-gray-800 mb-10"></h2>

                <div class="max-w-4xl mx-auto">
                    <div class="flex justify-center mb-8 border-b border-gray-200">
                        <button data-tab="digitalized" class="tab-button tab-active text-sm md:text-base font-medium py-3 px-4 md:px-6 rounded-t-lg transition-colors duration-300">
                            <span class="mr-2">⚙️</span><span data-lang-key="tab_digitalized"></span>
                        </button>
                        <button data-tab="digital" class="tab-button tab-inactive text-sm md:text-base font-medium py-3 px-4 md:px-6 rounded-t-lg transition-colors duration-300">
                             <span class="mr-2">🚀</span><span data-lang-key="tab_digital"></span>
                        </button>
                        <button data-tab="agentic" class="tab-button tab-inactive text-sm md:text-base font-medium py-3 px-4 md:px-6 rounded-t-lg transition-colors duration-300">
                             <span class="mr-2">🧠</span><span data-lang-key="tab_agentic"></span>
                        </button>
                    </div>

                    <div id="tab-content" class="bg-white p-6 md:p-10 rounded-b-lg rounded-tr-lg shadow-lg">
                        <!-- Digitalized Content -->
                        <div id="digitalized" class="tab-pane">
                            <h3 data-lang-key="digitalized_title" class="text-2xl font-bold text-gray-800 mb-2"></h3>
                            <p data-lang-key="digitalized_subtitle" class="italic text-gray-500 mb-6"></p>
                            <ul class="space-y-4 text-gray-700">
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="digitalized_focus"></div></li>
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="digitalized_tech"></div></li>
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="digitalized_action"></div></li>
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="digitalized_reactivity"></div></li>
                            </ul>
                        </div>
                        <!-- Digital Content -->
                        <div id="digital" class="tab-pane hidden">
                            <h3 data-lang-key="digital_title" class="text-2xl font-bold text-gray-800 mb-2"></h3>
                            <p data-lang-key="digital_subtitle" class="italic text-gray-500 mb-6"></p>
                            <ul class="space-y-4 text-gray-700">
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="digital_focus"></div></li>
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="digital_tech"></div></li>
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="digital_action"></div></li>
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="digitalized_reactivity"></div></li>
                            </ul>
                        </div>
                        <!-- Agentic Content -->
                        <div id="agentic" class="tab-pane hidden">
                             <h3 data-lang-key="agentic_title" class="text-2xl font-bold text-gray-800 mb-2"></h3>
                            <p data-lang-key="agentic_subtitle" class="italic text-gray-500 mb-6"></p>
                           <ul class="space-y-4 text-gray-700">
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="agentic_focus"></div></li>
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="agentic_tech"></div></li>
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="agentic_action"></div></li>
                                <li class="flex items-start"><span class="text-blue-600 mr-3 mt-1">▶</span><div data-lang-key="agentic_proactivity"></div></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </section>
            
            <!-- Section 2: The Leapfrog Opportunity -->
            <section id="leapfrog" class="bg-gray-800 text-white rounded-lg p-8 md:p-12 mb-16 md:mb-24">
                 <div class="text-center mb-10">
                    <h2 data-lang-key="leapfrog_main_title" class="text-3xl font-bold mb-3"></h2>
                    <p data-lang-key="leapfrog_main_subtitle" class="text-lg text-gray-300 max-w-3xl mx-auto"></p>
                </div>

                <div class="flex flex-col md:flex-row items-center justify-center gap-4 md:gap-8 my-8">
                    <div class="text-center p-4 bg-gray-700 rounded-lg shadow-md">
                        <span class="text-4xl">⚙️</span>
                        <h4 data-lang-key="leapfrog_stage1_title" class="font-bold mt-2"></h4>
                    </div>
                    <div class="leapfrog-arrow relative w-full md:w-auto flex items-center justify-center my-4 md:my-0">
                        <div class="arrow-body h-1 bg-gray-400 w-0 md:w-48"></div>
                        <div class="arrow-head w-4 h-4 border-t-2 border-r-2 border-gray-400 transform rotate-45 -ml-2 transition-transform duration-300"></div>
                    </div>
                    <div class="text-center p-4 bg-blue-600 rounded-lg shadow-lg scale-110">
                        <span class="text-4xl">🧠</span>
                        <h4 data-lang-key="leapfrog_stage3_title" class="font-bold mt-2 text-white"></h4>
                    </div>
                </div>

                <div class="mt-12 grid grid-cols-1 md:grid-cols-2 gap-8 max-w-5xl mx-auto">
                    <!-- Accordion 1 -->
                    <div class="accordion-item bg-gray-700 rounded-lg">
                        <button data-lang-key="leapfrog_why_possible_header" class="accordion-header w-full text-left p-5 font-bold text-lg flex justify-between items-center">
                            <span></span>
                            <span class="accordion-icon transform rotate-0 transition-transform duration-300">+</span>
                        </button>
                        <div class="accordion-content px-5">
                            <ul class="space-y-3 pb-5 text-gray-300">
                                <li data-lang-key="leapfrog_why_data"></li>
                                <li data-lang-key="leapfrog_why_tech_maturity"></li>
                                 <li data-lang-key="leapfrog_why_agile"></li>
                            </ul>
                        </div>
                    </div>
                    <!-- Accordion 2 -->
                     <div class="accordion-item bg-gray-700 rounded-lg">
                        <button data-lang-key="leapfrog_what_needed_header" class="accordion-header w-full text-left p-5 font-bold text-lg flex justify-between items-center">
                            <span></span>
                            <span class="accordion-icon transform rotate-0 transition-transform duration-300">+</span>
                        </button>
                        <div class="accordion-content px-5">
                            <ul class="space-y-3 pb-5 text-gray-300">
                                <li data-lang-key="leapfrog_what_data_quality"></li>
                                 <li data-lang-key="leapfrog_what_ai_expertise"></li>
                                 <li data-lang-key="leapfrog_what_cultural_change"></li>
                                 <li data-lang-key="leapfrog_what_strategy"></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Section: AI Evolution Timeline -->
            <section id="ai-timeline" class="mb-16 md:mb-24">
                <h2 data-lang-key="timeline_main_title" class="text-3xl font-bold text-center text-gray-800 mb-3"></h2>
                <p data-lang-key="timeline_main_subtitle" class="text-lg text-gray-600 text-center max-w-3xl mx-auto mb-10"></p>

                <div class="relative max-w-4xl mx-auto py-8">
                    <div class="timeline-line absolute left-1/2 transform -translate-x-1 hidden md:block"></div>
                    <div class="relative">
                        <!-- Timeline Item: ChatGPT Launch -->
                        <div class="timeline-item left md:left-0 md:w-1/2 md:pr-12 md:text-right">
                            <div class="timeline-content bg-white p-6 rounded-lg shadow-md mb-4">
                                <h4 data-lang-key="timeline_chatgpt_date" class="font-bold text-lg text-gray-800"></h4>
                                <p data-lang-key="timeline_chatgpt_desc" class="text-gray-700 text-sm"></p>
                                <p data-lang-key="timeline_chatgpt_relevance" class="text-blue-600 text-xs mt-2"></p>
                            </div>
                        </div>

                        <!-- Timeline Item: GPT-4 & Multimodality -->
                        <div class="timeline-item right md:left-1/2 md:w-1/2 md:pl-12 md:text-left">
                            <div class="timeline-content bg-white p-6 rounded-lg shadow-md mb-4">
                                <h4 data-lang-key="timeline_gpt4_date" class="font-bold text-lg text-gray-800"></h4>
                                <p data-lang-key="timeline_gpt4_desc" class="text-gray-700 text-sm"></p>
                                <p data-lang-key="timeline_gpt4_relevance" class="text-blue-600 text-xs mt-2"></p>
                            </div>
                        </div>

                        <!-- Timeline Item: First AI Agent Frameworks (Open Source) -->
                        <div class="timeline-item left md:left-0 md:w-1/2 md:pr-12 md:text-right">
                            <div class="timeline-content bg-white p-6 rounded-lg shadow-md mb-4">
                                <h4 data-lang-key="timeline_agent_frameworks_date" class="font-bold text-lg text-gray-800"></h4>
                                <p data-lang-key="timeline_agent_frameworks_desc" class="text-gray-700 text-sm"></p>
                                <p data-lang-key="timeline_agent_frameworks_relevance" class="text-blue-600 text-xs mt-2"></p>
                            </div>
                        </div>

                        <!-- Timeline Item: Enterprise Agent Platforms -->
                        <div class="timeline-item right md:left-1/2 md:w-1/2 md:pl-12 md:text-left">
                            <div class="timeline-content bg-white p-6 rounded-lg shadow-md mb-4">
                                <h4 data-lang-key="timeline_enterprise_platforms_date" class="font-bold text-lg text-gray-800"></h4>
                                <p data-lang-key="timeline_enterprise_platforms_desc" class="text-gray-700 text-sm"></p>
                                <p data-lang-key="timeline_enterprise_platforms_relevance" class="text-blue-600 text-xs mt-2"></p>
                            </div>
                        </div>

                        <!-- Timeline Item: Advances in Reasoning & Tool Use -->
                        <div class="timeline-item left md:left-0 md:w-1/2 md:pr-12 md:text-right">
                            <div class="timeline-content bg-white p-6 rounded-lg shadow-md mb-4">
                                <h4 data-lang-key="timeline_reasoning_tools_date" class="font-bold text-lg text-gray-800"></h4>
                                <p data-lang-key="timeline_reasoning_tools_desc" class="text-gray-700 text-sm"></p>
                                <p data-lang-key="timeline_reasoning_tools_relevance" class="text-blue-600 text-xs mt-2"></p>
                            </div>
                        </div>

                        <!-- Timeline Item: 2025-2027: Specialized AGI / "Frontier Models" (after Altman/Musk/Page) -->
                        <div class="timeline-item right md:left-1/2 md:w-1/2 md:pl-12 md:text-left">
                            <div class="timeline-content bg-white p-6 rounded-lg shadow-md mb-4">
                                <h4 data-lang-key="timeline_specialized_agi_date" class="font-bold text-lg text-gray-800"></h4>
                                <p data-lang-key="timeline_specialized_agi_desc" class="text-gray-700 text-sm"></p>
                                <p data-lang-key="timeline_specialized_agi_relevance" class="text-blue-600 text-xs mt-2"></p>
                            </div>
                        </div>

                        <!-- Timeline Item: From 2028: AGI Approximation & Superintelligence Debate (after Musk/Altman) -->
                        <div class="timeline-item left md:left-0 md:w-1/2 md:pr-12 md:text-right">
                            <div class="timeline-content bg-white p-6 rounded-lg shadow-md mb-4">
                                <h4 data-lang-key="timeline_agi_approximation_date" class="font-bold text-lg text-gray-800"></h4>
                                <p data-lang-key="timeline_agi_approximation_desc" class="text-gray-700 text-sm"></p>
                                <p data-lang-key="timeline_agi_approximation_relevance" class="text-blue-600 text-xs mt-2"></p>
                            </div>
                        </div>

                    </div>
                </div>
            </section>

            <!-- Section 3: Maturity Model Chart -->
            <section id="chart-section">
                <h2 data-lang-key="chart_title" class="text-3xl font-bold text-center text-gray-800 mb-3"></h2>
                <p data-lang-key="chart_subtitle" class="text-lg text-gray-600 text-center max-w-3xl mx-auto mb-10"></p>
                <div class="chart-container bg-white p-4 rounded-lg shadow-lg">
                    <canvas id="maturityChart"></canvas>
                </div>
            </section>

        </main>

        <footer class="text-center mt-16 md:mt-24 text-gray-500">
            <p data-lang-key="footer_text"></p>
        </footer>

    </div>

    <script>
        const translations = {
            en: {
                main_title: "The Evolution of Companies",
                main_subtitle: "From digitized processes to autonomous, agentic AI leadership – a strategic necessity and your greatest opportunity.",
                stages_title: "Three Stages of Transformation",
                tab_digitalized: "Digitized",
                tab_digital: "Digital",
                tab_agentic: "Agentic",
                digitalized_title: "Stage 1: Digitized Company",
                digitalized_subtitle: "The 'Souped-Up Analog'",
                digitalized_focus: "<strong>Focus:</strong> Analog processes are mapped 1:1 digitally. The primary goal is to increase the efficiency of existing operations.",
                digitalized_tech: "<strong>Technology:</strong> Serves as a tool for acceleration and administration. Software primarily supports human work steps.",
                digitalized_action: "<strong>Action:</strong> Online forms replace paper, digital archives replace filing cabinets. The 'what' remains the same, only the 'how' becomes faster.",
                digitalized_reactivity: "<strong>Reactivity:</strong> The business model remains highly reactive and unchanged in its logic.",
                digital_title: "Stage 2: Digital Company",
                digital_subtitle: "The 'Digital Native'",
                digital_focus: "<strong>Focus:</strong> Business models and customer experience are fundamentally rethought and developed digitally from scratch.",
                digital_tech: "<strong>Technology:</strong> Is the core of the business model and an enabler for new digital products, services, and data-driven decisions.",
                digital_action: "<strong>Action:</strong> App-first approaches, building digital ecosystems, personalized offers based on Big Data Analytics.",
                digital_reactivity: "<strong>Reactivity:</strong> Agile and quick to react, but the fundamental control remains human-centered.",
                agentic_title: "Stage 3: Agentic Company",
                agentic_subtitle: "The 'Autonomous Creator'",
                agentic_focus: "<strong>Focus:</strong> Autonomous, proactive value creation through AI agents that understand and pursue overarching goals.",
                agentic_tech: "<strong>Technology:</strong> Is an autonomous actor and the learning engine of the business. AI makes decisions and self-optimizes.",
                agentic_action: "<strong>Action:</strong> AI agents proactively identify customer needs, develop product variants, optimize supply chains, and manage operations.",
                agentic_proactivity: "<strong>Proactivity:</strong> The system is highly proactive and self-optimizing; humans become strategic overseers.",
                leapfrog_main_title: "The Strategic Opportunity: The 'Leapfrog' Jump",
                leapfrog_main_subtitle: "Digitized companies do not have to follow the linear path. They can skip the digital stage and overtake their competitors by jumping directly to agentic leadership.",
                leapfrog_stage1_title: "Stage 1: Digitized",
                leapfrog_stage3_title: "Stage 3: Agentic",
                leapfrog_why_possible_header: "Why the jump is possible",
                leapfrog_why_data: "<strong>Existing Digital Data:</strong> Digitized companies often possess large, albeit unstructured, digital data – the raw material for AI training.",
                leapfrog_why_tech_maturity: "<strong>Technological Maturity:</strong> Modern AI frameworks enable 'AI-Overlay' strategies, allowing intelligent layers to be built over existing systems without a complete rebuild.",
                leapfrog_why_agile: "<strong>Agile Incubation:</strong> Rapid testing and scaling of AI agents in pilot projects is possible without reorganizing the entire company.",
                leapfrog_what_needed_header: "What is necessary",
                leapfrog_what_data_quality: "<strong>Data Integration & Quality:</strong> Building a consolidated, clean, and accessible data foundation.",
                leapfrog_what_ai_expertise: "<strong>Specialized AI Expertise:</strong> Investment in or partnership with experts for building AI agents.",
                leapfrog_what_cultural_change: "<strong>Cultural Change & Trust:</strong> Fostering a culture of autonomy and risk-taking; building trust in AI-driven processes.",
                leapfrog_what_strategy: "<strong>Clear Strategy & Leadership:</strong> A resolute top-down commitment to agentic transformation.",
                timeline_main_title: "AI Evolution & Your Timing Advantage",
                timeline_main_subtitle: "A timeline of fundamental AI development, showing how you can leverage 'leapfrogging' for optimal timing.",
                timeline_chatgpt_date: "November 2022: ChatGPT Launch",
                timeline_chatgpt_desc: "Democratization of Large Language Models (LLMs) and beginning of widespread interest in generative AI.",
                timeline_chatgpt_relevance: "**Relevance for Agentic Leapfrog:** Enables first steps towards intelligent, dialogue-based assistants and automations.",
                timeline_gpt4_date: "March 2023: GPT-4 & Multimodality",
                timeline_gpt4_desc: "Significantly expanded capabilities in logic, creativity, and understanding various data formats (text, image).",
                timeline_gpt4_relevance: "**Relevance for Agentic Leapfrog:** Foundation for more complex, context-sensitive agents capable of solving diverse tasks.",
                timeline_agent_frameworks_date: "April/May 2023: First AI Agent Frameworks",
                timeline_agent_frameworks_desc: "Concepts like Auto-GPT and BabyAGI demonstrate the potential for autonomous, goal-oriented AI systems.",
                timeline_agent_frameworks_relevance: "**Relevance for Agentic Leapfrog:** The concept of the 'agent' becomes tangible; first experiments with autonomous processes become possible.",
                timeline_enterprise_platforms_date: "Late 2023/Early 2024: Enterprise Agent Platforms",
                timeline_enterprise_platforms_desc: "Major tech providers (e.g., Microsoft Autogen, AWS Bedrock Agents) provide tools for enterprise use.",
                timeline_enterprise_platforms_relevance: "**Relevance for Agentic Leapfrog:** Agent technology becomes more production-ready and secure for use in regulated industries.",
                timeline_reasoning_tools_date: "Ongoing (2024-2025): Reasoning & Tool Use",
                timeline_reasoning_tools_desc: "LLMs improve in logical reasoning, planning, and using external software/databases. AI agents become more complex.",
                timeline_reasoning_tools_relevance: "**Relevance for Agentic Leapfrog:** Agents can autonomously handle more complex, multi-step business processes and interact with existing systems.",
                timeline_specialized_agi_date: "2025-2027: Specialized AGI / 'Frontier Models'",
                timeline_specialized_agi_desc: "**Forecast (incl. Sam Altman, Larry Page):** AI models reach human-level performance in specific, broad cognitive domains (e.g., complex legal reasoning, scientific research). Significant advances in multimodality and real-time interaction.",
                timeline_specialized_agi_relevance: "**Relevance for Agentic Leapfrog:** Enables the creation of highly autonomous, specialized agents that can transform entire business areas. Investing now secures expertise for this next wave.",
                timeline_agi_approximation_date: "From 2028: AGI Approximation & Superintelligence Debate",
                timeline_agi_approximation_desc: "**Forecast (incl. Elon Musk, Sam Altman):** AI systems approach the ability to perform *any* intellectual task of a human. Focus on AI safety and alignment becomes globally dominant.",
                timeline_agi_approximation_relevance: "**Relevance for Agentic Leapfrog:** Early understanding and integration of agent technology is crucial to maintain control and leverage the enormous potential of this stage. Those who are too late will be overwhelmed by the development.",
                chart_title: "Maturity Model of Transformation",
                chart_subtitle: "The agentic stage delivers exponential business value and autonomy compared to technological and financial effort.",
                chart_x_axis: "Technological Maturity & Integration",
                chart_y_axis: "Business Value & Autonomy",
                chart_digitalized_summary: "Focus on increasing efficiency of existing processes.",
                chart_digital_summary: "Focus on new, digital-native business models.",
                chart_agentic_summary: "Focus on autonomous, proactive value creation through AI.",
                footer_text: "&copy; 2025 Strategic Business Development. A conceptual framework."
            },
            de: {
                main_title: "Die Evolution der Unternehmen",
                main_subtitle: "Vom digitalisierten Prozess zur autonomen, agentischen KI-Führung – eine strategische Notwendigkeit und Ihre größte Chance.",
                stages_title: "Drei Stufen der Transformation",
                tab_digitalized: "Digitalisiert",
                tab_digital: "Digital",
                tab_agentic: "Agentisch",
                digitalized_title: "Stufe 1: Digitalisiertes Unternehmen",
                digitalized_subtitle: "Das 'aufgemotzte Analoge'",
                digitalized_focus: "<strong>Fokus:</strong> Analoge Prozesse werden 1:1 digital abgebildet. Effizienzsteigerung bestehender Abläufe steht im Vordergrund.",
                digitalized_tech: "<strong>Technologie:</strong> Dient als Werkzeug zur Beschleunigung und Verwaltung. Software unterstützt primär menschliche Arbeitsschritte.",
                digitalized_action: "<strong>Aktion:</strong> Online-Formulare ersetzen Papier, digitale Ablagen ersetzen Aktenschränke. Das 'Was' bleibt gleich, nur das 'Wie' wird schneller.",
                digitalized_reactivity: "<strong>Reaktivität:</strong> Das Geschäftsmodell bleibt hoch reaktiv und in seiner Logik unverändert.",
                digital_title: "Stufe 2: Digitales Unternehmen",
                digital_subtitle: "Das 'digital Native'",
                digital_focus: "<strong>Fokus:</strong> Geschäftsmodelle und das Kundenerlebnis werden von Grund auf digital neu gedacht.",
                digital_tech: "<strong>Technologie:</strong> Ist der Kern des Geschäftsmodells und Enabler für neue, digitale Produkte, Services und datengetriebene Entscheidungen.",
                digital_action: "<strong>Aktion:</strong> App-First-Ansätze, Aufbau digitaler Ökosysteme, personalisierte Angebote basierend auf Big Data Analytics.",
                digital_reactivity: "<strong>Reaktivität:</strong> Agil und schnell in der Reaktion, aber die grundlegende Steuerung bleibt menschenzentriert.",
                agentic_title: "Stufe 3: Agentisches Unternehmen",
                agentic_subtitle: "Der 'autonome Gestalter'",
                agentic_focus: "<strong>Fokus:</strong> Autonome, proaktive Wertschöpfung durch KI-Agenten, die übergeordnete Ziele verstehen und verfolgen.",
                agentic_tech: "<strong>Technologie:</strong> Ist ein autonomer Akteur und der lernende Motor des Geschäfts. KI trifft Entscheidungen und optimiert sich selbst.",
                agentic_action: "<strong>Aktion:</strong> KI-Agenten erkennen Kundenbedürfnisse proaktiv, entwickeln Produktvarianten, optimieren Lieferketten und steuern Betriebsabläufe.",
                agentic_proactivity: "<strong>Proaktivität:</strong> Das System ist hoch proaktiv und selbstoptimierend; der Mensch wird zum strategischen Überwacher.",
                leapfrog_main_title: "Die strategische Chance: Der 'Leapfrog'-Sprung",
                leapfrog_main_subtitle: "Digitalisierte Unternehmen müssen nicht den linearen Weg gehen. Sie können die digitale Stufe überspringen und ihre Wettbewerber durch den direkten Sprung zur agentischen Führung überholen.",
                leapfrog_stage1_title: "Stufe 1: Digitalisiert",
                leapfrog_stage3_title: "Stufe 3: Agentisch",
                leapfrog_why_possible_header: "Warum der Sprung möglich ist",
                leapfrog_why_data: "<strong>Vorhandene digitale Daten:</strong> Digitalisierte Unternehmen besitzen den Rohstoff für KI-Training.",
                leapfrog_why_tech_maturity: "<strong>Technologische Reife:</strong> Moderne KI-Frameworks können als intelligente Schicht ('AI-Overlay') über bestehende Systeme gelegt werden.",
                leapfrog_why_agile: "<strong>Agile Inkubation:</strong> Schnelles Testen und Skalieren von KI-Agenten in Pilotprojekten ist möglich, ohne das gesamte Unternehmen umzubauen.",
                leapfrog_what_needed_header: "Was dafür notwendig ist",
                leapfrog_what_data_quality: "<strong>Datenintegration & -qualität:</strong> Aufbau einer konsolidierten, sauberen und zugänglichen Datenbasis.",
                leapfrog_what_ai_expertise: "<strong>Spezialisierte KI-Expertise:</strong> Investition in oder Partnerschaft mit Experten für den Aufbau von KI-Agenten.",
                leapfrog_what_cultural_change: "<strong>Kultureller Wandel & Vertrauen:</strong> Förderung von Autonomie und Risikobereitschaft; Aufbau von Vertrauen in KI-gesteuerte Prozesse.",
                leapfrog_what_strategy: "<strong>Klare Strategie & Führung:</strong> Ein entschlossenes Top-Down-Commitment zur agentischen Transformation.",
                timeline_main_title: "Die KI-Evolution & Ihr Timing-Vorteil",
                timeline_main_subtitle: "Eine Zeitlinie der grundlegenden KI-Entwicklung, die zeigt, wie Sie durch 'Leapfrogging' das richtige Timing nutzen können.",
                timeline_chatgpt_date: "November 2022: ChatGPT Launch",
                timeline_chatgpt_desc: "Demokratisierung von Large Language Models (LLMs) und Beginn des breiten Interesses an generativer KI.",
                timeline_chatgpt_relevance: "**Relevanz für Agentic Leapfrog:** Ermöglicht erste Schritte zu intelligenten, dialogbasierten Assistenten und Automatisierungen.",
                timeline_gpt4_date: "März 2023: GPT-4 & Multimodalität",
                timeline_gpt4_desc: "Deutlich erweiterte Fähigkeiten in Logik, Kreativität und Verständnis verschiedener Datenformate (Text, Bild).",
                timeline_gpt4_relevance: "**Relevanz für Agentic Leapfrog:** Basis für komplexere, kontextsensitivere Agenten, die vielfältige Aufgaben lösen können.",
                timeline_agent_frameworks_date: "April/Mai 2023: Erste KI-Agenten-Frameworks",
                timeline_agent_frameworks_desc: "Konzepte wie Auto-GPT und BabyAGI zeigen das Potenzial für autonome, zielorientierte KI-Systeme.",
                timeline_agent_frameworks_relevance: "**Relevanz für Agentic Leapfrog:** Das Konzept des 'Agenten' wird greifbar; erste Experimente mit autonomen Prozessen werden möglich.",
                timeline_enterprise_platforms_date: "Spät 2023/Früh 2024: Enterprise Agenten-Plattformen",
                timeline_enterprise_platforms_desc: "Große Tech-Anbieter (z.B. Microsoft Autogen, AWS Bedrock Agents) stellen Tools für den Unternehmenseinsatz bereit.",
                timeline_enterprise_platforms_relevance: "**Relevanz für Agentic Leapfrog:** Agenten-Technologie wird produktionsreifer und sicherer für den Einsatz in regulierten Branchen.",
                timeline_reasoning_tools_date: "Laufend (2024-2025): Reasoning & Tool Use",
                timeline_reasoning_tools_desc: "LLMs werden besser im logischen Denken, Planen und Nutzen externer Software/Datenbanken. KI-Agenten werden komplexer.",
                timeline_reasoning_tools_relevance: "**Relevanz für Agentic Leapfrog:** Agenten können komplexere, mehrstufige Geschäftsprozesse autonom abwickeln und mit bestehenden Systemen interagieren.",
                timeline_specialized_agi_date: "2025-2027: Spezialisierte AGI / 'Frontier Models'",
                timeline_specialized_agi_desc: "**Prognose (u.a. Sam Altman, Larry Page):** KI-Modelle erreichen menschliches Niveau in spezifischen, breiten kognitiven Domänen (z.B. komplexes juristisches Denken, wissenschaftliche Forschung). Deutliche Fortschritte in der Multimodalität und Echtzeit-Interaktion.",
                timeline_specialized_agi_relevance: "**Relevanz für Agentic Leapfrog:** Ermöglicht den Aufbau hochautonomer, spezialisierter Agenten, die ganze Geschäftsbereiche transformieren können. Wer jetzt investiert, sichert sich die Expertise für diese nächste Welle.",
                timeline_agi_approximation_date: "Ab 2028: AGI-Annäherung & Superintelligenz-Debatte",
                timeline_agi_approximation_desc: "**Prognose (u.a. Elon Musk, Sam Altman):** KI-Systeme nähern sich der Fähigkeit, *jede* intellektuelle Aufgabe eines Menschen zu erfüllen. Fokus auf AI-Sicherheit und Alignment wird global dominant.",
                timeline_agi_approximation_relevance: "**Relevanz für Agentic Leapfrog:** Das frühe Verständnis und die Integration von Agenten-Technologie ist entscheidend, um die Kontrolle zu behalten und die enormen Potenziale dieser Stufe zu nutzen. Wer zu spät kommt, wird von der Entwicklung überrollt.",
                chart_title: "Maturity Model of Transformation",
                chart_subtitle: "The agentic stage delivers exponential business value and autonomy compared to technological and financial effort.",
                chart_x_axis: "Technological Maturity & Integration",
                chart_y_axis: "Business Value & Autonomy",
                chart_digitalized_summary: "Focus on increasing efficiency of existing processes.",
                chart_digital_summary: "Focus on new, digital-native business models.",
                chart_agentic_summary: "Focus on autonomous, proactive value creation through AI.",
                footer_text: "&copy; 2025 Strategic Business Development. A conceptual framework."
            },
            de: {
                main_title: "Die Evolution der Unternehmen",
                main_subtitle: "Vom digitalisierten Prozess zur autonomen, agentischen KI-Führung – eine strategische Notwendigkeit und Ihre größte Chance.",
                stages_title: "Drei Stufen der Transformation",
                tab_digitalized: "Digitalisiert",
                tab_digital: "Digital",
                tab_agentic: "Agentisch",
                digitalized_title: "Stufe 1: Digitalisiertes Unternehmen",
                digitalized_subtitle: "Das 'aufgemotzte Analoge'",
                digitalized_focus: "<strong>Fokus:</strong> Analoge Prozesse werden 1:1 digital abgebildet. Effizienzsteigerung bestehender Abläufe steht im Vordergrund.",
                digitalized_tech: "<strong>Technologie:</strong> Dient als Werkzeug zur Beschleunigung und Verwaltung. Software unterstützt primär menschliche Arbeitsschritte.",
                digitalized_action: "<strong>Aktion:</strong> Online-Formulare ersetzen Papier, digitale Ablagen ersetzen Aktenschränke. Das 'Was' bleibt gleich, nur das 'Wie' wird schneller.",
                digitalized_reactivity: "<strong>Reaktivität:</strong> Das Geschäftsmodell bleibt hoch reaktiv und in seiner Logik unverändert.",
                digital_title: "Stufe 2: Digitales Unternehmen",
                digital_subtitle: "Das 'digital Native'",
                digital_focus: "<strong>Fokus:</strong> Geschäftsmodelle und das Kundenerlebnis werden von Grund auf digital neu gedacht.",
                digital_tech: "<strong>Technologie:</strong> Ist der Kern des Geschäftsmodells und Enabler für neue, digitale Produkte, Services und datengetriebene Entscheidungen.",
                digital_action: "<strong>Aktion:</strong> App-First-Ansätze, Aufbau digitaler Ökosysteme, personalisierte Angebote basierend auf Big Data Analytics.",
                digital_reactivity: "<strong>Reaktivität:</strong> Agil und schnell in der Reaktion, aber die grundlegende Steuerung bleibt menschenzentriert.",
                agentic_title: "Stufe 3: Agentisches Unternehmen",
                agentic_subtitle: "Der 'autonome Gestalter'",
                agentic_focus: "<strong>Fokus:</strong> Autonome, proaktive Wertschöpfung durch KI-Agenten, die übergeordnete Ziele verstehen und verfolgen.",
                agentic_tech: "<strong>Technologie:</strong> Ist ein autonomer Akteur und der lernende Motor des Geschäfts. KI trifft Entscheidungen und optimiert sich selbst.",
                agentic_action: "<strong>Aktion:</strong> KI-Agenten erkennen Kundenbedürfnisse proaktiv, entwickeln Produktvarianten, optimieren Lieferketten und steuern Betriebsabläufe.",
                agentic_proactivity: "<strong>Proaktivität:</strong> Das System ist hoch proaktiv und selbstoptimierend; der Mensch wird zum strategischen Überwacher.",
                leapfrog_main_title: "Die strategische Chance: Der 'Leapfrog'-Sprung",
                leapfrog_main_subtitle: "Digitalisierte Unternehmen müssen nicht den linearen Weg gehen. Sie können die digitale Stufe überspringen und ihre Wettbewerber durch den direkten Sprung zur agentischen Führung überholen.",
                leapfrog_stage1_title: "Stufe 1: Digitalisiert",
                leapfrog_stage3_title: "Stufe 3: Agentisch",
                leapfrog_why_possible_header: "Warum der Sprung möglich ist",
                leapfrog_why_data: "<strong>Vorhandene digitale Daten:</strong> Digitalisierte Unternehmen besitzen den Rohstoff für KI-Training.",
                leapfrog_why_tech_maturity: "<strong>Technologische Reife:</strong> Moderne KI-Frameworks können als intelligente Schicht ('AI-Overlay') über bestehende Systeme gelegt werden.",
                leapfrog_why_agile: "<strong>Agile Inkubation:</strong> Schnelles Testen und Skalieren von KI-Agenten in Pilotprojekten ist möglich, ohne das gesamte Unternehmen umzubauen.",
                leapfrog_what_needed_header: "Was dafür notwendig ist",
                leapfrog_what_data_quality: "<strong>Datenintegration & -qualität:</strong> Aufbau einer konsolidierten, sauberen und zugänglichen Datenbasis.",
                leapfrog_what_ai_expertise: "<strong>Spezialisierte KI-Expertise:</strong> Investition in oder Partnerschaft mit Experten für den Aufbau von KI-Agenten.",
                leapfrog_what_cultural_change: "<strong>Kultureller Wandel & Vertrauen:</strong> Förderung von Autonomie und Risikobereitschaft; Aufbau von Vertrauen in KI-gesteuerte Prozesse.",
                leapfrog_what_strategy: "<strong>Klare Strategie & Führung:</strong> Ein entschlossenes Top-Down-Commitment zur agentischen Transformation.",
                timeline_main_title: "Die KI-Evolution & Ihr Timing-Vorteil",
                timeline_main_subtitle: "Eine Zeitlinie der grundlegenden KI-Entwicklung, die zeigt, wie Sie durch 'Leapfrogging' das richtige Timing nutzen können.",
                timeline_chatgpt_date: "November 2022: ChatGPT Launch",
                timeline_chatgpt_desc: "Demokratisierung von Large Language Models (LLMs) und Beginn des breiten Interesses an generativer KI.",
                timeline_chatgpt_relevance: "**Relevanz für Agentic Leapfrog:** Ermöglicht erste Schritte zu intelligenten, dialogbasierten Assistenten und Automatisierungen.",
                timeline_gpt4_date: "März 2023: GPT-4 & Multimodalität",
                timeline_gpt4_desc: "Deutlich erweiterte Fähigkeiten in Logik, Kreativität und Verständnis verschiedener Datenformate (Text, Bild).",
                timeline_gpt4_relevance: "**Relevanz für Agentic Leapfrog:** Basis für komplexere, kontextsensitivere Agenten, die vielfältige Aufgaben lösen können.",
                timeline_agent_frameworks_date: "April/Mai 2023: Erste KI-Agenten-Frameworks",
                timeline_agent_frameworks_desc: "Konzepte wie Auto-GPT und BabyAGI zeigen das Potenzial für autonome, zielorientierte KI-Systeme.",
                timeline_agent_frameworks_relevance: "**Relevanz für Agentic Leapfrog:** Das Konzept des 'Agenten' wird greifbar; erste Experimente mit autonomen Prozessen werden möglich.",
                timeline_enterprise_platforms_date: "Spät 2023/Früh 2024: Enterprise Agenten-Plattformen",
                timeline_enterprise_platforms_desc: "Große Tech-Anbieter (z.B. Microsoft Autogen, AWS Bedrock Agents) stellen Tools für den Unternehmenseinsatz bereit.",
                timeline_enterprise_platforms_relevance: "**Relevanz für Agentic Leapfrog:** Agenten-Technologie wird produktionsreifer und sicherer für den Einsatz in regulierten Branchen.",
                timeline_reasoning_tools_date: "Laufend (2024-2025): Reasoning & Tool Use",
                timeline_reasoning_tools_desc: "LLMs werden besser im logischen Denken, Planen und Nutzen externer Software/Datenbanken. KI-Agenten werden komplexer.",
                timeline_reasoning_tools_relevance: "**Relevanz für Agentic Leapfrog:** Agenten können komplexere, mehrstufige Geschäftsprozesse autonom abwickeln und mit bestehenden Systemen interagieren.",
                timeline_specialized_agi_date: "2025-2027: Spezialisierte AGI / 'Frontier Models'",
                timeline_specialized_agi_desc: "**Prognose (u.a. Sam Altman, Larry Page):** KI-Modelle erreichen menschliches Niveau in spezifischen, breiten kognitiven Domänen (z.B. komplexes juristisches Denken, wissenschaftliche Forschung). Deutliche Fortschritte in der Multimodalität und Echtzeit-Interaktion.",
                timeline_specialized_agi_relevance: "**Relevanz für Agentic Leapfrog:** Ermöglicht den Aufbau hochautonomer, spezialisierter Agenten, die ganze Geschäftsbereiche transformieren können. Wer jetzt investiert, sichert sich die Expertise für diese nächste Welle.",
                timeline_agi_approximation_date: "Ab 2028: AGI-Annäherung & Superintelligenz-Debatte",
                timeline_agi_approximation_desc: "**Prognose (u.a. Elon Musk, Sam Altman):** KI-Systeme nähern sich der Fähigkeit, *jede* intellektuelle Aufgabe eines Menschen zu erfüllen. Fokus auf AI-Sicherheit und Alignment wird global dominant.",
                timeline_agi_approximation_relevance: "**Relevanz für Agentic Leapfrog:** Das frühe Verständnis und die Integration von Agenten-Technologie ist entscheidend, um die Kontrolle zu behalten und die enormen Potenziale dieser Stufe zu nutzen. Wer zu spät kommt, wird von der Entwicklung überrollt.",
                chart_title: "Maturity Model of Transformation",
                chart_subtitle: "The agentic stage delivers exponential business value and autonomy compared to technological and financial effort.",
                chart_x_axis: "Technological Maturity & Integration",
                chart_y_axis: "Business Value & Autonomy",
                chart_digitalized_summary: "Focus on increasing efficiency of existing processes.",
                chart_digital_summary: "Focus on new, digital-native business models.",
                chart_agentic_summary: "Focus on autonomous, proactive value creation through AI.",
                footer_text: "&copy; 2025 Strategic Business Development. A conceptual framework."
            },
            pt: {
                main_title: "A Evolução das Empresas",
                main_subtitle: "Desde processos digitalizados à liderança autónoma e agêntica de IA – uma necessidade estratégica e a sua maior oportunidade.",
                stages_title: "Três Fases de Transformação",
                tab_digitalized: "Digitalizada",
                tab_digital: "Digital",
                tab_agentic: "Agêntica",
                digitalized_title: "Fase 1: Empresa Digitalizada",
                digitalized_subtitle: "A 'Analógica Melhorada'",
                digitalized_focus: "<strong>Foco:</strong> Processos analógicos são mapeados 1:1 digitalmente. O objetivo principal é aumentar a eficiência das operações existentes.",
                digitalized_tech: "<strong>Tecnologia:</strong> Serve como ferramenta de aceleração e administração. O software apoia principalmente as etapas de trabalho humano.",
                digitalized_action: "<strong>Ação:</strong> Formulários online substituem papel, arquivos digitais substituem armários de arquivo. O 'o quê' permanece o mesmo, apenas o 'como' se torna mais rápido.",
                digitalized_reactivity: "<strong>Reatividade:</strong> O modelo de negócio permanece altamente reativo e inalterado na sua lógica.",
                digital_title: "Fase 2: Empresa Digital",
                digital_subtitle: "O 'Nativo Digital'",
                digital_focus: "<strong>Foco:</strong> Modelos de negócio e experiência do cliente são fundamentalmente repensados e desenvolvidos digitalmente desde o início.",
                digital_tech: "<strong>Tecnologia:</strong> É o núcleo do modelo de negócio e um facilitador para novos produtos digitais, serviços e decisões baseadas em dados.",
                digital_action: "<strong>Ação:</strong> Abordagens 'app-first', construção de ecossistemas digitais, ofertas personalizadas baseadas em Big Data Analytics.",
                digital_reactivity: "<strong>Reatividade:</strong> Ágil e rápido a reagir, mas o controlo fundamental permanece centrado no ser humano.",
                agentic_title: "Fase 3: Empresa Agêntica",
                agentic_subtitle: "O 'Criador Autónomo'",
                agentic_focus: "<strong>Foco:</strong> Criação de valor autónoma e proativa através de agentes de IA que compreendem e perseguem objetivos abrangentes.",
                agentic_tech: "<strong>Tecnologia:</strong> É um ator autónomo e o motor de aprendizagem do negócio. A IA toma decisões e otimiza-se.",
                agentic_action: "<strong>Ação:</strong> Agentes de IA identificam proativamente as necessidades do cliente, desenvolvem variantes de produtos, otimizam cadeias de suprimentos e gerem operações.",
                agentic_proactivity: "<strong>Proatividade:</strong> O sistema é altamente proativo e auto-otimizador; os humanos tornam-se supervisores estratégicos.",
                leapfrog_main_title: "A Oportunidade Estratégica: O Salto 'Leapfrog'",
                leapfrog_main_subtitle: "Empresas digitalizadas não precisam seguir o caminho linear. Elas podem saltar a fase digital e ultrapassar seus concorrentes saltando diretamente para a liderança agêntica.",
                leapfrog_stage1_title: "Fase 1: Digitalizada",
                leapfrog_stage3_title: "Fase 3: Agêntica",
                leapfrog_why_possible_header: "Por que o salto é possível",
                leapfrog_why_data: "<strong>Dados Digitais Existentes:</strong> Empresas digitalizadas frequentemente possuem grandes volumes de dados digitais, embora não estruturados – a matéria-prima para o treinamento de IA.",
                leapfrog_why_tech_maturity: "<strong>Maturidade Tecnológica:</strong> Estruturas modernas de IA permitem estratégias de 'AI-Overlay', permitindo que camadas inteligentes sejam construídas sobre sistemas existentes sem uma reconstrução completa.",
                leapfrog_why_agile: "<strong>Incubação Ágil:</strong> Testes e escalonamento rápidos de agentes de IA em projetos piloto são possíveis sem reorganizar toda a empresa.",
                leapfrog_what_needed_header: "O que é necessário",
                leapfrog_what_data_quality: "<strong>Integração e Qualidade de Dados:</strong> Construção de uma base de dados consolidada, limpa e acessível.",
                leapfrog_what_ai_expertise: "<strong>Experiência Especializada em IA:</strong> Investimento ou parceria com especialistas para a construção de agentes de IA.",
                leapfrog_what_cultural_change: "<strong>Mudança Cultural e Confiança:</strong> Fomento de uma cultura de autonomia e assunção de riscos; construção de confiança em processos impulsionados por IA.",
                leapfrog_what_strategy: "<strong>Estratégia e Liderança Claras:</strong> Um compromisso top-down resoluto com a transformação agêntica.",
                timeline_main_title: "Evolução da IA e Sua Vantagem de Tempo",
                timeline_main_subtitle: "Uma linha do tempo do desenvolvimento fundamental da IA, mostrando como você pode aproveitar o 'salto' para um timing ideal.",
                timeline_chatgpt_date: "Novembro de 2022: Lançamento do ChatGPT",
                timeline_chatgpt_desc: "Democratização de Grandes Modelos de Linguagem (LLMs) e início do interesse generalizado em IA generativa.",
                timeline_chatgpt_relevance: "**Relevância para o Salto Agêntico:** Permite os primeiros passos em direção a assistentes inteligentes baseados em diálogo e automações.",
                timeline_gpt4_date: "Março de 2023: GPT-4 e Multimodalidade",
                timeline_gpt4_desc: "Capacidades significativamente expandidas em lógica, criatividade e compreensão de vários formatos de dados (text, image).",
                timeline_gpt4_relevance: "**Relevância para o Salto Agêntico:** Base para agentes mais complexos e sensíveis ao contexto, capazes de resolver diversas tarefas.",
                timeline_agent_frameworks_date: "Abril/Maio de 2023: Primeiros Frameworks de Agentes de IA",
                timeline_agent_frameworks_desc: "Conceitos como Auto-GPT e BabyAGI demonstram o potencial para sistemas de IA autónomos e orientados a objetivos.",
                timeline_agent_frameworks_relevance: "**Relevância para o Salto Agêntico:** O conceito de 'agente' torna-se tangível; as primeiras experiências com processos autónomos tornam-se possíveis.",
                timeline_enterprise_platforms_date: "Final de 2023/Início de 2024: Plataformas de Agentes Empresariais",
                timeline_enterprise_platforms_desc: "Grandes fornecedores de tecnologia (por exemplo, Microsoft Autogen, AWS Bedrock Agents) fornecem ferramentas para uso empresarial.",
                timeline_enterprise_platforms_relevance: "**Relevância para o Salto Agêntico:** A tecnologia de agentes torna-se mais pronta para produção e segura para uso em indústrias regulamentadas.",
                timeline_reasoning_tools_date: "Em Andamento (2024-2025): Raciocínio e Uso de Ferramentas",
                timeline_reasoning_tools_desc: "LLMs melhoram no raciocínio lógico, planeamento e uso de software/bancos de dados externos. Agentes de IA tornam-se mais complexos.",
                timeline_reasoning_tools_relevance: "**Relevância para o Salto Agêntico:** Agentes podem lidar autonomamente com processos de negócio mais complexos e de várias etapas e interagir com sistemas existentes.",
                timeline_specialized_agi_date: "2025-2027: AGI Especializada / 'Modelos de Fronteira'",
                timeline_specialized_agi_desc: "**Previsão (incl. Sam Altman, Larry Page):** Modelos de IA atingem o desempenho de nível humano em domínios cognitivos específicos e amplos (por exemplo, raciocínio jurídico complexo, pesquisa científica). Avanços significativos em multimodalidade e interação em tempo real.",
                timeline_specialized_agi_relevance: "**Relevância para o Salto Agêntico:** Permite a criação de agentes especializados altamente autónomos que podem transformar áreas de negócio inteiras. Investir agora garante expertise para esta próxima onda.",
                timeline_agi_approximation_date: "A partir de 2028: Aproximação da AGI e Debate sobre Superinteligência",
                timeline_agi_approximation_desc: "**Previsão (incl. Elon Musk, Sam Altman):** Sistemas de IA aproximam-se da capacidade de realizar *qualquer* tarefa intelectual de um ser humano. O foco na segurança e alinhamento da IA torna-se globalmente dominante.",
                timeline_agi_approximation_relevance: "**Relevância para o Salto Agêntico:** A compreensão e integração precoce da tecnologia de agentes é crucial para manter o controlo e aproveitar o enorme potencial desta fase. Aqueles que chegam tarde serão sobrecarregados pelo desenvolvimento.",
                chart_title: "Modelo de Maturidade da Transformação",
                chart_subtitle: "A fase agêntica oferece valor de negócio exponencial e autonomia em comparação com o esforço tecnológico e financeiro.",
                chart_x_axis: "Maturidade Tecnológica e Integração",
                chart_y_axis: "Valor de Negócio e Autonomia",
                chart_digitalized_summary: "Foco no aumento da eficiência dos processos existentes.",
                chart_digital_summary: "Foco em novos modelos de negócios nativos digitais.",
                chart_agentic_summary: "Foco na criação de valor autónoma e proativa através da IA.",
                footer_text: "&copy; 2025 Desenvolvimento Estratégico de Negócios. Um quadro conceptual."
            },
            zh: {
                main_title: "企业演进：从数字化到代理式人工智能领导力",
                main_subtitle: "从数字化流程到自主代理式人工智能领导力——一项战略必要性，也是您最大的机遇。",
                stages_title: "转型的三个阶段",
                tab_digitalized: "数字化",
                tab_digital: "数字",
                tab_agentic: "代理式",
                digitalized_title: "阶段1：数字化企业",
                digitalized_subtitle: "“升级版模拟”",
                digitalized_focus: "<strong>焦点：</strong>模拟流程1:1数字化。主要目标是提高现有操作的效率。",
                digitalized_tech: "<strong>技术：</strong>作为加速和管理的工具。软件主要支持人工操作步骤。",
                digitalized_action: "<strong>行动：</strong>在线表格取代纸质表格，数字档案取代文件柜。‘做什么’保持不变，只是‘怎么做’变得更快。",
                digitalized_reactivity: "<strong>反应性：</strong>业务模式保持高度反应性，其逻辑不变。",
                digital_title: "阶段2：数字企业",
                digital_subtitle: "“数字原生”",
                digital_focus: "<strong>焦点：</strong>业务模式和客户体验从根本上被重新构思和数字化开发。",
                digital_tech: "<strong>技术：：</strong>是业务模式的核心，也是新数字产品、服务和数据驱动决策的推动者。",
                digital_action: "<strong>行动：</strong>‘应用优先’方法，构建数字生态系统，基于大数据分析的个性化产品。",
                digital_reactivity: "<strong>反应性：</strong>敏捷且反应迅速，但基本控制仍以人为中心。",
                agentic_title: "阶段3：代理式企业",
                agentic_subtitle: "“自主创造者”",
                agentic_focus: "<strong>焦点：：</strong>通过理解并追求总体目标的AI代理实现自主、主动的价值创造。",
                agentic_tech: "<strong>技术：</strong>是业务的自主行动者和学习引擎。AI做出决策并自我优化。",
                agentic_action: "<strong>行动：</strong>AI代理主动识别客户需求，开发产品变体，优化供应链，并管理运营。",
                agentic_proactivity: "<strong>主动性：：</strong>系统高度主动并自我优化；人类成为战略监督者。",
                leapfrog_main_title: "战略机遇：“蛙跳式”飞跃",
                leapfrog_main_subtitle: "数字化企业不必遵循线性路径。它们可以通过直接跳到代理式领导地位来跳过数字阶段并超越竞争对手。",
                leapfrog_stage1_title: "阶段1：数字化",
                leapfrog_stage3_title: "阶段3：代理式",
                leapfrog_why_possible_header: "为什么跳跃是可能的",
                leapfrog_why_data: "<strong>现有数字数据：</strong>数字化企业通常拥有大量（尽管非结构化）数字数据——AI训练的原材料。",
                leapfrog_why_tech_maturity: "<strong>技术成熟度：</strong>现代AI框架支持“AI叠加”策略，允许在现有系统之上构建智能层，而无需完全重建。",
                leapfrog_why_agile: "<strong>敏捷孵化：</strong>在试点项目中快速测试和扩展AI代理是可能的，而无需重组整个公司。",
                leapfrog_what_needed_header: "什么是必要的",
                leapfrog_what_data_quality: "<strong>数据集成与质量：</strong>建立一个统一、干净且可访问的数据基础。",
                leapfrog_what_ai_expertise: "<strong>专业AI专长：：</strong>投资或与AI代理构建专家合作。",
                leapfrog_what_cultural_change: "<strong>文化变革与信任：</strong>培养自主性和承担风险的文化；建立对AI驱动流程的信任。",
                leapfrog_what_strategy: "<strong>清晰的战略与领导力：</strong>对代理式转型的坚定自上而下的承诺。",
                timeline_main_title: "AI演进与您的时机优势",
                timeline_main_subtitle: "AI基础发展的时间线，展示了如何利用“蛙跳式”实现最佳时机。",
                timeline_chatgpt_date: "2022年11月：ChatGPT发布",
                timeline_chatgpt_desc: "大型语言模型（LLMs）的民主化，以及生成式AI广泛兴趣的开始。",
                timeline_chatgpt_relevance: "**代理式蛙跳的相关性：**为智能、基于对话的助手和自动化迈出第一步。",
                timeline_gpt4_date: "2023年3月：GPT-4与多模态",
                timeline_gpt4_desc: "在逻辑、创造力和理解各种数据格式（文本、图像）方面显著扩展了能力。",
                timeline_gpt4_relevance: "**代理式蛙跳的相关性：**更复杂、上下文敏感的代理的基础，能够解决多样化任务。",
                timeline_agent_frameworks_date: "2023年4月/5月：首批AI代理框架",
                timeline_agent_frameworks_desc: "Auto-GPT和BabyAGI等概念展示了自主、目标导向AI系统的潜力。",
                timeline_agent_frameworks_relevance: "**代理式蛙跳的相关性：**“代理”的概念变得具体；自主流程的首次实验成为可能。",
                timeline_enterprise_platforms_date: "2023年末/2024年初：企业代理平台",
                timeline_enterprise_platforms_desc: "主要技术提供商（例如，Microsoft Autogen，AWS Bedrock Agents）提供企业级工具。",
                timeline_enterprise_platforms_relevance: "**代理式蛙跳的相关性：**代理技术变得更具生产就绪性，在受监管行业中更安全。",
                timeline_reasoning_tools_date: "持续进行中（2024-2025）：推理与工具使用",
                timeline_reasoning_tools_desc: "LLMs在逻辑推理、规划和使用外部软件/数据库方面得到改进。AI代理变得更加复杂。",
                timeline_reasoning_tools_relevance: "**代理式蛙跳的相关性：**代理可以自主处理更复杂、多步骤的业务流程，并与现有系统交互。",
                timeline_specialized_agi_date: "2025-2027年：专业化AGI / “前沿模型”",
                timeline_specialized_agi_desc: "**预测（包括Sam Altman, Larry Page）：**AI模型在特定、广泛的认知领域（例如，复杂的法律推理、科学研究）达到人类水平的性能。多模态和实时交互方面取得显著进展。",
                timeline_specialized_agi_relevance: "**代理式蛙跳的相关性：**能够创建高度自主的专业代理，可以改变整个业务领域。现在投资可确保下一波浪潮的专业知识。",
                timeline_agi_approximation_date: "2028年起：AGI逼近与超级智能辩论",
                timeline_agi_approximation_desc: "**预测（包括Elon Musk, Sam Altman）：**AI系统接近执行人类*任何*智力任务的能力。AI安全和对齐的焦点在全球范围内变得主导。",
                timeline_agi_approximation_relevance: "**代理式蛙跳的相关性：**早期理解和整合代理技术对于保持控制和利用这一阶段的巨大潜力至关重要。迟到者将被发展所淹没。",
                chart_title: "转型成熟度模型",
                chart_subtitle: "代理式阶段提供指数级的业务价值和自主性，与技术和财务投入相比。",
                chart_x_axis: "技术成熟度与集成",
                chart_y_axis: "业务价值与自主性",
                chart_digitalized_summary: "专注于提高现有流程的效率。",
                chart_digital_summary: "专注于新的、数字原生的商业模式。",
                chart_agentic_summary: "专注于通过AI实现自主、主动的价值创造。",
                footer_text: "&copy; 2025 战略业务发展。一个概念框架。"
            }
        };

        let currentLang = localStorage.getItem('selectedLang') || 'en'; // Default to English or saved preference

        function applyLanguage(lang) {
            document.querySelectorAll('[data-lang-key]').forEach(element => {
                const key = element.dataset.langKey;
                if (translations[lang] && translations[lang][key]) {
                    element.innerHTML = translations[lang][key];
                }
            });

            // Update chart axis titles and tooltip callbacks
            // Only attempt to update chart if it has been initialized
            if (window.maturityChart) {
                window.maturityChart.options.scales.x.title.text = translations[lang].chart_x_axis;
                window.maturityChart.options.scales.y.title.text = translations[lang].chart_y_axis;
                window.maturityChart.options.plugins.tooltip.callbacks.label = function(context) {
                    let label = context.dataset.label || '';
                    let summary = '';
                    // Use the fixed dataset label to get the correct translated summary
                    switch(context.dataset.label) {
                        case 'Digitalisiert':
                            summary = translations[lang].chart_digitalized_summary;
                            break;
                        case 'Digital':
                            summary = translations[lang].chart_digital_summary;
                            break;
                        case 'Agentisch':
                            summary = translations[lang].chart_agentic_summary;
                            break;
                        // For English dataset labels, if the chart labels themselves are not translated
                        case 'Digitized': // This label comes from the dataset, not data-lang-key
                            summary = translations[lang].chart_digitalized_summary;
                            break;
                        case 'Agentic': // This label comes from the dataset, not data-lang-key
                            summary = translations[lang].chart_agentic_summary;
                            break;
                    }
                    return summary;
                };
                window.maturityChart.update();
            }

            // Update the HTML lang attribute for accessibility
            document.documentElement.lang = lang;
        }

        document.addEventListener('DOMContentLoaded', function () {
            // Chart.js functionality - Initialize the chart FIRST
            const ctx = document.getElementById('maturityChart').getContext('2d');
            window.maturityChart = new Chart(ctx, { // Assign to window for global access
                type: 'scatter',
                data: {
                    datasets: [{
                        label: 'Digitalisiert', // This label is fixed in the dataset, its summary is translated
                        data: [{x: 2, y: 2}],
                        backgroundColor: 'rgba(239, 68, 68, 0.8)',
                        pointRadius: 15,
                        pointHoverRadius: 20,
                    }, {
                        label: 'Digital', // This label is fixed in the dataset, its summary is translated
                        data: [{x: 5, y: 6}],
                        backgroundColor: 'rgba(59, 130, 246, 0.8)',
                         pointRadius: 15,
                        pointHoverRadius: 20,
                    }, {
                        label: 'Agentisch', // This label is fixed in the dataset, its summary is translated
                        data: [{x: 8, y: 9}],
                        backgroundColor: 'rgba(22, 163, 74, 0.8)',
                        pointRadius: 15,
                        pointHoverRadius: 20,
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            type: 'linear',
                            position: 'bottom',
                            title: {
                                display: true,
                                // Use currentLang for initial chart axis title
                                text: translations[currentLang].chart_x_axis,
                                font: {
                                    size: 14,
                                    weight: 'bold'
                                }
                            },
                            min: 0,
                            max: 10
                        },
                        y: {
                            title: {
                                display: true,
                                // Use currentLang for initial chart axis title
                                text: translations[currentLang].chart_y_axis,
                                font: {
                                    size: 14,
                                    weight: 'bold'
                                }
                            },
                             min: 0,
                            max: 10
                        }
                    },
                    plugins: {
                        legend: {
                            position: 'top',
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    let summary = '';
                                    // Use the fixed dataset label to get the correct translated summary
                                    switch(context.dataset.label) {
                                        case 'Digitalisiert':
                                            summary = translations[currentLang].chart_digitalized_summary;
                                            break;
                                        case 'Digital':
                                            summary = translations[currentLang].chart_digital_summary;
                                            break;
                                        case 'Agentisch':
                                            summary = translations[currentLang].chart_agentic_summary;
                                            break;
                                    }
                                    return summary;
                                }
                            },
                            bodyFont: {
                                size: 12
                            },
                            padding: 10,
                            backgroundColor: 'rgba(31, 41, 55, 0.9)'
                        }
                    }
                }
            });

            // Now apply the language to all elements, including the chart's initial state
            document.getElementById('language-select').value = currentLang;
            applyLanguage(currentLang);

            // Language selector functionality
            document.getElementById('language-select').addEventListener('change', function() {
                currentLang = this.value;
                localStorage.setItem('selectedLang', currentLang); // Save preference
                applyLanguage(currentLang);
            });

            // Tab functionality
            const tabButtons = document.querySelectorAll('.tab-button');
            const tabPanes = document.querySelectorAll('.tab-pane');

            tabButtons.forEach(button => {
                button.addEventListener('click', () => {
                    const tab = button.dataset.tab;

                    tabButtons.forEach(btn => {
                        btn.classList.remove('tab-active');
                        btn.classList.add('tab-inactive');
                    });
                    button.classList.add('tab-active');
                    button.classList.remove('tab-inactive');

                    tabPanes.forEach(pane => {
                        if (pane.id === tab) {
                            pane.classList.remove('hidden');
                        } else {
                            pane.classList.add('hidden');
                        }
                    });
                });
            });
            
            // Accordion functionality
            const accordionHeaders = document.querySelectorAll('.accordion-header');
            accordionHeaders.forEach(header => {
                header.addEventListener('click', () => {
                    const content = header.nextElementSibling;
                    const icon = header.querySelector('.accordion-icon');
                    
                    if (content.style.maxHeight) {
                        content.style.maxHeight = null;
                        icon.style.transform = 'rotate(0deg)';
                    } else {
                        // Close other accordions
                        document.querySelectorAll('.accordion-content').forEach(c => c.style.maxHeight = null);
                        document.querySelectorAll('.accordion-icon').forEach(i => i.style.transform = 'rotate(0deg)');
                        
                        content.style.maxHeight = content.scrollHeight + "px";
                        icon.style.transform = 'rotate(45deg)';
                    }
                });
            });
        });
    </script>
</body>
</html>
