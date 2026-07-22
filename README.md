<!DOCTYPE html>
<html lang="en" data-theme="light">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Interactive Resume</title>

    <!-- Font Awesome for Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css" />

    <style>
        /* ========== CSS VARIABLES / THEMING ========== */
        :root {
            --bg-primary: #ffffff;
            --bg-secondary: #f8f9fa;
            --text-primary: #1a1a2e;
            --text-secondary: #555;
            --accent: #6c63ff;
            --accent-hover: #5a52d5;
            --card-bg: #ffffff;
            --shadow: 0 10px 30px rgba(0, 0, 0, 0.08);
            --border-radius: 16px;
            --transition: 0.4s cubic-bezier(0.25, 0.46, 0.45, 0.94);
        }

        [data-theme="dark"] {
            --bg-primary: #0f0f1a;
            --bg-secondary: #1a1a2e;
            --text-primary: #f0f0f5;
            --text-secondary: #b0b0c0;
            --card-bg: #1e1e32;
            --shadow: 0 10px 30px rgba(0, 0, 0, 0.4);
        }

        /* ========== RESET & BASE ========== */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        html {
            scroll-behavior: smooth;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: var(--bg-primary);
            color: var(--text-primary);
            transition: background 0.4s, color 0.4s;
            line-height: 1.6;
            overflow-x: hidden;
        }

        a {
            text-decoration: none;
            color: inherit;
        }

        .container {
            max-width: 1100px;
            margin: 0 auto;
            padding: 0 24px;
        }

        /* ========== MOUSE GLOW ========== */
        #mouse-glow {
            position: fixed;
            width: 300px;
            height: 300px;
            border-radius: 50%;
            background: radial-gradient(circle, rgba(108, 99, 255, 0.06) 0%, transparent 70%);
            pointer-events: none;
            transform: translate(-50%, -50%);
            z-index: 0;
            transition: opacity 0.3s;
        }

        /* ========== NAVBAR ========== */
        nav {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            z-index: 1000;
            background: rgba(255, 255, 255, 0.75);
            backdrop-filter: blur(12px);
            box-shadow: 0 2px 20px rgba(0, 0, 0, 0.04);
            padding: 14px 0;
            transition: background 0.4s;
        }

        [data-theme="dark"] nav {
            background: rgba(15, 15, 26, 0.85);
        }

        .nav-container {
            display: flex;
            justify-content: space-between;
            align-items: center;
            max-width: 1100px;
            margin: 0 auto;
            padding: 0 24px;
        }

        .logo {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--accent);
        }

        .nav-links {
            display: flex;
            gap: 32px;
            list-style: none;
            align-items: center;
        }

        .nav-links a {
            font-weight: 500;
            font-size: 0.95rem;
            color: var(--text-secondary);
            transition: color 0.3s;
            position: relative;
        }

        .nav-links a:hover,
        .nav-links a.active {
            color: var(--accent);
        }

        .nav-links a::after {
            content: '';
            position: absolute;
            bottom: -4px;
            left: 0;
            width: 0%;
            height: 2px;
            background: var(--accent);
            transition: width 0.3s;
        }

        .nav-links a:hover::after,
        .nav-links a.active::after {
            width: 100%;
        }

        #theme-toggle {
            background: none;
            border: none;
            font-size: 1.3rem;
            cursor: pointer;
            color: var(--text-primary);
            transition: transform 0.3s;
            padding: 4px 8px;
        }

        #theme-toggle:hover {
            transform: rotate(20deg);
        }

        /* ========== HERO ========== */
        .hero {
            min-height: 100vh;
            display: flex;
            align-items: center;
            padding-top: 80px;
            position: relative;
            z-index: 1;
        }

        .hero-content {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 60px;
            align-items: center;
            width: 100%;
        }

        .hero-text h1 {
            font-size: 3.2rem;
            font-weight: 700;
            line-height: 1.2;
        }

        .hero-text h1 span {
            color: var(--accent);
        }

        .hero-text .typing-wrapper {
            font-size: 1.4rem;
            color: var(--text-secondary);
            margin: 16px 0 24px;
            min-height: 2.4rem;
        }

        .hero-text .typing-wrapper .cursor {
            display: inline-block;
            width: 2px;
            height: 1.2em;
            background: var(--accent);
            margin-left: 2px;
            animation: blink 0.8s step-end infinite;
        }

        @keyframes blink {
            0%,
            100% {
                opacity: 1;
            }
            50% {
                opacity: 0;
            }
        }

        .btn {
            display: inline-block;
            padding: 12px 32px;
            background: var(--accent);
            color: #fff;
            border-radius: 50px;
            font-weight: 600;
            transition: background 0.3s, transform 0.2s;
            border: none;
            cursor: pointer;
        }

        .btn:hover {
            background: var(--accent-hover);
            transform: translateY(-2px);
        }

        .btn-outline {
            background: transparent;
            color: var(--accent);
            border: 2px solid var(--accent);
            margin-left: 12px;
        }

        .btn-outline:hover {
            background: var(--accent);
            color: #fff;
        }

        /* ========== AVATAR WITH IMAGE ========== */
        .hero-avatar {
            display: flex;
            justify-content: center;
            align-items: center;
            position: relative;
        }

        .avatar-wrapper {
            position: relative;
            width: 320px;
            height: 320px;
            border-radius: 50%;
            overflow: hidden;
            border: 4px solid var(--accent);
            box-shadow: 0 20px 60px rgba(108, 99, 255, 0.3);
            animation: float 6s ease-in-out infinite;
            background: var(--bg-secondary);
        }

        .avatar-wrapper img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            display: block;
        }

        /* Glow overlay on avatar */
        .avatar-wrapper::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 30% 30%, rgba(108, 99, 255, 0.15), transparent 70%);
            pointer-events: none;
            border-radius: 50%;
        }

        /* Floating AI tags */
        .avatar-tags {
            position: absolute;
            bottom: -15px;
            right: -10px;
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .avatar-tags span {
            background: var(--accent);
            color: white;
            padding: 4px 14px;
            border-radius: 20px;
            font-size: 0.7rem;
            font-weight: 600;
            box-shadow: 0 4px 15px rgba(108, 99, 255, 0.3);
            backdrop-filter: blur(4px);
            letter-spacing: 0.5px;
        }

        @keyframes float {
            0%,
            100% {
                transform: translateY(0px);
            }
            50% {
                transform: translateY(-16px);
            }
        }

        /* ========== SECTION COMMON ========== */
        section {
            padding: 80px 0;
            position: relative;
            z-index: 1;
        }

        .section-title {
            font-size: 2.2rem;
            font-weight: 700;
            margin-bottom: 48px;
            text-align: center;
        }

        .section-title span {
            color: var(--accent);
        }

        /* ========== FADE-IN ANIMATION ========== */
        .fade-in {
            opacity: 0;
            transform: translateY(40px);
            transition: opacity 0.8s ease, transform 0.8s ease;
        }

        .fade-in.visible {
            opacity: 1;
            transform: translateY(0);
        }

        .fade-in-left {
            opacity: 0;
            transform: translateX(-40px);
            transition: opacity 0.8s ease, transform 0.8s ease;
        }

        .fade-in-left.visible {
            opacity: 1;
            transform: translateX(0);
        }

        .fade-in-right {
            opacity: 0;
            transform: translateX(40px);
            transition: opacity 0.8s ease, transform 0.8s ease;
        }

        .fade-in-right.visible {
            opacity: 1;
            transform: translateX(0);
        }

        /* ========== SKILLS ========== */
        #skills {
            background: var(--bg-secondary);
        }

        .skills-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 32px;
        }

        .skill-item {
            background: var(--card-bg);
            padding: 24px 28px;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            transition: transform 0.3s;
        }

        .skill-item:hover {
            transform: translateY(-4px);
        }

        .skill-item .skill-name {
            display: flex;
            justify-content: space-between;
            font-weight: 600;
            margin-bottom: 8px;
        }

        .skill-bar {
            width: 100%;
            height: 8px;
            background: var(--bg-secondary);
            border-radius: 10px;
            overflow: hidden;
        }

        .skill-progress {
            height: 100%;
            width: 0%;
            background: linear-gradient(90deg, var(--accent), #a78bfa);
            border-radius: 10px;
            transition: width 1.2s ease;
        }

        /* ========== EXPERIENCE ========== */
        .timeline {
            display: flex;
            flex-direction: column;
            gap: 32px;
            max-width: 700px;
            margin: 0 auto;
        }

        .timeline-item {
            background: var(--card-bg);
            padding: 28px 32px;
            border-radius: var(--border-radius);
            box-shadow: var(--shadow);
            border-left: 4px solid var(--accent);
            transition: transform 0.3s;
        }

        .timeline-item:hover {
            transform: translateX(6px);
        }

        .timeline-item h3 {
            font-size: 1.2rem;
        }

        .timeline-item .company {
            color: var(--accent);
            font-weight: 600;
        }

        .timeline-item .date {
            font-size: 0.85rem;
            color: var(--text-secondary);
            margin-top: 4px;
        }

        /* ========== PROJECTS ========== */
        #projects {
            background: var(--bg-secondary);
        }

        .project-filters {
            display: flex;
            justify-content: center;
            gap: 12px;
            flex-wrap: wrap;
            margin-bottom: 40px;
        }

        .filter-btn {
            padding: 8px 24px;
            border: 2px solid var(--accent);
            background: transparent;
            color: var(--text-primary);
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
            transition: 0.3s;
        }

        .filter-btn.active,
        .filter-btn:hover {
            background: var(--accent);
            color: #fff;
        }

        .projects-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 28px;
        }

        .project-card {
            background: var(--card-bg);
            border-radius: var(--border-radius);
            padding: 28px;
            box-shadow: var(--shadow);
            transition: transform 0.3s, box-shadow 0.3s;
        }

        .project-card:hover {
            transform: translateY(-8px);
            box-shadow: 0 20px 40px rgba(0, 0, 0, 0.12);
        }

        .project-card .icon {
            font-size: 2.5rem;
            color: var(--accent);
            margin-bottom: 12px;
        }

        .project-card h3 {
            margin-bottom: 8px;
        }

        .project-card p {
            color: var(--text-secondary);
            font-size: 0.95rem;
        }

        .project-card .tags {
            display: flex;
            gap: 8px;
            flex-wrap: wrap;
            margin-top: 16px;
        }

        .project-card .tags span {
            background: var(--bg-secondary);
            padding: 4px 14px;
            border-radius: 50px;
            font-size: 0.75rem;
            font-weight: 600;
            color: var(--text-secondary);
        }

        /* ========== CONTACT ========== */
        .contact-wrapper {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 48px;
            max-width: 900px;
            margin: 0 auto;
        }

        .contact-info h3 {
            font-size: 1.6rem;
            margin-bottom: 16px;
        }

        .contact-info p {
            color: var(--text-secondary);
            margin-bottom: 24px;
        }

        .contact-info .social-links {
            display: flex;
            gap: 16px;
        }

        .contact-info .social-links a {
            font-size: 1.6rem;
            color: var(--text-secondary);
            transition: color 0.3s, transform 0.3s;
        }

        .contact-info .social-links a:hover {
            color: var(--accent);
            transform: scale(1.1);
        }

        .contact-form input,
        .contact-form textarea {
            width: 100%;
            padding: 14px 18px;
            margin-bottom: 16px;
            border: 2px solid var(--bg-secondary);
            border-radius: 12px;
            background: var(--bg-secondary);
            color: var(--text-primary);
            font-size: 1rem;
            transition: border 0.3s;
            font-family: inherit;
        }

        .contact-form input:focus,
        .contact-form textarea:focus {
            outline: none;
            border-color: var(--accent);
        }

        .contact-form textarea {
            min-height: 140px;
            resize: vertical;
        }

        .contact-form .btn {
            width: 100%;
        }

        /* ========== FOOTER ========== */
        footer {
            text-align: center;
            padding: 32px 0;
            border-top: 1px solid var(--bg-secondary);
            color: var(--text-secondary);
            font-size: 0.9rem;
        }

        /* ========== RESPONSIVE ========== */
        @media (max-width: 768px) {
            .hero-content {
                grid-template-columns: 1fr;
                text-align: center;
            }

            .hero-text h1 {
                font-size: 2.4rem;
            }

            .avatar-wrapper {
                width: 200px;
                height: 200px;
            }

            .avatar-tags {
                bottom: -10px;
                right: 0;
                justify-content: center;
            }

            .avatar-tags span {
                font-size: 0.6rem;
                padding: 3px 10px;
            }

            .nav-links {
                gap: 16px;
            }

            .nav-links a {
                font-size: 0.8rem;
            }

            .contact-wrapper {
                grid-template-columns: 1fr;
            }

            .section-title {
                font-size: 1.8rem;
            }
        }

        @media (max-width: 480px) {
            .nav-links {
                gap: 10px;
            }
            .nav-links a {
                font-size: 0.7rem;
            }
            .hero-text h1 {
                font-size: 1.8rem;
            }
            .btn {
                padding: 10px 20px;
                font-size: 0.9rem;
            }
            .avatar-wrapper {
                width: 160px;
                height: 160px;
            }
        }
    </style>
