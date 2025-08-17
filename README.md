<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task-01 Enhanced Responsive Landing Page</title>
    <style>
        /* CSS Reset and Base Styles */
        *,
        *::before,
        *::after {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary-color: #1e40af;
            --primary-light: #3b82f6;
            --primary-dark: #1e3a8a;
            --secondary-color: #60a5fa;
            --text-light: #ffffff;
            --text-dark: #1f2937;
            --bg-overlay: rgba(30, 64, 175, 0.95);
            --bg-transparent: rgba(255, 255, 255, 0.1);
            --transition-fast: 0.2s ease;
            --transition-normal: 0.3s ease;
            --transition-slow: 0.5s ease;
            --border-radius: 8px;
            --shadow-light: 0 2px 10px rgba(0, 0, 0, 0.1);
            --shadow-heavy: 0 10px 40px rgba(0, 0, 0, 0.2);
            --z-navbar: 1000;
            --z-overlay: 999;
            --navbar-height: 70px;
        }

        html {
            scroll-behavior: smooth;
            font-size: 16px;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            line-height: 1.6;
            color: var(--text-dark);
            overflow-x: hidden;
            -webkit-font-smoothing: antialiased;
            -moz-osx-font-smoothing: grayscale;
        }

        /* Accessibility improvements */
        *:focus {
            outline: 2px solid var(--secondary-color);
            outline-offset: 2px;
        }

        .sr-only {
            position: absolute;
            width: 1px;
            height: 1px;
            padding: 0;
            margin: -1px;
            overflow: hidden;
            clip: rect(0, 0, 0, 0);
            white-space: nowrap;
            border: 0;
        }

        /* Scroll Progress Indicator */
        .scroll-progress {
            position: fixed;
            top: 0;
            left: 0;
            width: 0%;
            height: 3px;
            background: linear-gradient(90deg, var(--secondary-color), var(--primary-light));
            z-index: calc(var(--z-navbar) + 1);
            transition: width var(--transition-fast);
            will-change: width;
        }

        /* Enhanced Navigation Styles */
        .navbar {
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            background: var(--bg-transparent);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            z-index: var(--z-navbar);
            transition: all var(--transition-normal);
            padding: 0;
            height: var(--navbar-height);
            will-change: background-color, backdrop-filter, box-shadow;
        }

        .navbar::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: var(--bg-overlay);
            opacity: 0;
            transition: opacity var(--transition-normal);
            z-index: -1;
        }

        .navbar.scrolled::before {
            opacity: 1;
        }

        .navbar.scrolled {
            backdrop-filter: blur(20px);
            -webkit-backdrop-filter: blur(20px);
            box-shadow: var(--shadow-light);
            border-bottom-color: rgba(255, 255, 255, 0.2);
        }

        .nav-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 1.5rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            height: 100%;
            position: relative;
        }

        /* Logo Styles */
        .nav-logo {
            font-size: 1.5rem;
            font-weight: 700;
            color: var(--text-light);
            text-decoration: none;
            transition: all var(--transition-normal);
            letter-spacing: -0.025em;
            z-index: 1;
        }

        .nav-logo:hover {
            color: var(--secondary-color);
            text-shadow: 0 0 20px rgba(96, 165, 250, 0.3);
            transform: translateY(-1px);
        }

        .nav-logo:focus {
            outline-color: var(--secondary-color);
        }

        /* Navigation Menu */
        .nav-menu {
            display: flex;
            list-style: none;
            gap: 0.5rem;
            margin: 0;
            padding: 0;
        }

        .nav-item {
            position: relative;
        }

        .nav-link {
            display: inline-block;
            padding: 0.75rem 1.25rem;
            color: var(--text-light);
            text-decoration: none;
            font-weight: 500;
            font-size: 0.95rem;
            border-radius: var(--border-radius);
            transition: all var(--transition-normal);
            position: relative;
            overflow: hidden;
            white-space: nowrap;
            cursor: pointer;
            user-select: none;
        }

        /* Enhanced hover effects */
        .nav-link::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(
                90deg,
                transparent,
                rgba(255, 255, 255, 0.1),
                transparent
            );
            transition: left var(--transition-slow);
            z-index: -1;
        }

        .nav-link::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 50%;
            width: 0;
            height: 2px;
            background: var(--secondary-color);
            transition: all var(--transition-normal);
            transform: translateX(-50%);
        }

        .nav-link:hover,
        .nav-link:focus {
            color: var(--primary-dark);
            background: rgba(255, 255, 255, 0.95);
            transform: translateY(-2px);
            box-shadow: var(--shadow-light);
        }

        .nav-link:hover::before {
            left: 100%;
        }

        .nav-link:hover::after,
        .nav-link:focus::after {
            width: 100%;
        }

        .nav-link.active::after {
            width: 100%;
        }

        /* Mobile Menu Button */
        .nav-toggle {
            display: none;
            flex-direction: column;
            justify-content: space-around;
            width: 2rem;
            height: 2rem;
            background: transparent;
            border: none;
            cursor: pointer;
            padding: 0;
            z-index: calc(var(--z-navbar) + 1);
            transition: transform var(--transition-normal);
        }

        .nav-toggle:hover {
            transform: scale(1.1);
        }

        .nav-toggle:focus {
            outline: 2px solid var(--secondary-color);
        }

        .hamburger-line {
            width: 2rem;
            height: 0.25rem;
            background: var(--text-light);
            border-radius: 10px;
            transition: all var(--transition-normal);
            transform-origin: 1px;
        }

        .nav-toggle.active .hamburger-line:first-child {
            transform: rotate(45deg);
        }

        .nav-toggle.active .hamburger-line:nth-child(2) {
            opacity: 0;
            transform: translateX(20px);
        }

        .nav-toggle.active .hamburger-line:third-child {
            transform: rotate(-45deg);
        }

        /* Mobile Overlay */
        .nav-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            opacity: 0;
            visibility: hidden;
            transition: all var(--transition-normal);
            z-index: var(--z-overlay);
            backdrop-filter: blur(2px);
            -webkit-backdrop-filter: blur(2px);
        }

        .nav-overlay.active {
            opacity: 1;
            visibility: visible;
        }

        /* Responsive Styles */
        @media (max-width: 768px) {
            .nav-toggle {
                display: flex;
            }

            .nav-menu {
                position: fixed;
                top: var(--navbar-height);
                left: 0;
                width: 100%;
                height: calc(100vh - var(--navbar-height));
                background: var(--bg-overlay);
                backdrop-filter: blur(20px);
                -webkit-backdrop-filter: blur(20px);
                flex-direction: column;
                justify-content: flex-start;
                align-items: center;
                gap: 0;
                padding: 2rem 1rem;
                transform: translateX(-100%);
                transition: transform var(--transition-normal);
                overflow-y: auto;
                -webkit-overflow-scrolling: touch;
            }

            .nav-menu.active {
                transform: translateX(0);
            }

            .nav-item {
                width: 100%;
                max-width: 300px;
            }

            .nav-link {
                display: block;
                width: 100%;
                text-align: center;
                padding: 1rem 1.5rem;
                margin: 0.5rem 0;
                border-radius: var(--border-radius);
                font-size: 1.1rem;
            }

            .nav-container {
                padding: 0 1rem;
            }
        }

        @media (max-width: 480px) {
            :root {
                --navbar-height: 60px;
            }

            .nav-logo {
                font-size: 1.25rem;
            }

            .nav-container {
                padding: 0 0.75rem;
            }
        }

        /* Page Content Styles */
        .page-content {
            margin-top: var(--navbar-height);
        }

        .hero {
            height: 100vh;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            display: flex;
            align-items: center;
            justify-content: center;
            color: var(--text-light);
            text-align: center;
            position: relative;
            overflow: hidden;
        }

        .hero::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.1);
            z-index: 1;
        }

        .hero-content {
            position: relative;
            z-index: 2;
            max-width: 800px;
            padding: 0 2rem;
        }

        .hero-title {
            font-size: clamp(2.5rem, 5vw, 4rem);
            font-weight: 700;
            margin-bottom: 1.5rem;
            line-height: 1.2;
            opacity: 0;
            animation: fadeInUp 1s ease 0.5s forwards;
        }

        .hero-description {
            font-size: clamp(1.1rem, 2vw, 1.3rem);
            margin-bottom: 2rem;
            opacity: 0.9;
            line-height: 1.6;
            opacity: 0;
            animation: fadeInUp 1s ease 0.7s forwards;
        }

        .cta-button {
            display: inline-block;
            padding: 1rem 2rem;
            background: rgba(255, 255, 255, 0.1);
            color: var(--text-light);
            text-decoration: none;
            border: 2px solid rgba(255, 255, 255, 0.3);
            border-radius: 50px;
            font-weight: 600;
            transition: all var(--transition-normal);
            opacity: 0;
            animation: fadeInUp 1s ease 0.9s forwards;
        }

        .cta-button:hover {
            background: rgba(255, 255, 255, 0.9);
            color: var(--primary-color);
            transform: translateY(-3px);
            box-shadow: var(--shadow-heavy);
            border-color: transparent;
        }

        .section {
            padding: 5rem 2rem;
            max-width: 1200px;
            margin: 0 auto;
        }

        .section-title {
            font-size: clamp(2rem, 4vw, 2.5rem);
            text-align: center;
            margin-bottom: 3rem;
            color: var(--text-dark);
            font-weight: 700;
        }

        .section-content {
            font-size: 1.1rem;
            line-height: 1.8;
            color: #6b7280;
            max-width: 700px;
            margin: 0 auto;
            text-align: center;
        }

        /* Animations */
        @keyframes fadeInUp {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Performance optimizations */
        .navbar,
        .nav-menu,
        .nav-link,
        .scroll-progress {
            contain: layout style paint;
        }

        /* Reduced motion support */
        @media (prefers-reduced-motion: reduce) {
            * {
                animation-duration: 0.01ms !important;
                animation-iteration-count: 1 !important;
                transition-duration: 0.01ms !important;
                scroll-behavior: auto !important;
            }
        }

        /* High contrast mode support */
        @media (prefers-contrast: high) {
            .navbar {
                border-bottom: 2px solid;
            }
            
            .nav-link {
                border: 1px solid transparent;
            }
            
            .nav-link:hover,
            .nav-link:focus {
                border-color: currentColor;
            }
        }

        /* Print styles */
        @media print {
            .navbar,
            .scroll-progress {
                display: none;
            }
            
            .page-content {
                margin-top: 0;
            }
        }
    </style>
</head>
<body>
    <!-- Skip to main content for accessibility -->
    <a href="#main-content" class="sr-only">Skip to main content</a>

    <!-- Scroll Progress Indicator -->
    <div class="scroll-progress" role="progressbar" aria-label="Page scroll progress" aria-valuemin="0" aria-valuemax="100" aria-valuenow="0"></div>

    <!-- Enhanced Navigation -->
    <nav class="navbar" role="navigation" aria-label="Main navigation">
        <div class="nav-container">
            <a href="#home" class="nav-logo" aria-label="Task-01 Homepage">
                Task-01
            </a>

            <ul class="nav-menu" role="menubar">
                <li class="nav-item" role="none">
                    <a href="#home" class="nav-link" role="menuitem" data-section="home">
                        Home
                    </a>
                </li>
                <li class="nav-item" role="none">
                    <a href="#about" class="nav-link" role="menuitem" data-section="about">
                        About
                    </a>
                </li>
                <li class="nav-item" role="none">
                    <a href="#services" class="nav-link" role="menuitem" data-section="services">
                        Services
                    </a>
                </li>
                <li class="nav-item" role="none">
                    <a href="#portfolio" class="nav-link" role="menuitem" data-section="portfolio">
                        Portfolio
                    </a>
                </li>
                <li class="nav-item" role="none">
                    <a href="#contact" class="nav-link" role="menuitem" data-section="contact">
                        Contact
                    </a>
                </li>
            </ul>

            <button class="nav-toggle" aria-label="Toggle navigation menu" aria-expanded="false" aria-controls="nav-menu">
                <span class="hamburger-line"></span>
                <span class="hamburger-line"></span>
                <span class="hamburger-line"></span>
            </button>
        </div>
    </nav>

    <!-- Mobile Overlay -->
    <div class="nav-overlay" aria-hidden="true"></div>

    <!-- Main Content -->
    <main class="page-content" id="main-content">
        <!-- Hero Section -->
        <section id="home" class="hero">
            <div class="hero-content">
                <h1 class="hero-title">Enhanced Responsive Landing Page</h1>
                <p class="hero-description">
                    Experience a highly optimized navigation menu with advanced stability, 
                    performance optimizations, and accessibility features.
                </p>
                <a href="#about" class="cta-button">Discover Features</a>
            </div>
        </section>

        <!-- About Section -->
        <section id="about" class="section">
            <h2 class="section-title">Enhanced Stability Features</h2>
            <div class="section-content">
                <p>
                    This improved version includes performance optimizations, better error handling, 
                    cross-browser compatibility, accessibility enhancements, and responsive design 
                    patterns that ensure consistent behavior across all devices and browsers.
                </p>
            </div>
        </section>

        <!-- Services Section -->
        <section id="services" class="section">
            <h2 class="section-title">Advanced Optimizations</h2>
            <div class="section-content">
                <p>
                    Features include CSS containment for better performance, reduced motion support, 
                    high contrast mode compatibility, proper ARIA labels, keyboard navigation, 
                    and optimized animations with will-change properties for smooth interactions.
                </p>
            </div>
        </section>

        <!-- Portfolio Section -->
        <section id="portfolio" class="section">
            <h2 class="section-title">Cross-Browser Compatibility</h2>
            <div class="section-content">
                <p>
                    Built with modern CSS custom properties, progressive enhancement, 
                    and fallback support for older browsers. The navigation works consistently 
                    across Chrome, Firefox, Safari, and Edge with optimized performance.
                </p>
            </div>
        </section>

        <!-- Contact Section -->
        <section id="contact" class="section">
            <h2 class="section-title">Production Ready</h2>
            <div class="section-content">
                <p>
                    This enhanced navigation menu is production-ready with comprehensive 
                    error handling, accessibility compliance, performance monitoring, 
                    and maintainable code structure suitable for enterprise applications.
                </p>
            </div>
        </section>
    </main>

    <script>
        /**
         * Enhanced Navigation Menu Controller
         * Provides stable, performant navigation with error handling
         */
        class NavigationController {
            constructor() {
                this.navbar = null;
                this.navToggle = null;
                this.navMenu = null;
                this.navOverlay = null;
                this.navLinks = [];
                this.scrollProgress = null;
                this.isMenuOpen = false;
                this.scrollThreshold = 50;
                this.rafId = null;
                this.lastScrollY = 0;
                this.sections = [];
                
                this.init();
            }

            init() {
                try {
                    this.cacheElements();
                    this.bindEvents();
                    this.setupIntersectionObserver();
                    this.updateScrollProgress();
                } catch (error) {
                    console.error('Navigation initialization failed:', error);
                }
            }

            cacheElements() {
                this.navbar = document.querySelector('.navbar');
                this.navToggle = document.querySelector('.nav-toggle');
                this.navMenu = document.querySelector('.nav-menu');
                this.navOverlay = document.querySelector('.nav-overlay');
                this.navLinks = Array.from(document.querySelectorAll('.nav-link'));
                this.scrollProgress = document.querySelector('.scroll-progress');
                this.sections = Array.from(document.querySelectorAll('section[id]'));

                // Validate critical elements
                if (!this.navbar || !this.navMenu || !this.scrollProgress) {
                    throw new Error('Critical navigation elements not found');
                }
            }

            bindEvents() {
                // Optimized scroll handler with requestAnimationFrame
                window.addEventListener('scroll', this.throttledScrollHandler.bind(this), { passive: true });
                
                // Mobile menu toggle
                if (this.navToggle) {
                    this.navToggle.addEventListener('click', this.toggleMobileMenu.bind(this));
                }

                // Navigation links
                this.navLinks.forEach(link => {
                    link.addEventListener('click', this.handleNavClick.bind(this));
                });

                // Overlay click to close menu
                if (this.navOverlay) {
                    this.navOverlay.addEventListener('click', this.closeMobileMenu.bind(this));
                }

                // Keyboard navigation
                document.addEventListener('keydown', this.handleKeydown.bind(this));

                // Window resize handler
                window.addEventListener('resize', this.debounce(this.handleResize.bind(this), 150));

                // Handle focus management
                document.addEventListener('focusin', this.handleFocusIn.bind(this));
            }

            throttledScrollHandler() {
                if (this.rafId) {
                    cancelAnimationFrame(this.rafId);
                }
                
                this.rafId = requestAnimationFrame(() => {
                    this.handleScroll();
                    this.updateScrollProgress();
                });
            }

            handleScroll() {
                const currentScrollY = window.pageYOffset;
                
                try {
                    // Update navbar appearance
                    if (currentScrollY > this.scrollThreshold) {
                        this.navbar.classList.add('scrolled');
                    } else {
                        this.navbar.classList.remove('scrolled');
                    }

                    this.lastScrollY = currentScrollY;
                } catch (error) {
                    console.warn('Scroll handling error:', error);
                }
            }

            updateScrollProgress() {
                try {
                    const scrollHeight = document.documentElement.scrollHeight - window.innerHeight;
                    const scrollPercent = Math.min((window.pageYOffset / scrollHeight) * 100, 100);
                    
                    if (this.scrollProgress) {
                        this.scrollProgress.style.width = `${scrollPercent}%`;
                        this.scrollProgress.setAttribute('aria-valuenow', Math.round(scrollPercent));
                    }
                } catch (error) {
                    console.warn('Scroll progress update failed:', error);
                }
            }

            setupIntersectionObserver() {
                if (!('IntersectionObserver' in window)) {
                    return; // Fallback for older browsers
                }

                const options = {
                    threshold: 0.3,
                    rootMargin: '-100px 0px -100px 0px'
                };

                const observer = new IntersectionObserver((entries) => {
                    entries.forEach(entry => {
                        if (entry.isIntersecting) {
                            this.updateActiveNavLink(entry.target.id);
                        }
                    });
                }, options);

                this.sections.forEach(section => {
                    observer.observe(section);
                });
            }

            updateActiveNavLink(activeSection) {
                try {
                    this.navLinks.forEach(link => {
                        const section = link.getAttribute('data-section');
                        if (section === activeSection) {
                            link.classList.add('active');
                            link.setAttribute('aria-current', 'page');
                        } else {
                            link.classList.remove('active');
                            link.removeAttribute('aria-current');
                        }
                    });
                } catch (error) {
                    console.warn('Active link update failed:', error);
                }
            }

            toggleMobileMenu() {
                try {
                    this.isMenuOpen = !this.isMenuOpen;
                    
                    if (this.isMenuOpen) {
                        this.openMobileMenu();
                    } else {
                        this.closeMobileMenu();
                    }
                } catch (error) {
                    console.error('Mobile menu toggle failed:', error);
                }
            }

            openMobileMenu() {
                this.navToggle?.classList.add('active');
                this.navMenu?.classList.add('active');
                this.navOverlay?.classList.add('active');
                
                // Update ARIA attributes
                this.navToggle?.setAttribute('aria-expanded', 'true');
                this.navMenu?.setAttribute('aria-hidden', 'false');
                
                // Prevent body scroll
                document.body.style.overflow = 'hidden';
                
                // Focus first menu item
                const firstLink = this.navMenu?.querySelector('.nav-link');
                firstLink?.focus();
            }

            closeMobileMenu() {
                this.isMenuOpen = false;
                
                this.navToggle?.classList.remove('active');
                this.navMenu?.classList.remove('active');
                this.navOverlay?.classList.remove('active');
                
                // Update ARIA attributes
                this.navToggle?.setAttribute('aria-expanded', 'false');
                this.navMenu?.setAttribute('aria-hidden', 'true');
                
                // Restore body scroll
                document.body.style.overflow = '';
            }

            handleNavClick(event) {
                event.preventDefault();
                
                try {
                    const href = event.target.getAttribute('href');
                    const targetId = href?.substring(1);
                    const targetElement = document.getElementById(targetId);
                    
                    if (targetElement) {
                        const offsetTop = targetElement.offsetTop - (this.navbar?.offsetHeight || 70);
                        
                        window.scrollTo({
                            top: Math.max(0, offsetTop),
                            behavior: 'smooth'
                        });
                        
                        // Close mobile menu if open
                        if (this.isMenuOpen) {
                            this.closeMobileMenu();
                        }
                        
                        // Update focus for accessibility
                        setTimeout(() => {
                            targetElement.setAttribute('tabindex', '-1');
                            targetElement.focus();
                        }, 500);
                    }
                } catch (error) {
                    console.error('Navigation click failed:', error);
                }
            }

            handleKeydown(event) {
                // Handle Escape key to close mobile menu
                if (event.key === 'Escape' && this.isMenuOpen) {
                    this.closeMobileMenu();
                    this.navToggle?.focus();
                }
                
                // Handle Tab navigation in mobile menu
                if (event.key === 'Tab' && this.isMenuOpen) {
                    this.trapFocus(event);
                }
            }

            trapFocus(event) {
                const focusableElements = this.navMenu?.querySelectorAll(
                    'a, button, [tabindex]:not([tabindex="-1"])'
                );
                
                if (!focusableElements?.length) return;
                
                const firstElement = focusableElements[0];
                const lastElement = focusableElements[focusableElements.length - 1];
                
                if (event.shiftKey) {
                    if (document.activeElement === firstElement) {
                        event.preventDefault();
                        lastElement.focus();
                    }
                } else {
                    if (document.activeElement === lastElement) {
                        event.preventDefault();
                        firstElement.focus();
                    }
                }
            }

            handleResize() {
                // Close mobile menu on resize to desktop
                if (window.innerWidth > 768 && this.isMenuOpen) {
                    this.closeMobileMenu();
                }
            }

            handleFocusIn(event) {
                // Ensure mobile menu is visible when focused
                if (this.navMenu?.contains(event.target) && !this.isMenuOpen) {
                    // Only auto-open if using keyboard navigation
                    if (event.target.matches(':focus-visible')) {
                        this.openMobileMenu();
                    }
                }
            }

            // Utility methods
            debounce(func, wait) {
                let timeout;
                return function executedFunction(...args) {
                    const later = () => {
                        clearTimeout(timeout);
                        func(...args);
                    };
                    clearTimeout(timeout);
                    timeout = setTimeout(later, wait);
                };
            }

            // Public method to destroy the instance
            destroy() {
                if (this.rafId) {
                    cancelAnimationFrame(this.rafId);
                }
                
                // Remove event listeners
                window.removeEventListener('scroll', this.throttledScrollHandler);
                window.removeEventListener('resize', this.handleResize);
                document.removeEventListener('keydown', this.handleKeydown);
                document.removeEventListener('focusin', this.handleFocusIn);
                
                // Reset DOM state
                document.body.style.overflow = '';
                
                console.log('Navigation controller destroyed');
            }
        }

        // Initialize navigation when DOM is ready
        document.addEventListener('DOMContentLoaded', () => {
            try {
                window.navigationController = new NavigationController();
                
                // Optional: Global error handler for navigation
                window.addEventListener('error', (event) => {
                    if (event.filename?.includes('navigation')) {
                        console.error('Navigation runtime error:', event.error);
                    }
                });
                
            } catch (error) {
                console.error('Failed to initialize navigation:', error);
                
                // Fallback: Basic functionality without advanced features
                const links = document.querySelectorAll('.nav-link');
                links.forEach(link => {
                    link.addEventListener('click', (e) => {
                        e.preventDefault();
                        const target = document.querySelector(link.getAttribute('href'));
                        if (target) {
                            target.scrollIntoView({ behavior: 'smooth', block: 'start' });
                        }
                    });
                });
            }
        });

        // Cleanup on page unload
        window.addEventListener('beforeunload', () => {
            if (window.navigationController && typeof window.navigationController.destroy === 'function') {
                window.navigationController.destroy();
            }
        });
    </script>
</body>
</html>
# PRODIGY_WB_01
task 01 of my internship on web development at infotech
