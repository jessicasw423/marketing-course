# marketing-course
Basic Marketing Course for begginners
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Marketing Course: From Strategy to Evaluation</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Earthy Neutrals -->
    <!-- Application Structure Plan: The SPA is structured into three main, navigable sections corresponding to the course modules: Strategy, Implementation, and Evaluation. This tab-like structure provides clear, high-level navigation and breaks the content into digestible parts, which is superior to a long, linear scroll for an educational curriculum. Within each module, key topics are presented as interactive cards that expand on click to reveal details. This 'drill-down' interaction encourages user engagement and prevents information overload. This design transforms a static outline into an interactive learning path, enhancing usability and comprehension. -->
    <!-- Visualization & Content Choices:
        - Module Navigation (HTML/JS): Standard tab buttons for intuitive, top-level organization. Goal: Organize.
        - Key Concepts (HTML/Tailwind): Topics are interactive cards. Frameworks like the 4Ps/7Ps are visualized using structured HTML grids instead of static images. Goal: Inform, Organize. Interaction: Click to expand. Justification: Enhances engagement and breaks down complex information.
        - KPI Data (Chart.js/Canvas): The long list of KPIs in Module 3 is presented as a filterable bar chart. Goal: Compare, Inform. Interaction: Buttons filter the chart by KPI category (e.g., SEO, Social Media). Justification: This makes a dense list of data interactive and easier to digest than a static list, allowing users to focus on specific areas of interest. Library: Chart.js.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body { font-family: 'Inter', sans-serif; }
        .nav-active {
            background-color: #84a98c;
            color: #ffffff;
        }
        .nav-inactive {
            background-color: #cad2c5;
            color: #354f52;
        }
        .topic-card-details {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.5s ease-out;
        }
        .chart-container {
            position: relative;
            width: 100%;
            height: 400px;
            max-height: 50vh;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 500px;
            }
        }
        .prose ul li {
            margin-bottom: 0.5rem;
        }
        .prose h4 {
            margin-top: 1.5rem;
            margin-bottom: 0.5rem;
            font-size: 1.125rem; /* text-lg */
        }
        .llm-output {
            background-color: #e2e8f0; /* bg-blue-100 */
            padding: 1rem;
            border-radius: 0.5rem;
            margin-top: 1rem;
            white-space: pre-wrap; /* Preserve whitespace and line breaks */
            font-family: monospace;
            overflow-x: auto;
            max-height: 400px; /* Limit height for long outputs */
        }
        .submit-button {
            display: inline-block;
            padding: 10px 20px;
            background-color: #3498db;
            color: white;
            border-radius: 5px;
            font-size: 1em;
            cursor: pointer;
            transition: background-color 0.3s ease;
        }
        .submit-button:hover {
            background-color: #2980b9;
        }
        .loading-spinner {
            border: 4px solid #f3f3f3; /* Light grey */
            border-top: 4px solid #3498db; /* Blue */
            border-radius: 50%;
            width: 20px;
            height: 20px;
            animation: spin 1s linear infinite;
            display: inline-block;
            margin-left: 10px;
            vertical-align: middle;
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        .exam-question {
            margin-bottom: 20px;
            padding: 15px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #f9f9f9;
        }
        .exam-question p {
            font-weight: bold;
            margin-bottom: 10px;
        }
        .exam-question label {
            display: block;
            margin-bottom: 8px;
            cursor: pointer;
        }
        .exam-question input[type="radio"] {
            margin-right: 8px;
        }
        .exam-submit-button {
            display: block;
            width: 200px;
            padding: 12px 20px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            font-size: 1.1em;
            cursor: pointer;
            margin-top: 30px;
            transition: background-color 0.3s ease;
        }
        .exam-submit-button:hover {
            background-color: #218838;
        }
    </style>
</head>
<body class="bg-[#f5f3f1]">

    <div class="container mx-auto p-4 md:p-8 max-w-7xl">
        <header class="text-center mb-8">
            <h1 class="text-4xl md:text-5xl font-bold text-[#354f52]">Marketing Course</h1>
            <p class="text-lg text-[#52796f] mt-2">From Strategy to Evaluation</p>
        </header>

        <nav class="flex justify-center mb-8 rounded-lg shadow-md p-2 bg-white/50 backdrop-blur-sm sticky top-2 z-10">
            <button id="nav-strategy" class="nav-btn nav-active flex-1 md:flex-none text-sm md:text-base font-semibold py-2 px-4 md:px-6 rounded-md transition-colors duration-300">Strategy</button>
            <button id="nav-implementation" class="nav-btn nav-inactive flex-1 md:flex-none text-sm md:text-base font-semibold py-2 px-4 md:px-6 rounded-md transition-colors duration-300">Implementation</button>
            <button id="nav-evaluation" class="nav-btn nav-inactive flex-1 md:flex-none text-sm md:text-base font-semibold py-2 px-4 md:px-6 rounded-md transition-colors duration-300">Evaluation</button>
            <button id="nav-exam" class="nav-btn nav-inactive flex-1 md:flex-none text-sm md:text-base font-semibold py-2 px-4 md:px-6 rounded-md transition-colors duration-300">Exam</button>
            <button id="nav-tools" class="nav-btn nav-inactive flex-1 md:flex-none text-sm md:text-base font-semibold py-2 px-4 md:px-6 rounded-md transition-colors duration-300">Tools</button>
        </nav>

        <main id="content-area">
            
            <section id="module-strategy" class="module-content">
                <div class="bg-white p-6 rounded-xl shadow-lg mb-6">
                    <h2 class="text-3xl font-bold text-[#354f52] mb-2">Module 1: Strategic Marketing Foundations</h2>
                    <p class="text-md text-gray-600">This first module lays the essential groundwork for all marketing activities. Here, you will learn the core principles of strategic marketing, from conducting thorough market analysis and identifying your ideal customers to crafting a powerful value proposition that sets you apart from the competition. Mastering these fundamentals is the key to building effective and sustainable marketing efforts.</p>
                </div>
                <div id="strategy-topics" class="space-y-4"></div>
            </section>
            
            <section id="module-implementation" class="module-content hidden">
                 <div class="bg-white p-6 rounded-xl shadow-lg mb-6">
                    <h2 class="text-3xl font-bold text-[#354f52] mb-2">Module 2: Marketing Implementation</h2>
                    <p class="text-md text-gray-600">With a solid strategy in place, this module focuses on execution. You will learn how to translate your strategic plans into actionable campaigns across a variety of digital channels. We will explore content marketing, SEO, social media, and email marketing, focusing on how to create integrated communications that deliver a consistent message and a seamless customer experience, ultimately driving engagement and action.</p>
                </div>
                <div id="implementation-topics" class="space-y-4"></div>
            </section>

            <section id="module-evaluation" class="module-content hidden">
                 <div class="bg-white p-6 rounded-xl shadow-lg mb-6">
                    <h2 class="text-3xl font-bold text-[#354f52] mb-2">Module 3: Marketing Evaluation and KPIs</h2>
                    <p class="text-md text-gray-600">The final module is dedicated to measurement and optimization, answering the critical question: "Did it work?". You will learn to measure the effectiveness of your marketing efforts using relevant Key Performance Indicators (KPIs). We will cover essential analytics tools, how to build insightful reports, and most importantly, how to use data to calculate ROI and make informed decisions for continuous improvement.</p>
                </div>
                <div id="evaluation-topics" class="space-y-4"></div>
            </section>

            <!-- Exam Content - Re-added Section -->
            <section id="module-exam" class="module-content hidden">
                <div class="bg-white p-6 rounded-xl shadow-lg mb-6">
                    <h2 class="text-3xl font-bold text-[#354f52] mb-2">Digital Marketing Fundamentals - Exam</h2>
                    <p class="text-md text-gray-600">Test your knowledge on the core concepts of digital marketing covered in this course. Choose the best answer for each question.</p>
                    <p class="text-sm text-gray-500 mt-2">
                        <strong>Note:</strong> This exam runs in your browser. Submitting it will not send results to a server, nor will it provide automatic scoring. It's for self-assessment only.
                    </p>
                </div>
                <div class="bg-white p-6 rounded-xl shadow-lg">
                    <form id="digitalMarketingExamForm">
                        <!-- Question 1: Digital Marketing Basics -->
                        <div class="exam-question">
                            <p>1. Which of the following is NOT typically considered a core component of digital marketing?</p>
                            <label class="block mb-2"><input type="radio" name="q1" value="a" class="mr-2"> a) Search Engine Optimization</label>
                            <label class="block mb-2"><input type="radio" name="q1" value="b" class="mr-2"> b) Television Advertising</label>
                            <label class="block mb-2"><input type="radio" name="q1" value="c" class="mr-2"> c) Social Media Marketing</label>
                            <label class="block"><input type="radio" name="q1" value="d" class="mr-2"> d) Email Marketing</label>
                        </div>

                        <!-- Question 2: Digital Marketing Basics -->
                        <div class="exam-question">
                            <p>2. A key advantage of digital marketing over traditional marketing is:</p>
                            <label class="block mb-2"><input type="radio" name="q2" value="a" class="mr-2"> a) Lower initial costs for all campaigns</label>
                            <label class="block mb-2"><input type="radio" name="q2" value="b" class="mr-2"> b) Easier to track ROI and target specific audiences</label>
                            <label class="block mb-2"><input type="radio" name="q2" value="c" class="mr-2"> c) Guaranteed viral reach for all content</label>
                            <label class="block"><input type="radio" name="q2" value="d" class="mr-2"> d) Exclusively targets offline consumers</label>
                        </div>

                        <!-- Question 3: SEO -->
                        <div class="exam-question">
                            <p>3. Which of these is an example of an "on-page SEO" factor?</p>
                            <label class="block mb-2"><input type="radio" name="q3" value="a" class="mr-2"> a) Building high-quality backlinks</label>
                            <label class="block mb-2"><input type="radio" name="q3" value="b" class="mr-2"> b) Optimizing meta titles and descriptions</label>
                            <label class="block mb-2"><input type="radio" name="q3" value="c" class="mr-2"> c) Getting mentions from influential websites</label>
                            <label class="block"><input type="radio" name="q3" value="d" class="mr-2"> d) Increasing website loading speed via server upgrades</label>
                        </div>

                        <!-- Question 4: SEO -->
                        <div class="exam-question">
                            <p>4. What does SERP stand for in the context of SEO?</p>
                            <label class="block mb-2"><input type="radio" name="q4" value="a" class="mr-2"> a) Search Engine Ranking Process</label>
                            <label class="block mb-2"><input type="radio" name="q4" value="b" class="mr-2"> b) Standardized External Resource Page</label>
                            <label class="block mb-2"><input type="radio" name="q4" value="c" class="mr-2"> c) Search Engine Results Page</label>
                            <label class="block"><input type="radio" name="q4" value="d" class="mr-2"> d) Site Engagement Reporting Protocol</label>
                        </div>

                        <!-- Question 5: SEM / PPC -->
                        <div class="exam-question">
                            <p>5. In Google Ads, what is the primary goal of setting a "maximum bid"?</p>
                            <label class="block mb-2"><input type="radio" name="q5" value="a" class="mr-2"> a) To limit the total budget for the campaign</label>
                            <label class="block mb-2"><input type="radio" name="q5" value="b" class="mr-2"> b) To determine the highest price you're willing to pay per click</label>
                            <label class="block mb-2"><input type="radio" name="q5" value="c" class="mr-2"> c) To control the number of impressions your ad receives</label>
                            <label class="block"><input type="radio" name="q5" value="d" class="mr-2"> d) To guarantee a top position in search results</label>
                        </div>

                        <!-- Question 6: SEM / PPC -->
                        <div class="exam-question">
                            <p>6. Which metric helps you understand the effectiveness of your PPC ad in terms of attracting clicks relative to impressions?</p>
                            <label class="block mb-2"><input type="radio" name="q6" value="a" class="mr-2"> a) Conversion Rate</label>
                            <label class="block mb-2"><input type="radio" name="q6" value="b" class="mr-2"> b) Cost Per Click (CPC)</label>
                            <label class="block mb-2"><input type="radio" name="q6" value="c" class="mr-2"> c) Click-Through Rate (CTR)</label>
                            <label class="block"><input type="radio" name="q6" value="d" class="mr-2"> d) Quality Score</label>
                        </div>

                        <!-- Question 7: Social Media Marketing -->
                        <div class="exam-question">
                            <p>7. Which social media platform is best known for short-form video content and trends?</p>
                            <label class="block mb-2"><input type="radio" name="q7" value="a" class="mr-2"> a) LinkedIn</label>
                            <label class="block mb-2"><input type="radio" name="q7" value="b" class="mr-2"> b) Facebook</label>
                            <label class="block mb-2"><input type="radio" name="q7" value="c" class="mr-2"> c) Instagram</label>
                            <label class="block"><input type="radio" name="q7" value="d" class="mr-2"> d) TikTok</label>
                        </div>

                        <!-- Question 8: Social Media Marketing -->
                        <div class="exam-question">
                            <p>8. Engaging with your audience by responding to comments and messages is primarily a tactic for:</p>
                            <label class="block mb-2"><input type="radio" name="q8" value="a" class="mr-2"> a) Boosting ad impressions</label>
                            <label class="block mb-2"><input type="radio" name="q8" value="b" class="mr-2"> b) Building community and brand loyalty</label>
                            <label class="block mb-2"><input type="radio" name="q8" value="c" class="mr-2"> c) Automating content distribution</label>
                            <label class="block"><input type="radio" name="q8" value="d" class="mr-2"> d) Directly increasing website traffic without ads</label>
                        </div>

                        <!-- Question 9: Content Marketing -->
                        <div class="exam-question">
                            <p>9. Which stage of the buyer's journey is best served by blog posts, infographics, and guides that answer common questions?</p>
                            <label class="block mb-2"><input type="radio" name="q9" value="a" class="mr-2"> a) Decision Stage</label>
                            <label class="block mb-2"><input type="radio" name="q9" value="b" class="mr-2"> b) Awareness Stage</label>
                            <label class="block mb-2"><input type="radio" name="q9" value="c" class="mr-2"> c) Consideration Stage</label>
                            <label class="block"><input type="radio" name="q9" value="d" class="mr-2"> d) Retention Stage</label>
                        </div>

                        <!-- Question 10: Content Marketing -->
                        <div class="exam-question">
                            <p>10. What is a key purpose of storytelling in content marketing?</p>
                            <label class="block mb-2"><input type="radio" name="q10" value="a" class="mr-2"> a) To make content longer and more detailed</label>
                            <label class="block mb-2"><input type="radio" name="q10" value="b" class="mr-2"> b) To connect emotionally with the audience and make content memorable</label>
                            <label class="block mb-2"><input type="radio" name="q10" value="c" class="mr-2"> c) To directly sell products in the first paragraph</label>
                            <label class="block"><input type="radio" name="q10" value="d" class="mr-2"> d) To optimize for specific keywords only</label>
                        </div>

                        <!-- Question 11: Email Marketing -->
                        <div class="exam-question">
                            <p>11. What is the primary benefit of segmenting your email list?</p>
                            <label class="block mb-2"><input type="radio" name="q11" value="a" class="mr-2"> a) To send more emails to every subscriber</label>
                            <label class="block mb-2"><input type="radio" name="q11" value="b" class="mr-2"> b) To ensure all emails look identical</label>
                            <label class="block mb-2"><input type="radio" name="q11" value="c" class="mr-2"> c) To send more targeted and relevant content to specific groups</label>
                            <label class="block"><input type="radio" name="q11" value="d" class="mr-2"> d) To reduce the overall cost of email marketing software</label>
                        </div>

                        <!-- Question 12: Email Marketing -->
                        <div class="exam-question">
                            <p>12. An email's "Open Rate" measures:</p>
                            <label class="block mb-2"><input type="radio" name="q12" value="a" class="mr-2"> a) The percentage of recipients who clicked a link in the email</label>
                            <label class="block mb-2"><input type="radio" name="q12" value="b" class="mr-2"> b) The percentage of emails that were successfully delivered</label>
                            <label class="block mb-2"><input type="radio" name="q12" value="c" class="mr-2"> c) The percentage of recipients who opened the email</label>
                            <label class="block"><input type="radio" name="q12" value="d" class="mr-2"> d) The percentage of recipients who unsubscribed</label>
                        </div>

                        <!-- Question 13: Analytics & Tracking -->
                        <div class="exam-question">
                            <p>13. In Google Analytics, what is a "conversion"?</p>
                            <label class="block mb-2"><input type="radio" name="q13" value="a" class="mr-2"> a) Any visit to your website</label>
                            <label class="block mb-2"><input type="radio" name="q13" value="b" class="mr-2"> b) A completed activity on your website that contributes to the success of your business</label>
                            <label class="block mb-2"><input type="radio" name="q13" value="c" class="mr-2"> c) The number of pages viewed per session</label>
                            <label class="block"><input type="radio" name="q13" value="d" class="mr-2"> d) The amount of time a user spends on your site</label>
                        </div>

                        <!-- Question 14: Analytics & Tracking -->
                        <div class="exam-question">
                            <p>14. Which KPI would be most relevant if your goal is to understand how many unique visitors returned to your website over a period?</p>
                            <label class="block mb-2"><input type="radio" name="q14" value="a" class="mr-2"> a) Bounce Rate</label>
                            <label class="block mb-2"><input type="radio" name="q14" value="b" class="mr-2"> b) Sessions</label>
                            <label class="block mb-2"><input type="radio" name="q14" value="c" class="mr-2"> c) New Users</label>
                            <label class="block"><input type="radio" name="q14" value="d" class="mr-2"> d) Returning Users</label>
                        </div>

                        <button type="submit" class="exam-submit-button">Submit Exam</button>
                    </form>
                </div>
            </section>

            <!-- New Tools Section for LLM features -->
            <section id="module-tools" class="module-content hidden">
                <div class="bg-white p-6 rounded-xl shadow-lg mb-6">
                    <h2 class="text-3xl font-bold text-[#354f52] mb-2">Marketing AI Tools</h2>
                    <p class="text-md text-gray-600">Leverage AI to brainstorm ideas and get quick explanations for marketing concepts.</p>
                </div>

                <div class="space-y-6">
                    <!-- Marketing Plan Generator -->
                    <div class="bg-white p-6 rounded-xl shadow-lg">
                        <h3 class="text-2xl font-bold text-[#354f52] mb-4">✨ Generate Marketing Plan Outline</h3>
                        <p class="text-md text-gray-700 mb-4">Get a high-level marketing plan outline based on your industry and product/service.</p>
                        <div class="mb-4">
                            <label for="industryInput" class="block text-gray-700 text-sm font-bold mb-2">Industry (e.g., Fashion, Food, Tech):</label>
                            <input type="text" id="industryInput" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" placeholder="e.g., Luxury Fashion">
                        </div>
                        <div class="mb-4">
                            <label for="productServiceInput" class="block text-gray-700 text-sm font-bold mb-2">Product/Service (e.g., Sustainable Handbags, Plant-Based Burgers):</label>
                            <input type="text" id="productServiceInput" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" placeholder="e.g., Artisan Coffee Beans">
                        </div>
                        <button id="generatePlanBtn" class="submit-button">Generate Plan <span id="planSpinner" class="loading-spinner hidden"></span></button>
                        <div id="planOutput" class="llm-output hidden"></div>
                    </div>

                    <!-- Concept Explainer -->
                    <div class="bg-white p-6 rounded-xl shadow-lg">
                        <h3 class="text-2xl font-bold text-[#354f52] mb-4">✨ Explain Marketing Concept</h3>
                        <p class="text-md text-gray-700 mb-4">Enter a marketing concept and get a quick explanation.</p>
                        <div class="mb-4">
                            <label for="conceptInput" class="block text-gray-700 text-sm font-bold mb-2">Marketing Concept:</label>
                            <input type="text" id="conceptInput" class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" placeholder="e.g., Customer Lifetime Value">
                        </div>
                        <button id="explainConceptBtn" class="submit-button">Explain Concept <span id="conceptSpinner" class="loading-spinner hidden"></span></button>
                        <div id="conceptOutput" class="llm-output hidden"></div>
                    </div>
                </div>
            </section>

        </main>
    </d
