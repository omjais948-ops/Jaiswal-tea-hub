/* ================================
   JAVASCRIPT FOR JAISWAL JI TEA HUB
================================ */

// ===== LOADER ANIMATION =====
window.addEventListener('load', () => {
    const loader = document.getElementById('loader');
    setTimeout(() => {
        loader.style.display = 'none';
    }, 2000);
});

// ===== NAVBAR FUNCTIONALITY =====
const navbar = document.getElementById('navbar');
const navMenu = document.getElementById('navMenu');
const hamburger = document.getElementById('hamburger');
const navLinks = document.querySelectorAll('.nav-link');

// Sticky navbar on scroll
window.addEventListener('scroll', () => {
    if (window.scrollY > 100) {
        navbar.classList.add('scrolled');
    } else {
        navbar.classList.remove('scrolled');
    }
});

// Hamburger menu toggle
hamburger.addEventListener('click', () => {
    navMenu.classList.toggle('active');
    hamburger.classList.toggle('active');
});

// Close menu when link is clicked
navLinks.forEach(link => {
    link.addEventListener('click', () => {
        navMenu.classList.remove('active');
        hamburger.classList.remove('active');
    });
});

// ===== DARK MODE TOGGLE =====
const themeToggle = document.getElementById('themeToggle');
const htmlElement = document.documentElement;
const body = document.body;

// Check for saved theme preference
const currentTheme = localStorage.getItem('theme') || 'light';
if (currentTheme === 'dark') {
    body.classList.add('dark-mode');
    themeToggle.innerHTML = '<i class="fas fa-sun"></i>';
}

themeToggle.addEventListener('click', () => {
    body.classList.toggle('dark-mode');
    
    if (body.classList.contains('dark-mode')) {
        localStorage.setItem('theme', 'dark');
        themeToggle.innerHTML = '<i class="fas fa-sun"></i>';
    } else {
        localStorage.setItem('theme', 'light');
        themeToggle.innerHTML = '<i class="fas fa-moon"></i>';
    }
});

// ===== SMOOTH SCROLLING =====
document.querySelectorAll('a[href^="#"]').forEach(anchor => {
    anchor.addEventListener('click', function (e) {
        e.preventDefault();
        const target = document.querySelector(this.getAttribute('href'));
        if (target) {
            target.scrollIntoView({
                behavior: 'smooth',
                block: 'start'
            });
        }
    });
});

// ===== SCROLL TO TOP BUTTON =====
const scrollTopBtn = document.getElementById('scrollTop');

window.addEventListener('scroll', () => {
    if (window.scrollY > 300) {
        scrollTopBtn.classList.add('active');
    } else {
        scrollTopBtn.classList.remove('active');
    }
});

scrollTopBtn.addEventListener('click', () => {
    window.scrollTo({
        top: 0,
        behavior: 'smooth'
    });
});

// ===== MENU FILTERING =====
const menuTabs = document.querySelectorAll('.menu-tab');
const menuCards = document.querySelectorAll('.menu-card');

menuTabs.forEach(tab => {
    tab.addEventListener('click', () => {
        // Remove active class from all tabs
        menuTabs.forEach(t => t.classList.remove('active'));
        // Add active class to clicked tab
        tab.classList.add('active');

        const filterValue = tab.getAttribute('data-filter');

        menuCards.forEach(card => {
            if (filterValue === 'all' || card.getAttribute('data-category') === filterValue) {
                card.style.display = 'block';
                card.style.animation = 'fadeUp 0.6s ease';
            } else {
                card.style.display = 'none';
            }
        });
    });
});

// Set 'all' as active by default
document.querySelector('[data-filter="all"]').classList.add('active');

// ===== ADD TO CART FUNCTIONALITY =====
const cardButtons = document.querySelectorAll('.card-btn');

cardButtons.forEach(button => {
    button.addEventListener('click', (e) => {
        e.preventDefault();
        
        const card = button.closest('.menu-card');
        const itemName = card.querySelector('.card-content h3').textContent;
        const itemPrice = card.querySelector('.price').textContent;
        
        // Get WhatsApp phone number
        const whatsappNumber = '919506425900';
        const message = `Hello! I would like to order:\n${itemName} (${itemPrice})`;
        const whatsappUrl = `https://wa.me/${whatsappNumber}?text=${encodeURIComponent(message)}`;
        
        // Show confirmation
        showNotification(`${itemName} added! Opening WhatsApp...`);
        
        // Open WhatsApp after short delay
        setTimeout(() => {
            window.open(whatsappUrl, '_blank');
        }, 500);
    });
});

