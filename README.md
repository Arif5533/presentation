<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Deep Learning for Parkinson's Detection</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@100;400;600;700;900&display=swap" rel="stylesheet">
    <style>
        :root {
            --color-primary: #8B5CF6; /* Purple */
            --color-secondary: #3B82F6; /* Blue */
            --color-dark-bg: #0A0A0A;
            --color-light-text: #E5E7EB;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--color-dark-bg);
            color: var(--color-light-text);
            overflow: hidden; /* Hide scrollbars for presentation mode */
        }
        
        /* Deep Gradient Background Effect */
        #presentation-container {
            position: relative;
            width: 100vw;
            height: 100vh;
            background: radial-gradient(circle at 50% 50%, #1a1b26 0%, #0a0a0a 100%);
        }

        .slide {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: none;
            padding: 4rem;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: opacity 0.5s ease-in-out;
            opacity: 0;
        }

        .slide.active {
            display: flex;
            opacity: 1;
        }

        /* Glassmorphism Slide Content Container */
        .slide-content-box {
            background-color: rgba(25, 25, 35, 0.6); 
            backdrop-filter: blur(8px);
            border: 1px solid rgba(139, 92, 246, 0.2);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.3);
            border-radius: 20px;
            padding: 2.5rem;
            max-width: 90%;
            width: 100%;
            max-height: 85%;
            overflow-y: auto;
        }

        /* Custom Title Styles */
        .main-title {
            font-size: 2.5rem;
            font-weight: 800;
            line-height: 1.1;
            background: linear-gradient(90deg, var(--color-secondary), var(--color-primary));
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 10px rgba(139, 92, 246, 0.3);
        }

        /* Custom List/Bullet Styles */
        .slide-content-box ul {
            list-style-type: none;
            padding-left: 0;
        }

        .slide-content-box li {
            position: relative;
            padding-left: 1.5rem;
            margin-bottom: 0.75rem;
            font-size: 1.1rem;
            line-height: 1.4;
        }
        
        .slide-content-box li::before {
            content: '•';
            color: var(--color-primary);
            font-size: 1.5rem;
            position: absolute;
            left: 0;
            top: -2px;
            font-weight: 900;
        }
        
        .accent-text {
            color: var(--color-secondary);
            font-weight: 600;
        }

        /* Navigation Buttons */
        .nav-button {
            background: linear-gradient(90deg, var(--color-secondary), var(--color-primary));
            color: white;
            padding: 0.5rem 1.5rem;
            border-radius: 9999px;
            font-weight: 600;
            transition: transform 0.2s;
            box-shadow: 0 4px 10px rgba(139, 92, 246, 0.4);
        }

        .nav-button:hover {
            transform: scale(1.05);
            box-shadow: 0 6px 15px rgba(139, 92, 246, 0.6);
        }

        .nav-button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            box-shadow: none;
        }
    </style>
</head>
<body>

