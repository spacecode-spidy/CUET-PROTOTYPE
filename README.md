<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NTA Mock Test Prototype - CUET UG Style</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            background-color: #f0f0f0;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: flex-start; /* Align to top for scroll */
            min-height: 100vh;
        }

        .main-container {
            width: 100%;
            max-width: 1200px; /* Max width for larger screens */
            background-color: #fff;
            box-shadow: 0 0 15px rgba(0,0,0,0.1);
            margin-top: 20px; /* Space from top */
            margin-bottom: 20px; /* Space from bottom */
        }

        .page {
            display: none; /* Hide pages by default */
            padding: 0;
        }

        .page.active-page {
            display: block; /* Show active page */
        }

        /* Header Styles */
        .nta-header {
            background-color: #fff;
            padding: 15px 20px;
            display: flex;
            align-items: center;
            border-bottom: 1px solid #ddd;
            position: relative; /* For language selector positioning */
        }

        .nta-logo {
            height: 50px;
            margin-right: 15px;
        }

        .nta-header h1 {
            color: #007bff; /* NTA Blue */
            font-size: 1.5em;
            margin: 0;
        }

        .nta-header p {
            color: #555;
            font-size: 0.9em;
            margin: 0;
        }

        .language-selector {
            position: absolute;
            right: 20px;
            top: 50%;
            transform: translateY(-50%);
            display: flex;
            flex-direction: column;
            align-items: flex-end;
        }
        .language-selector label {
            font-size: 0.8em;
            margin-bottom: 3px;
            color: #555;
        }
        .language-selector select {
            padding: 5px;
            border: 1px solid #ccc;
            border-radius: 4px;
            background-color: #fff;
        }

        .content-area {
            padding: 20px;
        }

        .content-area h2 {
            color: #E65100; /* NTA Orange */
            border-bottom: 2px solid #E65100;
            padding-bottom: 10px;
            margin-bottom: 20px;
        }

        /* Instruction Page Specific */
        .instructions-text {
            font-size: 0.9em;
            line-height: 1.6;
            margin-bottom: 20px;
        }
        .instructions-text h3 {
            margin-top: 15px;
            color: #333;
        }
        .instructions-text ul {
            list-style-type: lower-alpha;
            padding-left: 20px;
        }
        .instructions-text ol > li {
            margin-bottom: 10px;
        }
        .note {
            font-style: italic;
            color: #555;
            margin-top: 20px;
            padding: 10px;
            background-color: #f9f9f9;
            border-left: 3px solid #007bff;
        }
        .agreement {
            margin-top: 20px;
            padding: 15px;
            background-color: #e9ecef;
            border: 1px solid #ced4da;
            border-radius: 4px;
            display: flex;
            align-items: flex-start;
        }
        .agreement input[type="checkbox"] {
            margin-right: 10px;
            margin-top: 4px; /* Align with text */
        }
        .agreement label {
            font-size: 0.9em;
            color: #495057;
        }

        .action-button {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 10px 20px;
            font-size: 1em;
            border-radius: 4px;
            cursor: pointer;
            transition: background-color 0.3s;
            display: block;
            margin: 20px auto 0 auto; /* Center button */
        }
        .action-button:hover:not(:disabled) {
            background-color: #0056b3;
        }
        .action-button:disabled {
            background-color: #cccccc;
            cursor: not-allowed;
        }

        /* Exam Selection Page Specific */
        .instruction-symbols {
            list-style-type: none;
            padding: 0;
            margin-bottom: 30px;
        }
        .instruction-symbols li {
            margin-bottom: 8px;
            display: flex;
            align-items: center;
        }
        .symbol-box {
            width: 20px;
            height: 20px;
            border: 1px solid #ccc;
            margin-right: 10px;
            display: inline-block;
        }
        .symbol-box.not-visited { background-color: #f8f9fa; } /* Light gray */
        .symbol-box.not-answered { background-color: #dc3545; } /* Red */
        .symbol-box.answered { background-color: #28a745; } /* Green */
        .symbol-box.marked-review { background-color: #6f42c1; } /* Purple */
        .symbol-box.answered-marked-review { background-color: #6f42c1; border: 2px solid #28a745; } /* Purple with green border element */

        .selection-form div {
            margin-bottom: 15px;
        }
        .selection-form label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #555;
        }
        .selection-form select {
            width: 100%;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }
        .error-message {
            color: red;
            font-size: 0.8em;
            margin-top: 3px;
            display: none; /* Hidden by default */
        }


        /* Question Page Specific */
        .question-header {
            background-color: #E65100; /* NTA Orange */
            color: white;
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .exam-info span, .user-timer-info .timer span {
            margin-right: 15px;
            font-weight: bold;
        }
        .user-timer-info {
            display: flex;
            align-items: center;
        }
        .user-details {
            font-size: 0.8em;
            text-align: right;
            margin-right: 20px;
            border-right: 1px solid #ffcc80; /* Lighter orange */
            padding-right: 20px;
        }
        .user-details p {
            margin: 2px 0;
        }
        .timer {
            font-size: 1.1em;
        }

        .question-main-area {
            display: flex;
            padding: 20px;
            min-height: 400px; /* Ensure enough space for question content */
        }
        .question-content {
            flex: 3; /* Takes 3/4 of space */
            padding-right: 20px;
        }
        .question-palette-container {
            flex: 1; /* Takes 1/4 of space */
            border-left: 1px solid #ddd;
            padding-left: 20px;
        }

        .question-number {
            font-weight: bold;
            margin-bottom: 10px;
        }
        .question-text {
            margin-bottom: 20px;
            line-height: 1.5;
            font-size: 1.1em;
        }
        .question-text u {
            text-decoration: underline;
            font-weight: bold;
        }
        .question-text b {
            font-weight: bold;
        }

        .options-container {
            margin-bottom: 20px;
        }
        .option {
            margin-bottom: 10px;
            padding: 8px;
            border: 1px solid #eee;
            border-radius: 4px;
            cursor: pointer;
            display: flex;
            align-items: center;
        }
        .option:hover {
            background-color: #f8f9fa;
        }
        .option input[type="radio"] {
            margin-right: 10px;
            accent-color: #007bff;
        }
        .option.selected {
            background-color: #e2e6ea;
            border-color: #dae0e5;
        }

        .question-navigation {
            margin-bottom: 20px;
            display: flex;
            flex-wrap: wrap; /* Allow buttons to wrap on smaller screens */
            gap: 10px; /* Space between buttons */
        }
        .nav-btn {
            padding: 10px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 0.9em;
            color: white;
            flex-grow: 1; /* Allow buttons to grow */
            min-width: 150px; /* Minimum width for buttons */
        }
        .nav-btn.save { background-color: #28a745; } /* Green */
        .nav-btn.save-mark { background-color: #fd7e14; } /* Orange */
        .nav-btn.clear { background-color: #6c757d; } /* Gray */
        .nav-btn.mark { background-color: #007bff; } /* Blue */

        .bottom-navigation {
            margin-top: 20px;
            display: flex;
            justify-content: space-between;
        }
        .nav-btn-small {
            padding: 8px 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
            background-color: #f8f9fa;
            cursor: pointer;
        }
        .nav-btn-small.submit {
            background-color: #dc3545; /* Red */
            color: white;
            border-color: #dc3545;
        }

        .question-palette-container h4 {
            margin-top: 0;
            color: #333;
            border-bottom: 1px solid #eee;
            padding-bottom: 5px;
        }
        .palette-legend {
            font-size: 0.8em;
            margin-bottom: 15px;
        }
        .palette-legend div {
            margin-bottom: 5px;
            display: flex;
            align-items: center;
        }
        .status-dot {
            width: 12px;
            height: 12px;
            border-radius: 50%;
            margin-right: 8px;
            display: inline-block;
            border: 1px solid #ccc;
        }
        .status-dot.not-viewed { background-color: #f8f9fa; } /* Matches symbol-box */
        .status-dot.not-attempted { background-color: #dc3545; }
        .status-dot.attempted { background-color: #28a745; }
        .status-dot.marked-review-palette { background-color: #6f42c1; }
        .status-dot.answered-marked-review-palette { background-color: #6f42c1; position: relative; }
        .status-dot.answered-marked-review-palette::after { /* Green tick illusion */
            content: '';
            position: absolute;
            top: 2px; left: 2px;
            width: 6px; height: 6px;
            background-color: #28a745;
            border-radius: 50%;
        }

        .palette-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(40px, 1fr));
            gap: 8px;
        }
        .palette-item {
            width: 40px;
            height: 40px;
            display: flex;
            justify-content: center;
            align-items: center;
            border: 1px solid #ccc;
            border-radius: 4px;
            cursor: pointer;
            font-weight: bold;
            background-color: #f8f9fa; /* Default: Not Viewed */
        }
        .palette-item.current {
            border: 2px solid #0056b3; /* Blue outline for current */
            box-shadow: 0 0 5px rgba(0, 86, 179, 0.5);
        }
        .palette-item.not-viewed { background-color: #f8f9fa; color: #333; }
        .palette-item.not-attempted { background-color: #dc3545; color: white; }
        .palette-item.attempted { background-color: #28a745; color: white; }
        .palette-item.marked-review { background-color: #6f42c1; color: white; }
        .palette-item.answered-marked-review {
            background-color: #6f42c1; /* Purple */
            color: white;
            position: relative; /* For the small green dot */
        }
        .palette-item.answered-marked-review::after { /* Small green indicator */
            content: '';
            position: absolute;
            bottom: 3px;
            right: 3px;
            width: 8px;
            height: 8px;
            background-color: #28a745; /* Green */
            border-radius: 50%;
            border: 1px solid white; /* Optional: white border for contrast */
        }

        /* Footer Styles */
        .nta-footer {
            background-color: #343a40; /* Dark Gray */
            color: white;
            text-align: center;
            padding: 15px 0;
            font-size: 0.9em;
            margin-top: auto; /* Pushes footer to bottom if content is short */
        }
    </style>
</head>
<body>
    <div class="main-container">
        <!-- Page 1: Instructions -->
        <div id="instructionPage" class="page active-page">
            <header class="nta-header">
                <img src="https://nta.ac.in/img/nta-logo.png" alt="NTA Logo" class="nta-logo">
                <div>
                    <h1>National Testing Agency</h1>
                    <p>Excellence in Assessment</p>
                </div>
            </header>
            <div class="content-area">
                <h2>Mock Test Instructions</h2>
                <div class="instructions-text">
                    <h3>Answering a Question:</h3>
                    <ol start="8">
                        <li>Procedure for answering a multiple choice type question:
                            <ul>
                                <li>a. To select you answer, click on the button of one of the options.</li>
                                <li>b. To deselect your chosen answer, click on the button of the chosen option again or click on the Clear Response button.</li>
                                <li>c. To save your answer, you MUST click on the Save & Next button.</li>
                                <li>d. To mark the question for review, click on the Mark for Review & Next button.</li>
                            </ul>
                        </li>
                        <li>To change your answer to a question that has already been answered, first select that question for answering and then follow the procedure for answering that type of question.</li>
                    </ol>
                    <h3>Navigating through sections:</h3>
                    <ol start="10">
                        <li>Sections in this question paper are displayed on the top bar of the screen. Questions in a section can be viewed by click on the section name. The section you are currently viewing is highlighted.</li>
                        <li>After click the Save & Next button on the last question for a section, you will automatically be taken to the first question of the next section.</li>
                        <li>You can shuffle between sections and questions anything during the examination as per your convenience only during the time stipulated.</li>
                        <li>Candidate can view the corresponding section summery as part of the legend that appears in every section above the question palette.</li>
                    </ol>
                    <p class="note">Please note all questions will appear in your default language. This language can be changed for a particular question later on.</p>
                </div>
                <div class="agreement">
                    <input type="checkbox" id="agreeCheckbox">
                    <label for="agreeCheckbox">I have read and understood the instructions. All computer hardware allotted to me are in proper working condition. I declare that I am not in possession of / not wearing / not carrying any prohibited gadget like mobile phone, bluetooth devices etc. /any prohibited material with me into the Examination Hall. I agree that in case of not adhering to the instructions, I shall be liable to be debarred from this Test and/or to disciplinary action, which may include ban from future Tests / Examinations.</label>
                </div>
                <button id="proceedBtn" class="action-button" disabled>PROCEED</button>
            </div>
            <footer class="nta-footer">
                © All Rights Reserved - National Testing Agency
            </footer>
        </div>

        <!-- Page 2: Exam Selection -->
        <div id="selectionPage" class="page">
            <header class="nta-header">
                <img src="https://nta.ac.in/img/nta-logo.png" alt="NTA Logo" class="nta-logo">
                <div>
                    <h1>National Testing Agency</h1>
                    <p>Excellence in Assessment</p>
                </div>
                <div class="language-selector">
                    <label for="defaultLanguage">Choose Your Default Language</label>
                    <select id="defaultLanguage">
                        <option value="english" selected>English</option>
                        <option value="hindi">Hindi</option>
                    </select>
                </div>
            </header>
            <div class="content-area">
                <h2>GENERAL INSTRUCTIONS</h2>
                <p><strong>Please read the instructions carefully</strong></p>
                <ul class="instruction-symbols">
                    <li><span class="symbol-box not-visited"></span> You have not visited the question yet.</li>
                    <li><span class="symbol-box not-answered"></span> You have not answered the question.</li>
                    <li><span class="symbol-box answered"></span> You have answered the question.</li>
                    <li><span class="symbol-box marked-review"></span> You have NOT answered the question, but have marked the question for review.</li>
                    <li><span class="symbol-box answered-marked-review"></span> The question(s) "Answered and Marked for Review" will be considered for evaluation.</li>
                </ul>

                <div class="selection-form">
                    <div>
                        <label for="examSelect">Select exam you would like to appear</label>
                        <select id="examSelect">
                            <option value="">--Select--</option>
                            <option value="JEE-Main">JEE-Main</option>
                            <option value="UGC-NET">UGC-NET</option>
                            <option value="NEET">NEET</option>
                            <option value="CUET-UG">CUET(UG)</option>
                            <option value="CUET-PG">CUET(PG)</option>
                        </select>
                        <p class="error-message" id="examError">This value is required.</p>
                    </div>
                    <div>
                        <label for="paperSelect">Paper</label>
                        <select id="paperSelect">
                            <option value="">--Select--</option>
                            <option value="Paper-1">Paper-1 (General Test)</option>
                            <option value="Paper-2">Paper-2 (Domain Specific)</option>
                        </select>
                        <p class="error-message" id="paperError">This value is required.</p>
                    </div>
                    <button id="startTestBtn" class="action-button">START MOCK TEST</button>
                </div>
            </div>
            <footer class="nta-footer">
                © All Rights Reserved - National Testing Agency
            </footer>
        </div>

        <!-- Page 3: Question Page -->
        <div id="questionPage" class="page">
            <header class="question-header">
                <div class="exam-info">
                    <span id="examNameDisplay">CUET (UG)</span>
                    <span id="sectionNameDisplay">General Test Mock</span>
                    <span id="questionProgressDisplay">0/15</span>
                </div>
                <div class="user-timer-info">
                    <div class="user-details">
                        <p>Candidate Name: <span id="candidateName">Aspirant User</span></p>
                        <p>Candidate Email: <span id="candidateEmail">user@example.com</span></p>
                        <p>Candidate ID: <span id="candidateId">2024</span></p>
                    </div>
                    <div class="timer">
                        Time Left: <span id="timeLeft">00:10:00</span>
                    </div>
                </div>
            </header>
            <div class="question-main-area">
                <div class="question-content">
                    <p class="question-number">Question No. <span id="currentQuestionNumber">1</span></p>
                    <p class="question-text" id="questionTextContainer">Loading question...</p>
                    <div class="options-container" id="optionsContainer">
                        <!-- Options will be populated by JS -->
                    </div>
                    <div class="question-navigation">
                        <button id="saveNextBtn" class="nav-btn save">Save & Next</button>
                        <button id="saveMarkReviewBtn" class="nav-btn save-mark">Save & Marked for Review & Next</button>
                        <button id="clearResponseBtn" class="nav-btn clear">Clear Response</button>
                        <button id="markReviewNextBtn" class="nav-btn mark">Marked for Review & Next</button>
                    </div>
                    <div class="bottom-navigation">
                        <button id="backBtn" class="nav-btn-small"><< Back</button>
                        <button id="nextBtn" class="nav-btn-small">Next >></button>
                        <button id="submitTestBtn" class="nav-btn-small submit">Submit</button>
                    </div>
                </div>
                <div class="question-palette-container">
                    <h4>Question Palette</h4>
                    <div class="palette-legend">
                        <div><span class="status-dot not-viewed"></span> Not Viewed</div>
                        <div><span class="status-dot not-attempted"></span> Not Attempted</div>
                        <div><span class="status-dot attempted"></span> Attempted</div>
                        <div><span class="status-dot marked-review-palette"></span> Marked For Review</div>
                        <div><span class="status-dot answered-marked-review-palette"></span> Answered & Marked For Review</div>
                    </div>
                    <div class="palette-grid" id="paletteGrid">
                        <!-- Palette numbers will be populated by JS -->
                    </div>
                </div>
            </div>
            <footer class="nta-footer">
                 © All Rights Reserved - National Testing Agency
            </footer>
        </div>

    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // Page elements
            const instructionPage = document.getElementById('instructionPage');
            const selectionPage = document.getElementById('selectionPage');
            const questionPage = document.getElementById('questionPage');
            const pages = [instructionPage, selectionPage, questionPage];

            // Instruction Page Elements
            const agreeCheckbox = document.getElementById('agreeCheckbox');
            const proceedBtn = document.getElementById('proceedBtn');

            // Selection Page Elements
            const examSelect = document.getElementById('examSelect');
            const paperSelect = document.getElementById('paperSelect');
            const examError = document.getElementById('examError');
            const paperError = document.getElementById('paperError');
            const startTestBtn = document.getElementById('startTestBtn');

            // Question Page Elements
            const examNameDisplay = document.getElementById('examNameDisplay');
            const sectionNameDisplay = document.getElementById('sectionNameDisplay');
            const questionProgressDisplay = document.getElementById('questionProgressDisplay');
            const timeLeftDisplay = document.getElementById('timeLeft');
            const currentQuestionNumberDisplay = document.getElementById('currentQuestionNumber');
            const questionTextContainer = document.getElementById('questionTextContainer');
            const optionsContainer = document.getElementById('optionsContainer');
            const paletteGrid = document.getElementById('paletteGrid');

            // Question Navigation Buttons
            const saveNextBtn = document.getElementById('saveNextBtn');
            const saveMarkReviewBtn = document.getElementById('saveMarkReviewBtn');
            const clearResponseBtn = document.getElementById('clearResponseBtn');
            const markReviewNextBtn = document.getElementById('markReviewNextBtn');
            const backBtn = document.getElementById('backBtn');
            const nextBtn = document.getElementById('nextBtn');
            const submitTestBtn = document.getElementById('submitTestBtn');

            // Data
            let questions = [];
            let currentQuestionIndex = 0;
            let userAnswers = []; 
            let timerInterval;
            const EXAM_DURATION_SECONDS = 10 * 60; // 10 minutes for prototype

            // --- Page Navigation Logic ---
            function showPage(pageToShow) {
                pages.forEach(page => page.classList.remove('active-page'));
                pageToShow.classList.add('active-page');
            }

            agreeCheckbox.addEventListener('change', () => {
                proceedBtn.disabled = !agreeCheckbox.checked;
            });

            proceedBtn.addEventListener('click', () => {
                if (agreeCheckbox.checked) {
                    showPage(selectionPage);
                }
            });

            startTestBtn.addEventListener('click', () => {
                let valid = true;
                if (examSelect.value === "") {
                    examError.style.display = 'block';
                    valid = false;
                } else {
                    examError.style.display = 'none';
                }
                if (paperSelect.value === "") {
                    paperError.style.display = 'block';
                    valid = false;
                } else {
                    paperError.style.display = 'none';
                }

                if (valid) {
                    examNameDisplay.textContent = examSelect.options[examSelect.selectedIndex].text;
                    sectionNameDisplay.textContent = paperSelect.options[paperSelect.selectedIndex].text;
                    initializeTest();
                    showPage(questionPage);
                }
            });

            // --- Test Initialization and Question Handling ---
            function initializeTest() {
                // CUET UG Style Mock Questions
                questions = [
                    // --- GEOGRAPHY (Questions 1-5) ---
                    {
                        id: 1,
                        text: "Which of the following Indian states does NOT share a border with China?",
                        options: ["Arunachal Pradesh", "Sikkim", "Bihar", "Uttarakhand"],
                        correctAnswer: 2 // Bihar
                    },
                    {
                        id: 2,
                        text: "The 'Ring of Fire' is an area in the basin of which ocean, known for a large number of earthquakes and volcanic eruptions?",
                        options: ["Atlantic Ocean", "Indian Ocean", "Pacific Ocean", "Arctic Ocean"],
                        correctAnswer: 2 // Pacific Ocean
                    },
                    {
                        id: 3,
                        text: "Which of the following latitudes passes through the middle of India, dividing it into almost two equal climatic halves?",
                        options: ["Equator", "Tropic of Capricorn", "Tropic of Cancer", "Arctic Circle"],
                        correctAnswer: 2 // Tropic of Cancer
                    },
                    {
                        id: 4,
                        text: "Laterite soils, commonly found in regions with high temperature and heavy rainfall, are rich in which of the following?",
                        options: ["Calcium and Potash", "Iron oxide and Aluminium", "Nitrogen and Phosphorus", "Organic matter and Humus"],
                        correctAnswer: 1 // Iron oxide and Aluminium
                    },
                    {
                        id: 5,
                        text: "The Chipko Movement, primarily aimed at forest conservation, originated in which Indian state known for its Himalayan ranges?",
                        options: ["Himachal Pradesh", "Uttarakhand", "Sikkim", "Jammu and Kashmir"],
                        correctAnswer: 1 // Uttarakhand
                    },

                    // --- ENGLISH (Questions 6-10) ---
                    {
                        id: 6,
                        text: "Choose the word that is most similar in meaning (SYNONYM) to '<b>Ephemeral</b>'.",
                        options: ["Permanent", "Transitory", "Beautiful", "Strong"],
                        correctAnswer: 1 // Transitory
                    },
                    {
                        id: 7,
                        text: "Identify the part of the sentence that has an error: <u>Neither of the students</u> (A) / <u>have submitted</u> (B) / <u>their assignments</u> (C) / <u>on time</u> (D).",
                        options: ["A", "B", "C", "D"],
                        correctAnswer: 1 // B (should be 'has submitted')
                    },
                    {
                        id: 8,
                        text: "Select the most appropriate meaning of the idiom: 'To beat around the bush'.",
                        options: ["To speak directly", "To avoid talking about the main topic", "To clear a bush", "To get very angry"],
                        correctAnswer: 1 // To avoid talking about the main topic
                    },
                    {
                        id: 9,
                        text: "Choose the option that best completes the sentence: 'She has a strong aversion ______ spicy food.'",
                        options: ["for", "with", "to", "against"],
                        correctAnswer: 2 // to
                    },
                    {
                        id: 10,
                        text: "Rearrange the following parts (P, Q, R, S) to form a meaningful sentence: <br>P: was awarded <br>Q: for her outstanding contribution <br>R: the Nobel Peace Prize <br>S: to human rights.",
                        options: ["PSRQ", "RQPS", "PRSQ", "RPSQ"],
                        correctAnswer: 3 // RPSQ (The Nobel Peace Prize was awarded for her outstanding contribution to human rights.)
                    },

                    // --- GENERAL APTITUDE / GENERAL TEST (Questions 11-15) ---
                    {
                        id: 11,
                        text: "If the price of an item is increased by 20% and then decreased by 20%, what is the net percentage change in its price?",
                        options: ["0% (No change)", "4% increase", "4% decrease", "1% decrease"],
                        correctAnswer: 2 // 4% decrease
                    },
                    {
                        id: 12,
                        text: "Complete the series: 3, 7, 15, 31, 63, ?",
                        options: ["127", "125", "94", "131"],
                        correctAnswer: 0 // 127 (Pattern: x*2 + 1)
                    },
                    {
                        id: 13,
                        text: "In a certain code language, 'TEACHER' is written as 'VGCEJGT'. How will 'STUDENT' be written in that code language?",
                        options: ["UVWFGPV", "KVWFGPU", "KVWFGPX", "SVWFGPV"],
                        correctAnswer: 0 // UVWFGPV (Pattern: Each letter +2)
                    },
                    {
                        id: 14,
                        text: "Who is known as the 'Father of the Indian Constitution'?",
                        options: ["Mahatma Gandhi", "Jawaharlal Nehru", "Sardar Vallabhbhai Patel", "Dr. B. R. Ambedkar"],
                        correctAnswer: 3 // Dr. B. R. Ambedkar
                    },
                    {
                        id: 15,
                        text: "World Environment Day is celebrated on:",
                        options: ["April 22", "June 5", "October 16", "December 1"],
                        correctAnswer: 1 // June 5
                    }
                ];

                userAnswers = questions.map(q => ({
                    questionId: q.id,
                    selectedOption: null,
                    status: 'not-viewed'
                }));

                currentQuestionIndex = 0;
                loadQuestion(currentQuestionIndex);
                renderPalette();
                startTimer(EXAM_DURATION_SECONDS);
                updateQuestionProgress();
            }

            function loadQuestion(index) {
                if (index < 0 || index >= questions.length) return;

                const question = questions[index];
                currentQuestionIndex = index; 

                if (userAnswers[index].status === 'not-viewed') {
                    userAnswers[index].status = 'not-attempted';
                }

                currentQuestionNumberDisplay.textContent = question.id;
                questionTextContainer.innerHTML = question.text;

                optionsContainer.innerHTML = '';
                question.options.forEach((option, i) => {
                    const optionId = `q${question.id}_opt${i}`;
                    const div = document.createElement('div');
                    div.classList.add('option');
                    div.innerHTML = `
                        <input type="radio" id="${optionId}" name="q${question.id}_options" value="${i}">
                        <label for="${optionId}">${i + 1}. ${option}</label>
                    `;
                    optionsContainer.appendChild(div);

                    if (userAnswers[index].selectedOption === i) {
                        div.querySelector('input[type="radio"]').checked = true;
                        div.classList.add('selected');
                    }

                    div.addEventListener('click', (event) => {
                        if (event.target.tagName === 'LABEL' && event.target.previousElementSibling.type === 'radio') {
                             event.target.previousElementSibling.checked = true; // Ensure radio is checked if label is clicked
                        }
                        
                        optionsContainer.querySelectorAll('.option').forEach(optDiv => optDiv.classList.remove('selected'));
                        div.classList.add('selected');
                        const radioBtn = div.querySelector('input[type="radio"]');
                        if (radioBtn) radioBtn.checked = true;


                        userAnswers[index].selectedOption = i;
                        if (userAnswers[index].status !== 'answered-marked-review') {
                             userAnswers[index].status = 'attempted';
                        }
                        renderPalette();
                    });
                });
                updateNavigationButtons();
                renderPalette();
                updateQuestionProgress();
            }
            
            function updateQuestionProgress() {
                const answeredCount = userAnswers.filter(ans => ans.status === 'attempted' || ans.status === 'answered-marked-review').length;
                questionProgressDisplay.textContent = `${answeredCount}/${questions.length}`;
            }


            function updateNavigationButtons() {
                backBtn.disabled = currentQuestionIndex === 0;
                nextBtn.disabled = currentQuestionIndex === questions.length - 1;
            }

            function renderPalette() {
                paletteGrid.innerHTML = '';
                questions.forEach((q, i) => {
                    const item = document.createElement('div');
                    item.classList.add('palette-item');
                    item.textContent = q.id;
                    item.dataset.questionIndex = i;

                    item.classList.remove('not-viewed', 'not-attempted', 'attempted', 'marked-review', 'answered-marked-review', 'current');
                    const status = userAnswers[i].status;
                    item.classList.add(status); 

                    if (i === currentQuestionIndex) {
                        item.classList.add('current');
                    }

                    item.addEventListener('click', () => {
                        loadQuestion(i);
                    });
                    paletteGrid.appendChild(item);
                });
                updateQuestionProgress();
            }

            saveNextBtn.addEventListener('click', () => {
                if (userAnswers[currentQuestionIndex].selectedOption !== null) {
                    userAnswers[currentQuestionIndex].status = 'attempted';
                } else {
                    if (userAnswers[currentQuestionIndex].status !== 'marked-review' && userAnswers[currentQuestionIndex].status !== 'answered-marked-review') {
                         userAnswers[currentQuestionIndex].status = 'not-attempted';
                    }
                }
                moveToNextQuestion();
            });

            saveMarkReviewBtn.addEventListener('click', () => {
                if (userAnswers[currentQuestionIndex].selectedOption !== null) {
                    userAnswers[currentQuestionIndex].status = 'answered-marked-review';
                } else {
                    alert("Please select an option to save and mark for review.");
                    return;
                }
                moveToNextQuestion();
            });

            clearResponseBtn.addEventListener('click', () => {
                userAnswers[currentQuestionIndex].selectedOption = null;
                userAnswers[currentQuestionIndex].status = 'not-attempted';
                optionsContainer.querySelectorAll('input[type="radio"]').forEach(radio => radio.checked = false);
                optionsContainer.querySelectorAll('.option').forEach(optDiv => optDiv.classList.remove('selected'));
                renderPalette();
            });

            markReviewNextBtn.addEventListener('click', () => {
                if (userAnswers[currentQuestionIndex].selectedOption !== null) {
                     userAnswers[currentQuestionIndex].status = 'answered-marked-review';
                } else {
                    userAnswers[currentQuestionIndex].status = 'marked-review';
                }
                moveToNextQuestion();
            });

            nextBtn.addEventListener('click', () => {
                moveToNextQuestion();
            });

            backBtn.addEventListener('click', () => {
                if (currentQuestionIndex > 0) {
                    loadQuestion(currentQuestionIndex - 1);
                }
            });

            function moveToNextQuestion() {
                if (currentQuestionIndex < questions.length - 1) {
                    loadQuestion(currentQuestionIndex + 1);
                } else {
                    alert("You are at the last question.");
                    renderPalette(); 
                }
            }

            submitTestBtn.addEventListener('click', () => {
                const confirmation = confirm("Are you sure you want to submit the test? This action cannot be undone.");
                if (confirmation) {
                    clearInterval(timerInterval);
                    let attemptedCount = userAnswers.filter(ans => ans.status === 'attempted').length;
                    let answeredMarkedReviewCount = userAnswers.filter(ans => ans.status === 'answered-marked-review').length;
                    let markedReviewUnansweredCount = userAnswers.filter(ans => ans.status === 'marked-review').length;
                    
                    // Calculate Not Attempted: Total - (Attempted + AnsweredMarkedReview + MarkedReviewUnanswered)
                    // Note: 'attemptedCount' here should ideally represent *only* those purely attempted, not those also marked for review.
                    // Let's refine the counts for clarity in the summary:
                    const purelyAttempted = userAnswers.filter(ans => ans.status === 'attempted').length;
                    const answeredAndMarked = userAnswers.filter(ans => ans.status === 'answered-marked-review').length;
                    const markedUnanswered = userAnswers.filter(ans => ans.status === 'marked-review').length;
                    const notViewedOrNotAttempted = userAnswers.filter(ans => ans.status === 'not-viewed' || ans.status === 'not-attempted').length;


                    alert(
                        `Test Submitted!\n\n` +
                        `Total Questions: ${questions.length}\n` +
                        `Answered: ${purelyAttempted}\n`+
                        `Answered & Marked for Review: ${answeredAndMarked}\n`+
                        `Marked for Review (Not Answered): ${markedUnanswered}\n`+
                        `Not Attempted / Not Viewed: ${notViewedOrNotAttempted}\n\n`+
                        `Thank you!`
                    );
                    
                    showPage(selectionPage);
                }
            });

            function startTimer(duration) {
                let timer = duration;
                if (timerInterval) clearInterval(timerInterval);

                timerInterval = setInterval(() => {
                    let hours = Math.floor((timer / 3600));
                    let minutes = Math.floor((timer % 3600) / 60);
                    let seconds = Math.floor(timer % 60);

                    hours = hours < 10 ? "0" + hours : hours;
                    minutes = minutes < 10 ? "0" + minutes : minutes;
                    seconds = seconds < 10 ? "0" + seconds : seconds;

                    timeLeftDisplay.textContent = `${hours}:${minutes}:${seconds}`;

                    if (--timer < 0) {
                        clearInterval(timerInterval);
                        alert("Time's up! The test will be submitted automatically.");
                        submitTestBtn.click();
                    }
                }, 1000);
            }

            showPage(instructionPage);
        });
    </script>
</body>
</html>