// ===== NOTIFICATION FUNCTION =====
function showNotification(message) {
    const notification = document.createElement('div');
    notification.style.cssText = `
        position: fixed;
        top: 80px;
        right: 20px;
        background: linear-gradient(135deg, #D4AF37, #C9A961);
        color: white;
        padding: 15px 25px;
        border-radius: 10px;
        box-shadow: 0 10px 30px rgba(0,0,0,0.2);
        font-weight: 600;
        z-index: 2000;
        animation: slideInRight 0.3s ease;
        max-width: 300px;
    `;
    notification.textContent = message;
    document.body.appendChild(notification);
    
    setTimeout(() => {
        notification.style.animation = 'slideOutRight 0.3s ease';
        setTimeout(() => {
            notification.remove();
        }, 300);
    }, 3000);
}

// ===== AOS INITIALIZATION =====
AOS.init({
    duration: 800,
    easing: 'ease-in-out',
    once: true,
    offset: 100
});

// ===== ACTIVE NAVIGATION LINK ON SCROLL =====
const sections = document.querySelectorAll('section[id]');

window.addEventListener('scroll', () => {
    let current = '';
    
    sections.forEach(section => {
        const sectionTop = section.offsetTop;
        const sectionHeight = section.clientHeight;
        
        if (scrollY >= sectionTop - 200) {
            current = section.getAttribute('id');
        }
    });

    navLinks.forEach(link => {
        link.classList.remove('active');
        if (link.getAttribute('href').slice(1) === current) {
            link.classList.add('active');
        }
    });
});

// ===== FORM VALIDATION (If needed) =====
function validateEmail(email) {
    const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    return re.test(email);
}

// ===== COUNTER ANIMATION =====
function animateCounters() {
    const counters = document.querySelectorAll('.counter');
    
    counters.forEach(counter => {
        const target = parseInt(counter.getAttribute('data-target'));
        const increment = target / 50;
        let count = 0;

        const updateCount = () => {
            if (count < target) {
                count += increment;
                counter.textContent = Math.floor(count) + '+';
                setTimeout(updateCount, 30);
            } else {
                counter.textContent = target + '+';
            }
        };

        updateCount();
    });
}

// ===== LAZY LOADING IMAGES =====
if ('IntersectionObserver' in window) {
    const imageObserver = new IntersectionObserver((entries, observer) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                const img = entry.target;
                img.src = img.dataset.src;
                img.classList.add('loaded');
                observer.unobserve(img);
            }
        });
    });

    document.querySelectorAll('img[data-src]').forEach(img => imageObserver.observe(img));
}

// ===== KEYBOARD NAVIGATION =====
document.addEventListener('keydown', (e) => {
    // ESC to close mobile menu
    if (e.key === 'Escape') {
        navMenu.classList.remove('active');
        hamburger.classList.remove('active');
    }
});