<div id="presentation-container">

    <!-- Slide 1: Title Slide -->
    <div id="slide-1" class="slide">
        <div class="slide-content-box text-center p-10">
            <h1 class="main-title mb-4" style="font-size: 3.5rem;">Deep Learning Approaches for Early Detection of Parkinson’s Disease</h1>
            <h2 class="text-2xl font-semibold mb-8 text-gray-300">Using Unpaired Handwriting and Voice Data</h2>
            <div class="text-lg text-gray-400 space-y-2">
                <p>👨‍🎓 <span class="text-white font-medium">Arif Hossen (002930191)</span></p>
                <p>👨‍🎓 <span class="text-white font-medium">Sabarno Dutta (002660294)</span></p>
                <div class="mt-6 pt-4 border-t border-gray-700">
                    <p class="font-light">CSC/DSCI 6851 – Introduction to Deep Learning (Fall 2025)</p>
                    <p class="font-light">Department of Computer Science, Georgia State University</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 2: Background & Motivation -->
    <div id="slide-2" class="slide">
        <div class="slide-content-box">
            <h2 class="text-3xl font-bold mb-6 border-b border-purple-500 pb-2">1. Background & Motivation</h2>
            <ul class="space-y-4">
                <li><strong class="accent-text">The Challenge:</strong> Parkinson’s Disease (<span class="text-white">PD</span>) is a progressive disorder impacting movement, <span class="text-white">handwriting</span>, and <span class="text-white">speech</span>.</li>
                <li><strong class="accent-text">The Problem:</strong> Early symptoms are subtle, subjective, and hard to detect, leading to delayed diagnosis.</li>
                <li><strong class="accent-text">The AI Solution:</strong> Develop a system for <span class="text-white">AI-driven early screening</span> using easily collected, non-invasive signals (handwriting and voice).</li>
                <li><strong class="accent-text">The Impact:</strong> Faster, data-driven clinical support and improved quality of life through early intervention.</li>
            </ul>
        </div>
    </div>

    <!-- Slide 3: Problem Statement -->
    <div id="slide-3" class="slide">
        <div class="slide-content-box">
            <h2 class="text-3xl font-bold mb-6 border-b border-purple-500 pb-2">2. Problem Statement: Unpaired Multimodal Fusion</h2>
            <ul class="space-y-4">
                <li><strong class="accent-text">Goal:</strong> Detect early PD signs using handwriting and voice data where samples <span class="text-white">are NOT from the same individuals (unpaired)</span>.</li>
                <li><strong class="accent-text">The Limitation of Past Work:</strong> Most studies use a single modality, limiting diagnostic power, or require perfectly paired data.</li>
                <li><strong class="accent-text">Our Novelty:</strong> Combine information from two separate modalities by fusing their intelligence:
                    <ul class="list-disc ml-8 mt-2 space-y-2">
                        <li><span class="font-semibold text-gray-300">Feature-level fusion:</span> Concatenate the learned feature embeddings.</li>
                        <li><span class="font-semibold text-gray-300">Decision-level ensemble:</span> Combine the final prediction probabilities.</li>
                    </ul>
                </li>
                <li>This approach creates a <span class="text-white font-semibold">single, stronger diagnostic model</span> scalable to real-world, heterogenous datasets.</li>
            </ul>
        </div>
    </div>

    <!-- Slide 4: Related Work (Table) -->
    <div id="slide-4" class="slide">
        <div class="slide-content-box">
            <h2 class="text-3xl font-bold mb-6 border-b border-purple-500 pb-2">3. Related Work & Research Gap</h2>
            <div class="overflow-x-auto">
                <table class="min-w-full divide-y divide-gray-700 rounded-lg overflow-hidden text-sm md:text-base">
                    <thead class="bg-gray-800">
                        <tr>
                            <th class="px-4 py-3 text-left font-semibold text-gray-300">Study</th>
                            <th class="px-4 py-3 text-left font-semibold text-gray-300">Modality</th>
                            <th class="px-4 py-3 text-left font-semibold text-gray-300">Method</th>
                            <th class="px-4 py-3 text-left font-semibold text-gray-300">Limitation / Gap</th>
                        </tr>
                    </thead>
                    <tbody class="divide-y divide-gray-800">
                        <tr class="hover:bg-gray-800/50">
                            <td class="px-4 py-3">Drotár et al. (2016)</td>
                            <td class="px-4 py-3">Handwriting</td>
                            <td class="px-4 py-3">CNN on spiral drawings</td>
                            <td class="px-4 py-3">No audio modality</td>
                        </tr>
                        <tr class="hover:bg-gray-800/50">
                            <td class="px-4 py-3">Tsanas et al. (2014)</td>
                            <td class="px-4 py-3">Voice</td>
                            <td class="px-4 py-3">Regression on jitter/shimmer</td>
                            <td class="px-4 py-3">No visual features</td>
                        </tr>
                        <tr class="hover:bg-gray-800/50">
                            <td class="px-4 py-3">Pereira et al. (2022)</td>
                            <td class="px-4 py-3">Handwriting</td>
                            <td class="px-4 py-3">Deep CNNs</td>
                            <td class="px-4 py-3">Overfitting on small datasets</td>
                        </tr>
                        <tr class="hover:bg-gray-800/50 bg-purple-900/20">
                            <td class="px-4 py-3 font-semibold text-purple-400" colspan="4">
                                Research Gap: Lack of scalable, <span class="text-white">unpaired multimodal</span> approaches integrating handwriting and voice features.
                            </td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- Slide 5: Proposed Method – Overview & Components -->
    <div id="slide-5" class="slide">
        <div class="slide-content-box">
            <h2 class="text-3xl font-bold mb-6 border-b border-purple-500 pb-2">4. Proposed Unpaired Multimodal Framework</h2>
            <div class="grid md:grid-cols-2 gap-6 items-center">
                <div class="space-y-4">
                    <p class="text-xl font-semibold accent-text">System Architecture:</p>
                    <ul class="space-y-3 ml-0">
                        <li><strong class="text-white">Handwriting Branch:</strong> Fine-tune advanced Vision Models (<span class="accent-text">EfficientNet-B3</span>, Inception V4 / ViT).</li>
                        <li><strong class="text-white">Voice Branch:</strong> Train classical models (XGBoost, SVM, RF) on comprehensive acoustic features.</li>
                        <li><strong class="text-white">Fusion Strategy (Unpaired):</strong> Implement and compare <span class="accent-text">Feature-Level Concatenation</span> vs. <span class="accent-text">Decision-Level Ensemble</span>.</li>
                        <li><strong class="text-white">Milestone Focus:</strong> Initial Feature-level fusion and Decision-level ensemble.</li>
                        <li><strong class="text-white">Final Project Goal:</strong> Develop an <span class="text-white font-extrabold">Adaptive Fusion Network (AFN)</span>.</li>
                    </ul>
                </div>
                <div class="p-4 bg-gray-900/70 border border-gray-700 rounded-lg text-center">
                    <p class="text-gray-400 italic mb-2">Placeholder: Pipeline Diagram</p>
                    <p class="text-gray-500 text-sm">Two independent feature streams converge at the fusion layer.</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Slide 6: Dataset and Preprocessing -->
    <div id="slide-6" class="slide">
        <div class="slide-content-box">
            <h2 class="text-3xl font-bold mb-6 border-b border-purple-500 pb-2">5. Dataset & Preprocessing</h2>

            <div class="grid grid-cols-2 gap-6">
                <div>
                    <h3 class="text-xl font-bold accent-text mb-3">Handwriting Modality (Vision)</h3>
                    <ul class="space-y-2 text-sm">
                        <li>Dataset: <span class="text-white">NIATS Handwriting Dataset</span> (spiral and wave drawings).</li>
                        <li>Preprocessing: <span class="text-white">Resize to 300x300</span>, Normalization, Augmentation.</li>
                        <li>Balancing: <span class="text-white">WeightedRandomSampler</span> to address class imbalance.</li>
                    </ul>
                </div>
                <div>
                    <h3 class="text-xl font-bold accent-text mb-3">Voice Modality (Acoustic)</h3>
                    <ul class="space-y-2 text-sm">
                        <li>Dataset: <span class="text-white">UCI / PPMI Voice Dataset</span> (MFCC, Jitter, Shimmer, Pitch).</li>
                        <li>Preprocessing: <span class="text-white">StandardScaler</span> for feature scaling, Denoising.</li>
                        <li>Balancing: <span class="text-white">SMOTE</span> technique for synthesizing minority class samples.</li>
                    </ul>
                </div>
            </div>
            <p class="mt-4 border-t pt-4 text-center text-gray-400 italic">Key Point: <span class="font-semibold text-white">Independent Train/Test Splits</span> are maintained to simulate the unpaired environment.</p>
            <div class="mt-4 p-3 bg-gray-900/70 border border-gray-700 rounded-lg text-center">
                <p class="text-gray-400 italic mb-1">Placeholder: Sample Handwriting Images & Waveforms</p>
                <p class="text-gray-500 text-xs">Visualizing the raw data inputs for the two separate streams.</p>
            </div>
        </div>
    </div>

    <!-- Slide 7: Experimental Setup & Results (Milestone) -->
    <div id="slide-7" class="slide">
        <div class="slide-content-box">
            <h2 class="text-3xl font-bold mb-6 border-b border-purple-500 pb-2">6. Experimental Setup & Results (Milestone)</h2>

            <div class="grid grid-cols-2 gap-6">
                <div>
                    <h3 class="text-xl font-bold accent-text mb-3">Handwriting Branch Setup</h3>
                    <ul class="space-y-2 text-sm">
                        <li>Model: <span class="text-white">EfficientNet-B3</span> (Fine-tuned).</li>
                        <li>Optimizer: AdamW (<span class="text-white">OneCycleLR</span> scheduling).</li>
                        <li>Loss/Epochs: Cross-Entropy, 40 epochs.</li>
                    </ul>
                </div>
                <div>
                    <h3 class="text-xl font-bold accent-text mb-3">Voice/Fusion Setup</h3>
                    <ul class="space-y-2 text-sm">
                        <li>Voice Model: <span class="text-white">XGBoost</span> (GPU-enabled), 200 trees, lr=0.05.</li>
                        <li>Fusion: Late fusion (weighted <span class="text-white">0.6 Image / 0.4 Voice</span>).</li>
                        <li>Explainability: <span class="text-white">SHAP</span> values for feature importance.</li>
                    </ul>
                </div>
            </div>
            <h3 class="text-xl font-bold accent-text mt-6 mb-3">Metrics & Results</h3>
            <ul class="space-y-2 text-sm">
                <li><strong class="text-white">Metrics:</strong> Accuracy, F1-Score, ROC-AUC, Precision, Recall.</li>
                <li><strong class="text-white">Milestone Status:</strong> Preliminary results available for individual modalities.</li>
            </ul>
            <div class="mt-4 p-3 bg-gray-900/70 border border-gray-700 rounded-lg text-center">
                <p class="text-gray-400 italic mb-1">Placeholder: Confusion Matrix & ROC Curve</p>
                <p class="text-gray-500 text-xs">Visualizing early detection performance.</p>
            </div>
        </div>
    </div>

    <!-- Slide 8: Next Steps and Conclusion -->
    <div id="slide-8" class="slide">
        <div class="slide-content-box">
            <h2 class="text-3xl font-bold mb-6 border-b border-purple-500 pb-2">7. Plan for Next Steps (Milestone & Final)</h2>

            <div class="grid grid-cols-2 gap-6">
                <div>
                    <h3 class="text-xl font-bold accent-text mb-3">Milestone Immediate Next Steps</h3>
                    <ul class="space-y-2 text-sm">
                        <li>Develop the core <span class="text-white">Adaptive Fusion Network (AFN)</span>.</li>
                        <li>Test ViT/Inception V4 variants against <span class="text-white">EfficientNet-B3</span>.</li>
                        <li>Finalize and visualize Feature-level vs <span class="text-white">Decision-level fusion</span> results.</li>
                    </ul>
                </div>
                <div>
                    <h3 class="text-xl font-bold accent-text mb-3">Final Project Extensions</h3>
                    <ul class="space-y-2 text-sm">
                        <li>Implement <span class="text-white">Domain Adaptation / Contrastive Learning</span> for better feature alignment.</li>
                        <li>Expand dataset cross-validation and <span class="text-white">XAI interpretability</span> (Grad-CAM, SHAP).</li>
                        <li>Finalize paper and deliver comprehensive comparison analysis.</li>
                    </ul>
                </div>
            </div>

            <h2 class="text-3xl font-bold mt-8 mb-6 border-b border-purple-500 pb-2">8. Conclusion</h2>
            <p class="text-lg text-gray-300">
                We propose a robust <span class="accent-text">unpaired multimodal deep learning system</span> for PD detection, combining state-of-the-art vision models and acoustic classifiers. This approach provides a <span class="text-white font-semibold">scalable and clinically relevant</span> diagnostic tool by leveraging the complementarity of handwriting and voice signals without requiring paired patient data.
            </p>
        </div>
    </div>


    <!-- Navigation Controls -->
    <div class="absolute bottom-6 right-8 flex space-x-4">
        <button id="prevBtn" class="nav-button flex items-center" disabled>
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor" class="w-5 h-5 mr-1">
                <path stroke-linecap="round" stroke-linejoin="round" d="M10.5 19.5 3 12m0 0 7.5-7.5M3 12h18" />
            </svg>
            Previous
        </button>
        <button id="nextBtn" class="nav-button flex items-center">
            Next
            <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="2.5" stroke="currentColor" class="w-5 h-5 ml-1">
                <path stroke-linecap="round" stroke-linejoin="round" d="M13.5 4.5 21 12m0 0-7.5 7.5M21 12H3" />
            </svg>
        </button>
    </div>