</head>
<body>

    <!-- ========== MOUSE GLOW ========== -->
    <div id="mouse-glow"></div>

    <!-- ========== NAVBAR ========== -->
    <nav>
        <div class="nav-container">
            <a href="#" class="logo">&lt;/&gt; Dev</a>
            <ul class="nav-links">
                <li><a href="#hero" class="active">Home</a></li>
                <li><a href="#skills">Skills</a></li>
                <li><a href="#experience">Experience</a></li>
                <li><a href="#projects">Projects</a></li>
                <li><a href="#contact">Contact</a></li>
                <li>
                    <button id="theme-toggle" aria-label="Toggle theme">
                        <i class="fas fa-moon"></i>
                    </button>
                </li>
            </ul>
        </div>
    </nav>

    <!-- ========== HERO SECTION WITH YOUR IMAGE ========== -->
    <section class="hero" id="hero">
        <div class="container hero-content">
            <div class="hero-text fade-in">
                <h1>Hi, I'm <span>Andualem Guchi</span></h1>
                <div class="typing-wrapper">
                    <span id="typed-text"></span><span class="cursor"></span>
                </div>
                <p style="color: var(--text-secondary); margin-bottom: 28px; max-width: 480px;">
                    AI & Machine Learning engineer passionate about building intelligent systems with neural networks and deep learning.
                </p>
                <a href="#contact" class="btn">Hire Me</a>
                <a href="#projects" class="btn btn-outline">View Work</a>
            </div>

            <!-- ========== AVATAR WITH YOUR IMAGE ========== -->
            <div class="hero-avatar fade-in">
                <div class="avatar-wrapper">
                    <!-- YOUR IMAGE HERE -->
                    <img src="Gemini_Generated_Image_z7m9j5z7m9j5z7m9.png" alt="Andualem Guchi - AI Developer" />
                </div>
                <!-- Floating AI Tags -->
                <div class="avatar-tags">
                    <span>🤖 AI</span>
                    <span>🧠 Neural</span>
                    <span>📊 ML</span>
                </div>
            </div>
        </div>
    </section>

    <!-- ========== SKILLS ========== -->
    <section id="skills">
        <div class="container">
            <h2 class="section-title fade-in">My <span>Skills</span></h2>
            <div class="skills-grid">

                <div class="skill-item fade-in">
                    <div class="skill-name"><span>Artificial Intelligence</span><span>90%</span></div>
                    <div class="skill-bar"><div class="skill-progress" data-width="90"></div></div>
                </div>

                <div class="skill-item fade-in">
                    <div class="skill-name"><span>Machine Learning</span><span>85%</span></div>
                    <div class="skill-bar"><div class="skill-progress" data-width="85"></div></div>
                </div>

                <div class="skill-item fade-in">
                    <div class="skill-name"><span>Neural Networks</span><span>78%</span></div>
                    <div class="skill-bar"><div class="skill-progress" data-width="78"></div></div>
                </div>

                <div class="skill-item fade-in">
                    <div class="skill-name"><span>Python</span><span>88%</span></div>
                    <div class="skill-bar"><div class="skill-progress" data-width="88"></div></div>
                </div>

                <div class="skill-item fade-in">
                    <div class="skill-name"><span>TensorFlow</span><span>75%</span></div>
                    <div class="skill-bar"><div class="skill-progress" data-width="75"></div></div>
                </div>

                <div class="skill-item fade-in">
                    <div class="skill-name"><span>Data Science</span><span>82%</span></div>
                    <div class="skill-bar"><div class="skill-progress" data-width="82"></div></div>
                </div>

            </div>
        </div>
    </section>

    <!-- ========== EXPERIENCE ========== -->
    <section id="experience">
        <div class="container">
            <h2 class="section-title fade-in">Work <span>Experience</span></h2>
            <div class="timeline">

                <div class="timeline-item fade-in-left">
                    <h3>Senior AI Engineer</h3>
                    <div class="company">NeuralTech Labs</div>
                    <div class="date">2022 – Present</div>
                    <p style="color: var(--text-secondary); margin-top: 8px;">
                        Leading a team of 8 AI researchers building neural network models for autonomous systems.
                    </p>
                </div>

                <div class="timeline-item fade-in-right">
                    <h3>Machine Learning Developer</h3>
                    <div class="company">DataFlow AI</div>
                    <div class="date">2020 – 2022</div>
                    <p style="color: var(--text-secondary); margin-top: 8px;">
                        Developed and deployed ML models for predictive analytics using TensorFlow and PyTorch.
                    </p>
                </div>

                <div class="timeline-item fade-in-left">
                    <h3>Data Scientist</h3>
                    <div class="company">Analytics Pro</div>
                    <div class="date">2019 – 2020</div>
                    <p style="color: var(--text-secondary); margin-top: 8px;">
                        Built data pipelines and visualization dashboards for enterprise clients.
                    </p>
                </div>

            </div>
        </div>
    </section>

    <!-- ========== PROJECTS ========== -->
    <section id="projects">
        <div class="container">
            <h2 class="section-title fade-in">My <span>Projects</span></h2>

            <div class="project-filters fade-in">
                <button class="filter-btn active" data-filter="all">All</button>
                <button class="filter-btn" data-filter="ai">AI/ML</button>
                <button class="filter-btn" data-filter="web">Web Apps</button>
                <button class="filter-btn" data-filter="data">Data Science</button>
            </div>

            <div class="projects-grid" id="projects-grid">

                <div class="project-card" data-category="ai">
                    <div class="icon"><i class="fas fa-brain"></i></div>
                    <h3>Neural Image Classifier</h3>
                    <p>Deep learning model achieving 95% accuracy on image recognition tasks.</p>
                    <div class="tags"><span>TensorFlow</span><span>CNN</span></div>
                </div>

                <div class="project-card" data-category="data">
                    <div class="icon"><i class="fas fa-chart-line"></i></div>
                    <h3>Predictive Analytics Dashboard</h3>
                    <p>Real-time data visualization with ML-powered forecasting.</p>
                    <div class="tags"><span>Python</span><span>Plotly</span></div>
                </div>

                <div class="project-card" data-category="ai">
                    <div class="icon"><i class="fas fa-robot"></i></div>
                    <h3>NLP Chatbot</h3>
                    <p>Conversational AI using transformer models and attention mechanisms.</p>
                    <div class="tags"><span>PyTorch</span><span>Transformers</span></div>
                </div>

                <div class="project-card" data-category="web">
                    <div class="icon"><i class="fas fa-code"></i></div>
                    <h3>AI Model Deployment Platform</h3>
                    <p>Web interface for deploying and monitoring ML models in production.</p>
                    <div class="tags"><span>React</span><span>FastAPI</span></div>
                </div>

            </div>
        </div>
    </section>

    <!-- ========== CONTACT ========== -->
    <section id="contact">
        <div class="container">
            <h2 class="section-title fade-in">Get In <span>Touch</span></h2>
            <div class="contact-wrapper">

                <div class="contact-info fade-in-left">
                    <h3>Let's build something intelligent</h3>
                    <p>
                        I'm always open to discussing AI projects, research collaborations, or opportunities to innovate together.
                    </p>
                    <div class="social-links">
                        <a href="#" aria-label="GitHub"><i class="fab fa-github"></i></a>
                        <a href="#" aria-label="LinkedIn"><i class="fab fa-linkedin-in"></i></a>
                        <a href="#" aria-label="Twitter"><i class="fab fa-twitter"></i></a>
                        <a href="#" aria-label="YouTube"><i class="fab fa-youtube"></i></a>
                    </div>
                </div>

                <form class="contact-form fade-in-right" id="contact-form">
                    <input type="text" placeholder="Your Name" required />
                    <input type="email" placeholder="Your Email" required />
                    <textarea placeholder="Your Message" required></textarea>
                    <button type="submit" class="btn">Send Message</button>
                </form>

            </div>
        </div>
    </section>

    <!-- ========== FOOTER ========== -->
    <footer>
        <div class="container">
            <p>&copy; 2026 Andualem Guchi. Built with <i class="fas fa-heart" style="color: var(--accent);"></i> and vanilla JavaScript.</p>
        </div>
    </footer>

    <!-- ========== JAVASCRIPT ========== -->
    <script>
        document.addEventListener('DOMContentLoaded', function() {

            // =============================================
            // 1. MOUSE GLOW
            // =============================================
            var glow = document.getElementById('mouse-glow');
            document.addEventListener('mousemove', function(e) {
                glow.style.left = e.clientX + 'px';
                glow.style.top = e.clientY + 'px';
            });

            // =============================================
            // 2. TYPING EFFECT
            // =============================================
            var typedTextSpan = document.getElementById('typed-text');
            var words = ['AI Engineer', 'ML Specialist', 'Deep Learning Expert', 'Data Scientist'];
            var wordIndex = 0;
            var charIndex = 0;
            var isDeleting = false;
            var typeSpeed = 100;

            function typeEffect() {
                var currentWord = words[wordIndex];
                if (isDeleting) {
                    typedTextSpan.textContent = currentWord.substring(0, charIndex - 1);
                    charIndex--;
                    typeSpeed = 60;
                } else {
                    typedTextSpan.textContent = currentWord.substring(0, charIndex + 1);
                    charIndex++;
                    typeSpeed = 120;
                }

                if (!isDeleting && charIndex === currentWord.length) {
                    isDeleting = true;
                    typeSpeed = 1800;
                } else if (isDeleting && charIndex === 0) {
                    isDeleting = false;
                    wordIndex = (wordIndex + 1) % words.length;
                    typeSpeed = 400;
                }

                setTimeout(typeEffect, typeSpeed);
            }
            typeEffect();

            // =============================================
            // 3. SCROLL ANIMATIONS
            // =============================================
            var fadeElements = document.querySelectorAll('.fade-in, .fade-in-left, .fade-in-right');

            var observer = new IntersectionObserver(function(entries) {
                entries.forEach(function(entry) {
                    if (entry.isIntersecting) {
                        entry.target.classList.add('visible');
                    }
                });
            }, {
                threshold: 0.15,
                rootMargin: '0px 0px -40px 0px'
            });

            fadeElements.forEach(function(el) {
                observer.observe(el);
            });

            // =============================================
            // 4. SKILL BARS
            // =============================================
            var skillBars = document.querySelectorAll('.skill-progress');

            var skillObserver = new IntersectionObserver(function(entries) {
                entries.forEach(function(entry) {
                    if (entry.isIntersecting) {
                        var bar = entry.target;
                        var width = bar.getAttribute('data-width');
                        if (width) {
                            bar.style.width = width + '%';
                        }
                    }
                });
            }, {
                threshold: 0.5
            });

            skillBars.forEach(function(bar) {
                skillObserver.observe(bar);
            });

            // =============================================
            // 5. DARK / LIGHT MODE
            // =============================================
            var toggle = document.getElementById('theme-toggle');
            var icon = toggle.querySelector('i');

            var currentTheme = localStorage.getItem('theme') || 'light';
            document.documentElement.setAttribute('data-theme', currentTheme);
            updateIcon(currentTheme);

            toggle.addEventListener('click', function() {
                var theme = document.documentElement.getAttribute('data-theme');
                var newTheme = theme === 'light' ? 'dark' : 'light';
                document.documentElement.setAttribute('data-theme', newTheme);
                localStorage.setItem('theme', newTheme);
                updateIcon(newTheme);
            });

            function updateIcon(theme) {
                if (theme === 'dark') {
                    icon.className = 'fas fa-sun';
                } else {
                    icon.className = 'fas fa-moon';
                }
            }

            // =============================================
            // 6. ACTIVE NAV LINK
            // =============================================
            var sections = document.querySelectorAll('section[id]');
            var navLinks = document.querySelectorAll('.nav-links a:not(#theme-toggle)');

            window.addEventListener('scroll', function() {
                var current = 'hero';
                sections.forEach(function(section) {
                    var top = section.offsetTop - 150;
                    if (window.scrollY >= top) {
                        current = section.getAttribute('id');
                    }
                });

                navLinks.forEach(function(link) {
                    link.classList.remove('active');
                    if (link.getAttribute('href') === '#' + current) {
                        link.classList.add('active');
                    }
                });
            });

            // =============================================
            // 7. PROJECT FILTER
            // =============================================
            var filterBtns = document.querySelectorAll('.filter-btn');
            var projectCards = document.querySelectorAll('.project-card');

            filterBtns.forEach(function(btn) {
                btn.addEventListener('click', function() {
                    filterBtns.forEach(function(b) {
                        b.classList.remove('active');
                    });
                    btn.classList.add('active');

                    var filter = btn.getAttribute('data-filter');

                    projectCards.forEach(function(card) {
                        var category = card.getAttribute('data-category');
                        if (filter === 'all' || category === filter) {
                            card.style.display = 'block';
                            card.style.animation = 'fadeIn 0.5s ease';
                        } else {
                            card.style.display = 'none';
                        }
                    });
                });
            });

            var styleSheet = document.createElement('style');
            styleSheet.textContent =
                '@keyframes fadeIn { from { opacity: 0; transform: scale(0.96); } to { opacity: 1; transform: scale(1); } }';
            document.head.appendChild(styleSheet);

            // =============================================
            // 8. CONTACT FORM - EMAIL UPDATED
            // =============================================
            var form = document.getElementById('contact-form');
            form.addEventListener('submit', function(e) {
                e.preventDefault();

                var inputs = form.querySelectorAll('input, textarea');
                var allFilled = true;

                inputs.forEach(function(input) {
                    if (!input.value.trim()) {
                        allFilled = false;
                        input.style.borderColor = '#ff6b6b';
                        setTimeout(function() {
                            input.style.borderColor = '';
                        }, 2000);
                    }
                });

                if (allFilled) {
                    var btn = form.querySelector('.btn');
                    var originalText = btn.textContent;
                    btn.textContent = '✅ Sent!';
                    btn.style.background = '#28a745';

                    // Email would be sent to: andualemandualem25@gmail.com
                    console.log('Form submitted to: andualemandualem25@gmail.com');

                    setTimeout(function() {
                        btn.textContent = originalText;
                        btn.style.background = '';
                        form.reset();
                    }, 2500);
                }
            });

            // =============================================
            // 9. SMOOTH SCROLL
            // =============================================
            document.querySelectorAll('a[href^="#"]').forEach(function(anchor) {
                anchor.addEventListener('click', function(e) {
                    var target = document.querySelector(this.getAttribute('href'));
                    if (target) {
                        e.preventDefault();
                        target.scrollIntoView({
                            behavior: 'smooth',
                            block: 'start'
                        });
                    }
                });
            });

        });
    </script>

</body>
</html>