// ===== PERFORMANCE OPTIMIZATION =====
// Debounce scroll events
function debounce(func, wait) {
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

// ===== PRELOAD CRITICAL RESOURCES =====
window.addEventListener('load', () => {
    // Prefetch links
    const links = document.querySelectorAll('a[href^="#"]');
    links.forEach(link => {
        link.addEventListener('mouseover', () => {
            const target = link.getAttribute('href');
            // Preload animation
        });
    });
});

// ===== MOBILE MENU AUTO-CLOSE ON OUTSIDE CLICK =====
document.addEventListener('click', (e) => {
    if (!e.target.closest('.nav-container')) {
        navMenu.classList.remove('active');
        hamburger.classList.remove('active');
    }
});

// ===== ANIMATION FOR MENU CARDS ON HOVER =====
const menuCard = document.querySelectorAll('.menu-card');
menuCard.forEach(card => {
    card.addEventListener('mouseenter', function() {
        this.style.transform = 'translateY(-15px)';
    });
    
    card.addEventListener('mouseleave', function() {
        this.style.transform = 'translateY(0)';
    });
});

// ===== WHATSAPP INTEGRATION =====
const whatsappBtn = document.querySelector('.whatsapp-btn');
whatsappBtn.addEventListener('click', (e) => {
    e.preventDefault();
    const phoneNumber = '919506425900';
    const message = 'Hello Jaiswal Ji Tea Hub! I would like to know more about your menu and services.';
    const whatsappUrl = `https://wa.me/${phoneNumber}?text=${encodeURIComponent(message)}`;
    window.open(whatsappUrl, '_blank');
});

// ===== INITIALIZE TOOLTIPS =====
function initializeTooltips() {
    const tooltips = document.querySelectorAll('[title]');
    tooltips.forEach(element => {
        element.addEventListener('mouseenter', function() {
            const title = this.getAttribute('title');
            const tooltip = document.createElement('div');
            tooltip.className = 'tooltip';
            tooltip.textContent = title;
            tooltip.style.cssText = `
                position: absolute;
                background: #8B7355;
                color: white;
                padding: 5px 10px;
                border-radius: 5px;
                font-size: 12px;
                pointer-events: none;
                z-index: 1000;
            `;
            document.body.appendChild(tooltip);
            
            const rect = this.getBoundingClientRect();
            tooltip.style.top = (rect.top - tooltip.offsetHeight - 10) + 'px';
            tooltip.style.left = (rect.left + rect.width / 2 - tooltip.offsetWidth / 2) + 'px';
            
            this.addEventListener('mouseleave', () => {
                tooltip.remove();
            });
        });
    });
}

// Initialize tooltips when DOM is ready
document.addEventListener('DOMContentLoaded', initializeTooltips);

// ===== CUSTOM ANIMATIONS =====
const style = document.createElement('style');
style.textContent = `
    @keyframes slideInRight {
        from {
            opacity: 0;
            transform: translateX(30px);
        }
        to {
            opacity: 1;
            transform: translateX(0);
        }
    }
    
    @keyframes slideOutRight {
        from {
            opacity: 1;
            transform: translateX(0);
        }
        to {
            opacity: 0;
            transform: translateX(30px);
        }
    }

    .nav-link.active {
        color: #D4AF37;
    }

    .nav-link.active::after {
        width: 100%;
    }
`;
document.head.appendChild(style);

// ===== INTERSECTION OBSERVER FOR ANIMATIONS =====
const observerOptions = {
    threshold: 0.1,
    rootMargin: '0px 0px -100px 0px'
};

const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.style.opacity = '1';
            entry.target.style.transform = 'translateY(0)';
        }
    });
}, observerOptions);

// ===== PAGE PERFORMANCE MONITORING =====
if (window.performance && window.performance.timing) {
    window.addEventListener('load', () => {
        const perfData = window.performance.timing;
        const pageLoadTime = perfData.loadEventEnd - perfData.navigationStart;
        console.log('Page load time: ' + pageLoadTime + 'ms');
    });
}

// ===== ACCESSIBILITY ENHANCEMENTS =====
// Ensure keyboard navigation works properly
document.addEventListener('keydown', (e) => {
    if (e.key === 'Tab') {
        document.body.classList.add('keyboard-nav');
    }
});

document.addEventListener('mousedown', () => {
    document.body.classList.remove('keyboard-nav');
});

// ===== DYNAMIC YEAR IN FOOTER =====
const currentYear = new Date().getFullYear();
const footerYear = document.querySelector('.footer-bottom p');
if (footerYear) {
    footerYear.textContent = `© ${currentYear} Jaiswal Ji Tea Hub. All rights reserved.`;
}

// ===== CONSOLE WELCOME MESSAGE =====
console.log('%cWelcome to Jaiswal Ji Tea Hub! ☕', 'font-size: 20px; color: #8B7355; font-weight: bold;');
console.log('%cFor inquiries, contact: +91 9506425900', 'font-size: 14px; color: #D4AF37;');

// ===== ERROR HANDLING =====
window.addEventListener('error', (e) => {
    console.error('Error:', e.message);
});

// ===== UNHANDLED PROMISE REJECTION =====
window.addEventListener('unhandledrejection', (e) => {
    console.error('Unhandled Promise Rejection:', e.reason);
});

// ===== EXPORT FUNCTIONS FOR EXTERNAL USE =====
window.jaiswalTeaHub = {
    openWhatsApp: function(message = 'Hello Jaiswal Ji Tea Hub!') {
        const phoneNumber = '919506425900';
        const whatsappUrl = `https://wa.me/${phoneNumber}?text=${encodeURIComponent(message)}`;
        window.open(whatsappUrl, '_blank');
    },
    
    toggleDarkMode: function() {
        themeToggle.click();
    },
    
    scrollToSection: function(sectionId) {
        const section = document.getElementById(sectionId);
        if (section) {
            section.scrollIntoView({ behavior: 'smooth', block: 'start' });
        }
    },
    
    showNotification: showNotification
};