</div>

<script>
    const slides = document.querySelectorAll('.slide');
    let currentSlide = 0;
    const prevBtn = document.getElementById('prevBtn');
    const nextBtn = document.getElementById('nextBtn');
    
    // Set total slides for the navigation
    const totalSlides = slides.length; 

    function showSlide(index) {
        slides.forEach((slide, i) => {
            slide.classList.remove('active');
            if (i === index) {
                slide.classList.add('active');
            }
        });
        currentSlide = index;
        updateButtons();
    }

    function updateButtons() {
        prevBtn.disabled = currentSlide === 0;
        nextBtn.disabled = currentSlide === totalSlides - 1;
    }

    prevBtn.addEventListener('click', () => {
        if (currentSlide > 0) {
            showSlide(currentSlide - 1);
        }
    });

    nextBtn.addEventListener('click', () => {
        if (currentSlide < totalSlides - 1) {
            showSlide(currentSlide + 1);
        }
    });

    document.addEventListener('keydown', (e) => {
        if (e.key === 'ArrowRight' || e.key === ' ') {
            if (currentSlide < totalSlides - 1) {
                e.preventDefault();
                showSlide(currentSlide + 1);
            }
        } else if (e.key === 'ArrowLeft') {
            if (currentSlide > 0) {
                e.preventDefault();
                showSlide(currentSlide - 1);
            }
        }
    });

    // Initialize presentation
    showSlide(currentSlide);
</script>

</body>
</html>
