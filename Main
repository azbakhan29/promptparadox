<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prompt Engineering Paradox</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8;
            color: #2d3748;
            line-height: 1.6;
        }
        .container {
            max-width: 1024px;
        }
        .card {
            background-color: #ffffff;
            border-radius: 0.75rem; /* rounded-xl */
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            border: 1px solid #e2e8f0;
        }
        textarea {
            resize: vertical;
            min-height: 100px;
        }
        button {
            transition: all 0.2s ease-in-out;
        }
        button:hover {
            opacity: 0.9;
            transform: translateY(-1px);
        }
        button:active {
            transform: translateY(0);
        }
        .btn-primary {
            background-color: #4c51bf; /* indigo-700 */
            color: white;
        }
        .btn-secondary {
            background-color: #a0aec0; /* gray-400 */
            color: #2d3748;
        }
        .btn-success {
            background-color: #38a169; /* green-600 */
            color: white;
        }
        .btn-warning {
            background-color: #dd6b20; /* orange-600 */
            color: white;
        }
        .feedback-message {
            padding: 0.75rem;
            border-radius: 0.5rem;
            margin-top: 1rem;
            font-weight: 600;
        }
        .feedback-success {
            background-color: #d1fae5; /* green-100 */
            color: #10b981; /* green-500 */
        }
        .feedback-error {
            background-color: #fee2e2; /* red-100 */
            color: #ef4444; /* red-500 */
        }
        .rubric-header {
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .rubric-content {
            max-height: 0;
            overflow: hidden;
            transition: max-height 0.3s ease-out;
        }
        .rubric-content.expanded {
            max-height: 500px; /* Adjust as needed, ensure it's large enough for content */
        }
        .modal {
            display: none; /* Hidden by default */
            position: fixed; /* Stay in place */
            z-index: 1000; /* Sit on top */
            left: 0;
            top: 0;
            width: 100%; /* Full width */
            height: 100%; /* Full height */
            overflow: auto; /* Enable scroll if needed */
            background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
            justify-content: center;
            align-items: center;
        }
        .modal-content {
            background-color: #fefefe;
            margin: auto;
            padding: 2.5rem;
            border-radius: 0.75rem;
            box-shadow: 0 8px 16px rgba(0,0,0,0.2);
            width: 90%;
            max-width: 600px;
            color: #2d3748;
        }
        .close-button {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
        }
        .close-button:hover,
        .close-button:focus {
            color: black;
            text-decoration: none;
            cursor: pointer;
        }
    </style>
</head>
<body class="p-6">
    <div class="container mx-auto py-8">
        <h1 class="text-4xl font-bold text-center text-indigo-800 mb-8">The Prompt Engineering Paradox</h1>
        <p class="text-center text-gray-700 mb-10 max-w-2xl mx-auto">Iteratively refine AI prompts for UX tasks, understanding the unintended consequences, biases, and ethical considerations introduced by subtle prompt changes.</p>

        <div id="game-area" class="space-y-8">
            <!-- Game Introduction / Setup -->
            <div id="intro-screen" class="card p-8 text-center">
                <h2 class="text-2xl font-semibold mb-4">Welcome, Prompt Engineer!</h2>
                <p class="mb-6">Your mission: Master the art of prompt engineering by refining AI inputs for UX tasks. Spot biases, ensure clarity, and drive user-centric outcomes.</p>
                <button id="start-game-btn" class="btn-primary px-8 py-3 rounded-lg text-lg font-bold">Start Game</button>
            </div>

            <!-- Main Game Play -->
            <div id="game-play-screen" class="hidden">
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                    <!-- Left Column: Scenario, Prompt Input, AI Output -->
                    <div class="col-span-1 space-y-6">
                        <div class="card p-6">
                            <h3 class="text-xl font-semibold mb-3 text-indigo-700">Current Challenge <span id="round-display" class="text-gray-500 text-base">Round 1</span></h3>
                            <p class="text-gray-800"><span class="font-bold">Scenario:</span> <span id="scenario-text"></span></p>
                            <p class="text-gray-800 mt-2"><span class="font-bold">Task Prompt:</span> <span id="task-prompt-text"></span></p>
                            <p id="modifier-section" class="text-orange-600 mt-2 hidden"><span class="font-bold">Constraint/Modifier:</span> <span id="modifier-text"></span></p>
                        </div>

                        <div class="card p-6">
                            <h3 class="text-xl font-semibold mb-3 text-indigo-700">Your Prompt</h3>
                            <textarea id="prompt-input" class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500" placeholder="Type your prompt here..."></textarea>
                            <button id="submit-prompt-btn" class="btn-primary w-full px-6 py-2 mt-4 rounded-lg font-bold">Submit Prompt to AI</button>
                            <div id="feedback-message" class="feedback-message hidden"></div>
                        </div>

                        <div id="ai-output-section" class="card p-6 hidden">
                            <h3 class="text-xl font-semibold mb-3 text-indigo-700">AI Output</h3>
                            <div id="ai-loading-indicator" class="text-center text-gray-600 italic hidden mt-4">AI is thinking...</div>
                            <pre id="ai-output-display" class="bg-gray-50 p-3 rounded-lg border border-gray-200 text-sm whitespace-pre-wrap mt-2"></pre>
                            <button id="evaluate-btn" class="btn-success w-full px-6 py-2 mt-4 rounded-lg font-bold hidden">Evaluate AI Output</button>
                        </div>
                    </div>

                    <!-- Right Column: Evaluation Rubric, Score, Next Round -->
                    <div class="col-span-1 space-y-6">
                        <div class="card p-6">
                            <h3 class="text-xl font-semibold mb-3 text-indigo-700">Awareness Points: <span id="score-display" class="font-bold text-green-600">0</span></h3>
                            <p class="text-sm text-gray-600">Earn points by identifying nuances, biases, and successfully refining prompts.</p>
                        </div>

                        <div id="evaluation-rubric-card" class="card p-6 hidden">
                            <div class="rubric-header" id="rubric-header">
                                <h3 class="text-xl font-semibold text-indigo-700">UX Evaluation Rubric</h3>
                                <span id="rubric-toggle-icon" class="text-2xl text-gray-600 transition-transform duration-300 transform rotate-0">&#9660;</span>
                            </div>
                            <div id="rubric-content" class="rubric-content">
                                <p class="text-gray-700 text-sm mt-3 mb-4">Select all issues present in the AI's output:</p>
                                <div id="rubric-checklist" class="space-y-2 text-sm">
                                    <!-- Rubric checkboxes will be inserted here by JS -->
                                </div>
                                <button id="submit-evaluation-btn" class="btn-warning w-full px-6 py-2 mt-4 rounded-lg font-bold hidden">Submit Evaluation</button>
                            </div>
                        </div>

                        <div class="flex justify-end mt-6">
                            <button id="next-round-btn" class="btn-secondary px-8 py-3 rounded-lg text-lg font-bold hidden">Next Round</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Debrief Modal -->
    <div id="debrief-modal" class="modal">
        <div class="modal-content">
            <span class="close-button" id="close-debrief-modal">&times;</span>
            <h2 class="text-3xl font-bold text-indigo-800 mb-4">Round Complete!</h2>
            <h3 class="text-xl font-semibold mb-3 text-gray-700">Debrief & Learnings</h3>
            <div id="debrief-content" class="mb-6 text-gray-700">
                <p>This is where we'll discuss:</p>
                <ul class="list-disc list-inside mt-2 space-y-1">
                    <li>How different prompts led to varied AI outputs.</li>
                    <li>Common pitfalls like implicit bias or literal interpretations.</li>
                    <li>Best practices for refining your prompts.</li>
                    <li>The crucial role of the human-in-the-loop for ethical AI.</li>
                </ul>
                <p class="mt-4 font-semibold text-lg">Your final score for this round: <span id="modal-round-score" class="text-green-600"></span> Awareness Points!</p>
            </div>
            <div class="flex justify-end">
                <button id="modal-next-round-btn" class="btn-primary px-6 py-2 rounded-lg font-bold">Continue to Next Challenge</button>
            </div>
        </div>
    </div>

    <script>
        // Data for Scenarios, Modifiers, and Rubric
        const scenarios = [
            {
                id: 1,
                scenario: "A tech company is developing a new smart home security system. They need to understand the initial concerns and expectations of **first-time homeowners** regarding home safety and their willingness to adopt new technology.",
                taskPrompt: "Generate 5 open-ended user interview questions for this scenario.",
                defaultPrompt: "Generate 5 open-ended user interview questions about smart home security for first-time homeowners.",
                modifier: null,
                outputs: [
                    {
                        promptKeywords: [], // Default output if no specific keywords match
                        content: `1. What do you think about home security?
2. Do you feel safe in your home?
3. What features would you like in a security system?
4. How much would you pay for security?
5. Tell me about your home.`,
                        flaws: ["completeness", "not open-ended", "not user-centric", "lacks nuance"]
                    },
                    {
                        promptKeywords: ["open-ended", "first-time homeowners", "concerns", "expectations"],
                        content: `1. As a first-time homeowner, what are your initial thoughts or concerns about securing your new property?
2. Can you describe any previous experiences you've had with home security, and what worked or didn't work well for you?
3. What aspects of home safety give you the most peace of mind, and what causes you the most anxiety?
4. If you imagine your ideal smart home security system, what key functionalities or features come to mind?
5. How do you envision a smart home security system integrating into your daily routines and lifestyle?`,
                        flaws: [] // This is a good output for the scenario
                    },
                    {
                        promptKeywords: ["gender stereotypes", "male", "female"],
                        content: `1. What kind of security do *you* feel a woman needs in her new home?
2. How do you plan to protect your family?
3. What smart features would help a busy mom?
4. Do you trust technology to protect your home?
5. How important is it for *him* to feel safe?`,
                        flaws: ["bias"]
                    }
                ]
            },
            {
                id: 2,
                scenario: "A mental wellness app is looking to expand its reach. They need to identify different **user archetypes** to tailor features and marketing, especially considering diverse motivations for seeking mental well-being support.",
                taskPrompt: "Create 3 distinct user persona archetypes for a mental wellness app.",
                defaultPrompt: "Generate 3 user persona archetypes for a mental wellness app focusing on user motivations.",
                modifier: "Ensure the output avoids gender stereotypes.",
                outputs: [
                    {
                        promptKeywords: [],
                        content: `Persona 1: Busy Professional Bill (30s, male, stressed by work). Motivation: Stress relief.
Persona 2: Anxious Annie (20s, female, worried about future). Motivation: Anxiety management.
Persona 3: Social Sarah (20s, female, wants to connect). Motivation: Community.`,
                        flaws: ["bias", "lack of depth"]
                    },
                    {
                        promptKeywords: ["avoid gender stereotypes", "motivations", "diverse"],
                        content: `Persona 1: The Burnout Battler (30s, career-focused individual). Motivation: Needs structured tools to manage chronic work-related stress and prevent burnout. Seeks efficiency and measurable progress.
Persona 2: The Self-Discovery Seeker (20s, student/early career). Motivation: Explores self-awareness and emotional regulation. Values expressive outlets and insights into personal growth.
Persona 3: The Connection Craver (40s, community-oriented individual). Motivation: Desires supportive social interactions and shared experiences to combat loneliness. Seeks a sense of belonging and practical social skills.`,
                        flaws: ["completeness"] // Good, but could be more specific on demographics beyond age/career stage
                    }
                ]
            },
            {
                id: 3,
                scenario: "A mobile banking app wants to improve user satisfaction by addressing key pain points. They have collected 20 user reviews and need to identify recurring issues, particularly those related to **trust and privacy**.",
                taskPrompt: "Summarize key insights from 20 user reviews for a mobile banking app.",
                defaultPrompt: "Summarize pain points related to trust and privacy from mobile banking app user reviews.",
                modifier: "Focus on users in rural areas with limited internet access.",
                outputs: [
                    {
                        promptKeywords: [],
                        content: `Insights:
- Users want better UI.
- They like the speed.
- Some mentioned glitches.`,
                        flaws: ["completeness", "not focused on trust/privacy", "not actionable"]
                    },
                    {
                        promptKeywords: ["rural", "limited internet", "trust", "privacy", "security"],
                        content: `Key Insights (Trust & Privacy, Rural Users):
1.  **Data Security Anxiety:** Users, especially those in rural areas, express significant concern about the security of their financial data given potential for less secure networks. Many specifically asked about offline access features.
2.  **Privacy of Location Data:** Several reviews from rural users highlighted discomfort with the app requesting location permissions, fearing it could be used to track their isolated residences.
3.  **Transparency of Data Usage:** A recurring pain point was the lack of clear communication on how their data (transaction history, personal info) is used, leading to distrust.
4.  **Reliability Concerns with Poor Connectivity:** While not strictly trust/privacy, slow app performance or transaction failures due to limited internet access led to a lack of trust in the app's reliability for critical financial tasks. Users questioned if their transactions were truly private or just "stuck" due to network issues.`,
                        flaws: [] // This is a good output for the scenario
                    }
                ]
            },
             {
                id: 4,
                scenario: "A financial institution is launching a new savings product designed to help **low-income individuals** build financial stability. They need to develop marketing copy that is both appealing and ethically sound.",
                taskPrompt: "Generate marketing copy for a financial product.",
                defaultPrompt: "Write marketing copy for a financial product aimed at low-income individuals.",
                isBonus: true,
                outputs: [
                    {
                        promptKeywords: [],
                        content: `🤑 Get rich quick! Our new loan product offers instant cash, no questions asked. Low-income, no problem! Sign up today and solve all your money worries!`,
                        flaws: ["ethical-implications", "unexpected-harm", "bias", "lacks nuance"]
                    },
                    {
                        promptKeywords: ["ethical", "responsible", "transparent", "support", "empower"],
                        content: `Empower your financial future. Our accessible savings account is designed to help you build stability, offering transparent terms and dedicated support. Start saving with confidence, at your own pace.`,
                        flaws: []
                    }
                ]
            }
        ];

        const rubricCriteria = [
            { id: "bias", label: "Bias (Gender, Racial, Socioeconomic, Geographic)" },
            { id: "ethical-implications", label: "Ethical Implications / Potential Harm" },
            { id: "completeness", label: "Completeness (Does it address all parts of the task?)" },
            { id: "clarity", label: "Clarity (Is the output easy to understand?)" },
            { id: "actionability", label: "Actionability (Can UX professionals use this output directly?)" },
            { id: "user-centricity", label: "User-Centricity (Is it focused on user needs/perspectives?)" },
            { id: "inclusivity", label: "Inclusivity (Does it consider diverse user groups?)" },
            { id: "alignment", label: "Alignment with Task (Does it actually answer the prompt?)" },
            { id: "nuance", label: "Nuance (Does it go beyond superficial understanding?)" },
            { id: "unexpected-harm", label: "Unexpected Harm (Did the prompt or output create unforeseen negative consequences?)" }
        ];

        // Game state variables
        let currentScenario = null;
        let currentPrompt = "";
        let currentAIOutput = null; // Stores the object {content, flaws}
        let currentRound = 0;
        let awarenessPoints = 0;
        let promptIterations = 0;
        const maxPromptIterations = 3; // Max attempts per scenario before moving on
        let currentRoundPoints = 0;

        // HTML Element references
        const introScreen = document.getElementById('intro-screen');
        const gamePlayScreen = document.getElementById('game-play-screen');
        const startGameBtn = document.getElementById('start-game-btn');
        const scenarioText = document.getElementById('scenario-text');
        const taskPromptText = document.getElementById('task-prompt-text'); // New element for task prompt
        const modifierText = document.getElementById('modifier-text');
        const modifierSection = document.getElementById('modifier-section');
        const promptInput = document.getElementById('prompt-input');
        const aiOutputDisplay = document.getElementById('ai-output-display');
        const aiOutputSection = document.getElementById('ai-output-section');
        const aiLoadingIndicator = document.getElementById('ai-loading-indicator');
        const scoreDisplay = document.getElementById('score-display');
        const feedbackMessage = document.getElementById('feedback-message');
        const rubricChecklist = document.getElementById('rubric-checklist');
        const evaluationRubricCard = document.getElementById('evaluation-rubric-card');
        const nextRoundBtn = document.getElementById('next-round-btn');
        const submitPromptBtn = document.getElementById('submit-prompt-btn');
        const evaluateBtn = document.getElementById('evaluate-btn');
        const submitEvaluationBtn = document.getElementById('submit-evaluation-btn');
        const roundDisplay = document.getElementById('round-display');
        const rubricHeader = document.getElementById('rubric-header');
        const rubricContent = document.getElementById('rubric-content');
        const rubricToggleIcon = document.getElementById('rubric-toggle-icon');

        // Debrief Modal elements
        const debriefModal = document.getElementById('debrief-modal');
        const closeDebriefModalBtn = document.getElementById('close-debrief-modal');
        const debriefContent = document.getElementById('debrief-content');
        const modalRoundScore = document.getElementById('modal-round-score');
        const modalNextRoundBtn = document.getElementById('modal-next-round-btn');

        // --- Game Logic Functions ---

        function initGame() {
            startGameBtn.addEventListener('click', () => {
                introScreen.classList.add('hidden');
                gamePlayScreen.classList.remove('hidden');
                nextRound();
            });

            submitPromptBtn.addEventListener('click', submitPrompt);
            evaluateBtn.addEventListener('click', () => {
                // Expand rubric and show submit evaluation button
                evaluationRubricCard.classList.remove('hidden'); // Ensure rubric card is visible
                rubricContent.classList.add('expanded');
                rubricToggleIcon.classList.add('rotate-180');
                evaluateBtn.classList.add('hidden');
                submitEvaluationBtn.classList.remove('hidden');
            });
            submitEvaluationBtn.addEventListener('click', submitEvaluation);
            nextRoundBtn.addEventListener('click', nextRound);

            rubricHeader.addEventListener('click', toggleRubric);
            closeDebriefModalBtn.addEventListener('click', () => debriefModal.style.display = 'none');
            modalNextRoundBtn.addEventListener('click', () => {
                debriefModal.style.display = 'none';
                nextRound();
            });

            renderRubricChecklist();
            updateUI(); // Initial UI update
        }

        function toggleRubric() {
            rubricContent.classList.toggle('expanded');
            rubricToggleIcon.classList.toggle('rotate-180');
        }

        function updateUI() {
            scoreDisplay.textContent = awarenessPoints;
            roundDisplay.textContent = `Round ${currentRound}`;

            if (currentScenario) {
                scenarioText.textContent = currentScenario.scenario; // Use the new 'scenario' field
                taskPromptText.textContent = currentScenario.taskPrompt; // Display the task prompt
                if (currentScenario.modifier) {
                    modifierText.textContent = currentScenario.modifier;
                    modifierSection.classList.remove('hidden');
                } else {
                    modifierSection.classList.add('hidden');
                }
            } else {
                scenarioText.textContent = "Loading new challenge...";
                taskPromptText.textContent = ""; // Clear task prompt
                modifierSection.classList.add('hidden');
            }

            // Pre-fill prompt input only if it's a new round (or first time loading) and no existing prompt
            if (promptIterations === 0 && !currentPrompt) {
                promptInput.value = currentScenario ? currentScenario.defaultPrompt : "";
            } else {
                promptInput.value = currentPrompt;
            }


            // Manage visibility based on game state
            aiOutputSection.classList.add('hidden'); // Hide AI output by default
            evaluationRubricCard.classList.add('hidden'); // Hide rubric card by default
            evaluateBtn.classList.add('hidden');
            submitEvaluationBtn.classList.add('hidden');
            nextRoundBtn.classList.add('hidden');
            aiLoadingIndicator.classList.add('hidden'); // Hide loading indicator by default

            // Reset rubric checkboxes
            rubricChecklist.querySelectorAll('input[type="checkbox"]').forEach(checkbox => checkbox.checked = false);

            feedbackMessage.classList.add('hidden');
            feedbackMessage.textContent = '';

            if (currentAIOutput) {
                aiOutputDisplay.textContent = currentAIOutput.content;
                aiOutputSection.classList.remove('hidden');
                evaluationRubricCard.classList.remove('hidden'); // Show rubric card once AI output is ready
                evaluateBtn.classList.remove('hidden'); // Show evaluate button
                rubricContent.classList.remove('expanded'); // Collapse rubric on new AI output
                rubricToggleIcon.classList.remove('rotate-180');
            } else {
                aiOutputDisplay.textContent = "";
            }

            if (promptIterations === maxPromptIterations && currentAIOutput) {
                submitPromptBtn.classList.add('hidden'); // Hide submit if max attempts reached and output generated
                nextRoundBtn.classList.remove('hidden'); // Show next round button
                showFeedback("Maximum refinement attempts reached. Evaluate the output and proceed to the next round.", 'success');
            } else if (!currentAIOutput) { // Awaiting first prompt or new prompt
                submitPromptBtn.classList.remove('hidden');
            } else { // AI output is present, awaiting evaluation or further refinement
                submitPromptBtn.classList.remove('hidden'); // Allow refinement
            }
        }

        function showFeedback(message, type) {
            feedbackMessage.textContent = message;
            feedbackMessage.classList.remove('hidden', 'feedback-success', 'feedback-error');
            if (type === 'success') {
                feedbackMessage.classList.add('feedback-success');
            } else if (type === 'error') {
                feedbackMessage.classList.add('feedback-error');
            }
        }

        function nextRound() {
            currentRoundPoints = 0; // Reset points for the new round
            currentRound++;
            promptIterations = 0;

            const availableScenarios = scenarios.filter(s => !s.isBonus || currentRound === scenarios.length); // Only show bonus in last round
            if (currentRound > availableScenarios.length) {
                // Game over, maybe show a final score screen
                alert(`Game Over! Your total Awareness Points: ${awarenessPoints}`);
                // For simplicity, reset game for now
                awarenessPoints = 0;
                currentRound = 0;
                initGame(); // Restart or provide a different end game experience
                return;
            }

            currentScenario = availableScenarios[currentRound - 1]; // Get scenario for the current round
            currentPrompt = ""; // Reset prompt for new round
            currentAIOutput = null; // Clear previous AI output

            updateUI();
        }

        function submitPrompt() {
            currentPrompt = promptInput.value.trim();
            if (!currentPrompt) {
                showFeedback("Please enter a prompt.", 'error');
                return;
            }

            promptIterations++;
            submitPromptBtn.classList.add('hidden'); // Hide submit button while thinking
            aiOutputSection.classList.remove('hidden'); // Show AI output section to display loading
            aiOutputDisplay.textContent = ""; // Clear previous output
            aiLoadingIndicator.classList.remove('hidden'); // Show loading indicator
            evaluateBtn.classList.add('hidden'); // Hide evaluate button during loading
            submitEvaluationBtn.classList.add('hidden'); // Hide submit evaluation button during loading
            evaluationRubricCard.classList.add('hidden'); // Hide rubric card during loading

            setTimeout(() => { // Simulate AI thinking time
                aiLoadingIndicator.classList.add('hidden'); // Hide loading indicator
                simulateAIOutput(currentPrompt);
                updateUI();
                showFeedback(`Prompt submitted. Now evaluate the AI's response. (${maxPromptIterations - promptIterations} attempts remaining)`, 'success');
                // Auto-scroll to AI output section for better UX
                aiOutputSection.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }, 1500);
        }

        function simulateAIOutput(prompt) {
            const lowerCasePrompt = prompt.toLowerCase();
            let matchedOutput = null;

            // Try to find the best match based on keywords
            // Prioritize specific keywords from scenario.outputs
            for (const output of currentScenario.outputs) {
                if (output.promptKeywords && output.promptKeywords.length > 0) {
                    const allKeywordsMatch = output.promptKeywords.every(keyword => lowerCasePrompt.includes(keyword.toLowerCase()));
                    if (allKeywordsMatch) {
                        matchedOutput = output;
                        break; // Found the best match
                    }
                }
            }

            // If no specific keyword match, use the default (first) output, or the one with empty keywords
            if (!matchedOutput) {
                matchedOutput = currentScenario.outputs.find(output => output.promptKeywords.length === 0) || currentScenario.outputs[0];
            }

            currentAIOutput = matchedOutput;
        }

        function renderRubricChecklist() {
            rubricChecklist.innerHTML = '';
            rubricCriteria.forEach(criterion => {
                const div = document.createElement('div');
                div.className = 'flex items-center space-x-2';
                div.innerHTML = `
                    <input type="checkbox" id="rubric-${criterion.id}" value="${criterion.id}" class="rounded text-indigo-600 focus:ring-indigo-500">
                    <label for="rubric-${criterion.id}" class="text-gray-700">${criterion.label}</label>
                `;
                rubricChecklist.appendChild(div);
            });
        }

        function submitEvaluation() {
            const selectedFlaws = Array.from(rubricChecklist.querySelectorAll('input[type="checkbox"]:checked'))
                                      .map(checkbox => checkbox.value);

            let correctIdentified = 0;
            let missedFlaws = 0;
            let falsePositives = 0;

            // Correctly identified flaws
            if (currentAIOutput && currentAIOutput.flaws) {
                currentAIOutput.flaws.forEach(actualFlaw => {
                    if (selectedFlaws.includes(actualFlaw)) {
                        correctIdentified++;
                    } else {
                        missedFlaws++;
                    }
                });
            }

            // False positives (selected a flaw that wasn't there)
            selectedFlaws.forEach(selectedFlaw => {
                if (currentAIOutput && !currentAIOutput.flaws.includes(selectedFlaw)) {
                    falsePositives++;
                }
            });

            // Scoring logic
            let roundScore = 0;
            roundScore += correctIdentified * 10; // 10 points for each correctly identified flaw
            roundScore -= missedFlaws * 5;      // -5 points for each missed flaw
            roundScore -= falsePositives * 3;   // -3 points for each false positive

            // Bonus for perfect identification
            if (currentAIOutput && currentAIOutput.flaws.length > 0 && correctIdentified === currentAIOutput.flaws.length && missedFlaws === 0 && falsePositives === 0) {
                roundScore += 20; // Bonus for catching all and only the real flaws
                showFeedback("Excellent! You identified all issues precisely. (+20 Bonus!)", 'success');
            } else if (correctIdentified > 0) {
                 showFeedback("Good job! You identified some key issues.", 'success');
            } else {
                showFeedback("Hmm, revisit the output. There might be more (or fewer) issues.", 'error');
            }

            // If prompt was refined and led to better output, also award points.
            if (currentAIOutput && currentAIOutput.flaws.length === 0 && promptIterations > 0) { // If it's a perfect output and refinement was attempted
                roundScore += 15; // Bonus for successfully prompting to a perfect output
                showFeedback("Your prompt engineering successfully led to a clean output! (+15 Bonus)", 'success');
            }

            currentRoundPoints = Math.max(0, roundScore); // Ensure score doesn't go negative for the round
            awarenessPoints += currentRoundPoints;

            updateUI();
            showDebriefModal();

            // Next steps for buttons
            submitPromptBtn.classList.add('hidden'); // Hide submit until next round/refinement
            evaluateBtn.classList.add('hidden');
            submitEvaluationBtn.classList.add('hidden');
            nextRoundBtn.classList.remove('hidden'); // Show next round button
        }

        function showDebriefModal() {
            modalRoundScore.textContent = currentRoundPoints;
            debriefModal.style.display = 'flex'; // Use flex to center
        }

        // Initialize the game when the DOM is fully loaded
        document.addEventListener('DOMContentLoaded', initGame);
    </script>
</body>
</html>
