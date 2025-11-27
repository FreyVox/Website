<!DOCTYPE html>
<html lang="en" id="lang-en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Homesafe Community Allies - Protecting Ottawa Homes</title>
    <meta name="description" content="Neighbor-serving-neighbor security patrols for Ottawa neighborhoods. Join for exclusive access to pricing, articles, and premium services.">
    <meta name="keywords" content="Ottawa residential security patrols, home protection, burglary prevention, membership security, Stripe subscription">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css" rel="stylesheet">
    <script src="https://js.stripe.com/v3/"></script> <!-- Stripe JS -->
    <style>
        :root {
            --primary-blue: #007BFF; /* Trust */
            --safety-green: #28A745; /* Safety */
            --dark-blue: #003087; /* Header background */
            --light-gray: #f8f9fa; /* Sections */
            --overlay-bg: rgba(0, 0, 0, 0.7); /* Paywall overlay */
        }
        body { font-family: 'Arial', sans-serif; line-height: 1.6; }
        .logo-svg { width: 80px; height: 80px; } /* 2x2 inches approx at 40dpi */
        .hero { background: linear-gradient(to right, var(--primary-blue), var(--dark-blue)); color: white; padding: 100px 0; }
        .cta-box { background: var(--safety-green); color: white; padding: 40px; border-radius: 10px; }
        .service-card { border-left: 4px solid var(--primary-blue); }
        .pricing-card { border: 1px solid var(--primary-blue); border-radius: 10px; }
        footer { background: var(--dark-blue); color: white; padding: 20px 0; }
        .paywall-overlay { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--overlay-bg); z-index: 1000; }
        .paywall-overlay.active { display: block; animation: fadeIn 0.3s; }
        .paywall-content { position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%); background: white; padding: 30px; border-radius: 10px; text-align: center; box-shadow: 0 4px 20px rgba(0,0,0,0.2); }
        .bilingual-note { font-size: 0.875em; color: #6c757d; }
        .login-modal .modal-content { border: 2px solid var(--primary-blue); }
        .dashboard-card { border: 1px solid var(--primary-blue); border-radius: 10px; }
        .stripe-element { padding: 10px; border: 1px solid #ddd; border-radius: 5px; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
    </style>
</head>
<body>
    <!-- Language Toggle -->
    <div class="lang-toggle">
        <button class="btn btn-sm btn-outline-light" onclick="switchLang('en')">EN</button>
        <button class="btn btn-sm btn-outline-light" onclick="switchLang('fr')">FR</button>
    </div>

    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary fixed-top">
        <div class="container">
            <a class="navbar-brand" href="#">
                <svg class="logo-svg" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
                    <defs>
                        <linearGradient id="shieldGrad" x1="0%" y1="0%" x2="100%" y2="100%">
                            <stop offset="0%" style="stop-color:#28A745;stop-opacity:1" />
                            <stop offset="100%" style="stop-color:#007BFF;stop-opacity:1" />
                        </linearGradient>
                    </defs>
                    <path d="M20 80 L50 20 L80 80 Z" fill="url(#shieldGrad)" stroke="#007BFF" stroke-width="2"/>
                    <path d="M50 30 L40 50 L50 50 L60 50 Z" fill="white"/>
                    <rect x="45" y="50" width="10" height="20" fill="white"/>
                    <path d="M50 30 L30 50 L70 50 Z" fill="#28A745"/>
                </svg>
                Homesafe Community Allies
            </a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav ms-auto">
                    <li class="nav-item"><a class="nav-link" href="#home">Home</a></li>
                    <li class="nav-item"><a class="nav-link" href="#about">About</a></li>
                    <li class="nav-item"><a class="nav-link" href="#services">Services</a></li>
                    <li class="nav-item"><a class="nav-link" href="#pricing" data-paywall>Pricing</a></li>
                    <li class="nav-item"><a class="nav-link" href="#dashboard" data-paywall>Dashboard</a></li>
                    <li class="nav-item"><a class="nav-link" href="#blog" data-paywall>Blog</a></li>
                    <li class="nav-item"><a class="nav-link" href="#contact">Contact</a></li>
                    <li class="nav-item"><a class="nav-link btn btn-success" href="#" data-bs-toggle="modal" data-bs-target="#loginModal">Login/Sign Up</a></li>
                </ul>
                <div class="bilingual-note ms-3">Français | English</div>
            </div>
        </div>
    </nav>

    <!-- Paywall Overlay -->
    <div class="paywall-overlay" id="paywallOverlay">
        <div class="paywall-content">
            <h4>Unlock Premium Access</h4>
            <p>Discover exclusive pricing, in-depth articles on Ottawa crime trends, and your personalized dashboard. Members enjoy 20-30% crime reduction insights and priority support.</p>
            <button class="btn btn-primary me-2" onclick="showPreview()">Preview Content</button>
            <button class="btn btn-success" data-bs-toggle="modal" data-bs-target="#loginModal">Subscribe Now</button>
        </div>
    </div>

    <!-- Login Modal with Stripe Subscription Form -->
    <div class="modal fade" id="loginModal" tabindex="-1" aria-labelledby="loginModalLabel" aria-hidden="true">
        <div class="modal-dialog modal-lg">
            <div class="modal-content">
                <div class="modal-header">
                    <h5 class="modal-title" id="loginModalLabel">Member Login/Sign Up & Subscribe</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                </div>
                <div class="modal-body">
                    <form id="loginForm">
                        <div class="mb-3">
                            <label for="username" class="form-label">Username</label>
                            <input type="text" class="form-control" id="username" required>
                        </div>
                        <div class="mb-3">
                            <label for="password" class="form-label">Password</label>
                            <input type="password" class="form-control" id="password" required>
                        </div>
                        <button type="submit" class="btn btn-primary">Log In</button>
                    </form>
                    <hr>
                    <h6>Or Sign Up & Subscribe</h6>
                    <form id="subscriptionForm">
                        <div class="mb-3">
                            <label for="email" class="form-label">Email</label>
                            <input type="email" class="form-control" id="email" required>
                        </div>
                        <div class="mb-3">
                            <label class="form-label">Plan</label>
                            <select class="form-control" id="plan">
                                <option value="basic">Basic - $29.99/month</option>
                                <option value="premium">Premium - $49.99/month</option>
                                <option value="community">Community - $19.99/month</option>
                            </select>
                        </div>
                        <div class="mb-3">
                            <label class="form-label">Card Details</label>
                            <div id="card-element" class="stripe-element"></div>
                            <div id="card-errors" class="text-danger"></div>
                        </div>
                        <button type="submit" class="btn btn-success" id="submitBtn">Subscribe Now</button>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- Hero Section -->
    <section id="home" class="hero text-center">
        <div class="container">
            <h1 class="display-4 fw-bold">Protecting Ottawa Homes with Visible, Approachable Patrols</h1>
            <p class="lead">Neighbors Serving Neighbors – Your Trusted Partners in Neighborhood Safety</p>
            <a href="#contact" class="btn btn-light btn-lg">Get Your Free Assessment</a>
        </div>
    </section>

    <!-- About Section -->
    <section id="about" class="py-5 bg-light">
        <div class="container">
            <div class="row align-items-center">
                <div class="col-md-6">
                    <h2>Who We Are</h2>
                    <p>In a city with over 2,500 annual burglaries, Homesafe Community Allies brings proactive deterrence to Ottawa's residential neighborhoods. Starting local and scaling across the capital, we're committed to building safer communities through vehicle-based patrols that feel like an extension of your neighborhood watch.</p>
                    <p>Founded by Ottawa residents for Ottawa families, we blend the OPS CORE strategy's data-driven insights with community collaboration, reducing property crime risks by up to 20%.</p>
                </div>
                <div class="col-md-6">
                    <img src="https://via.placeholder.com/500x300?text=Community+Patrol" alt="Community patrol in Ottawa neighborhood" class="img-fluid rounded">
                </div>
            </div>
        </div>
    </section>

    <!-- Services Section -->
    <section id="services" class="py-5">
        <div class="container">
            <h2 class="text-center mb-5">Our Services</h2>
            <div class="row">
                <div class="col-md-6 col-lg-4 mb-4">
                    <div class="card service-card h-100">
                        <div class="card-body">
                            <h5 class="card-title">Regular Vehicle Patrols</h5>
                            <p class="card-text">High-visibility checks to prevent break-ins and home invasions, tailored to your schedule.</p>
                        </div>
                    </div>
                </div>
                <div class="col-md-6 col-lg-4 mb-4">
                    <div class="card service-card h-100">
                        <div class="card-body">
                            <h5 class="card-title">Customized Coverage</h5>
                            <p class="card-text" data-paywall>Available to members: Tailored to at-risk residential areas using OPS data for maximum effectiveness.</p>
                        </div>
                    </div>
                </div>
                <div class="col-md-6 col-lg-4 mb-4">
                    <div class="card service-card h-100">
                        <div class="card-body">
                            <h5 class="card-title">Community Collaboration</h5>
                            <p class="card-text" data-paywall>Available to members: Partnering with residents and associations for shared security and intelligence.</p>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row mt-4">
                <div class="col-md-6 col-lg-4 mb-4">
                    <div class="card service-card h-100">
                        <div class="card-body">
                            <h5 class="card-title">Scalable Options</h5>
                            <p class="card-text" data-paywall>Available to members: Expand to alarm response, consultations, and multi-location services.</p>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Pricing Section (Paywalled) -->
    <section id="pricing" class="py-5 bg-light" data-paywall>
        <div class="container">
            <h2 class="text-center mb-5">Pricing Plans</h2>
            <div class="row">
                <div class="col-md-4 mb-4">
                    <div class="card pricing-card text-center">
                        <div class="card-header bg-primary text-white">
                            <h5>Basic</h5>
                        </div>
                        <div class="card-body">
                            <h3>$29.99<span class="small">/month</span></h3>
                            <ul class="list-unstyled">
                                <li>2-3 patrols/week</li>
                                <li>Basic deterrence</li>
                                <li>Email alerts</li>
                            </ul>
                        </div>
                    </div>
                </div>
                <div class="col-md-4 mb-4">
                    <div class="card pricing-card text-center border-primary">
                        <div class="card-header bg-primary text-white">
                            <h5>Premium</h5>
                        </div>
                        <div class="card-body">
                            <h3>$49.99<span class="small">/month</span></h3>
                            <ul class="list-unstyled">
                                <li>Daily patrols</li>
                                <li>Personal reports</li>
                                <li>Priority response</li>
                            </ul>
                        </div>
                    </div>
                </div>
                <div class="col-md-4 mb-4">
                    <div class="card pricing-card text-center">
                        <div class="card-header bg-success text-white">
                            <h5>Community</h5>
                        </div>
                        <div class="card-body">
                            <h3>$19.99<span class="small">/household (min. 10)</span></h3>
                            <ul class="list-unstyled">
                                <li>Shared patrols</li>
                                <li>Group discounts</li>
                                <li>Community meetings</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
            <div class="text-center mt-4">
                <a href="#contact" class="btn btn-primary btn-lg">Choose Your Plan</a>
            </div>
        </div>
    </section>

    <!-- Member Dashboard Section (Paywalled) -->
    <section id="dashboard" class="py-5" data-paywall>
        <div class="container">
            <h2 class="text-center mb-5">Member Dashboard</h2>
            <div class="row">
                <div class="col-md-6">
                    <div class="card dashboard-card mb-4">
                        <div class="card-header bg-primary text-white">
                            <h5>Subscription Status</h5>
                        </div>
                        <div class="card-body">
                            <p><strong>Plan:</strong> Premium</p>
                            <p><strong>Next Billing:</strong> November 24, 2025</p>
                            <p><strong>Status:</strong> Active</p>
                            <button class="btn btn-success">Manage Subscription</button>
                        </div>
                    </div>
                </div>
                <div class="col-md-6">
                    <div class="card dashboard-card mb-4">
                        <div class="card-header bg-success text-white">
                            <h5>Recent Patrols</h5>
                        </div>
                        <div class="card-body">
                            <table class="table table-sm">
                                <thead>
                                    <tr>
                                        <th>Date</th>
                                        <th>Neighborhood</th>
                                        <th>Status</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr>
                                        <td>Oct 23, 2025</td>
                                        <td>Kanata</td>
                                        <td>Completed</td>
                                    </tr>
                                    <tr>
                                        <td>Oct 22, 2025</td>
                                        <td>Orleans</td>
                                        <td>In Progress</td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-12">
                    <div class="card dashboard-card">
                        <div class="card-header bg-info text-white">
                            <h5>Account Settings</h5>
                        </div>
                        <div class="card-body">
                            <p><strong>Email:</strong> user@example.com</p>
                            <p><strong>Phone:</strong> (613) 555-1234</p>
                            <button class="btn btn-primary">Update Profile</button>
                            <button class="btn btn-danger">Cancel Membership</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- Blog Section (Paywalled) -->
    <section id="blog" class="py-5" data-paywall>
        <div class="container">
            <h2 class="text-center mb-5">Latest Insights</h2>
            <article class="blog-post">
                <h3>Ottawa's Quiet Shadow: How Neighborhood Patrols Are Rekindling Peace in Our Capital's Homes</h3>
                <p>As the leaves turn golden in Kanata's tree-lined streets and the first chill settles over Orleans' family enclaves, Ottawa’s vibrant neighborhoods hum with the familiar rhythm of daily life—children biking home from school, neighbors chatting over fences, and the soft glow of porch lights welcoming families after a long day. Yet beneath this serene facade lies a lingering unease: the fear of an unseen intruder slipping through the night, turning a sanctuary into a scene of violation. In a city where over 2,500 break-and-enter incidents are reported each year—a number that, while stable, still evokes heart-pounding scenarios of shattered glass and stolen heirlooms—the emotional toll of property crime weighs heavily on our middle- and upper-middle-class residents. These aren't just statistics; they're the disrupted bedtime stories for children, the anxious double-checks of locked doors, and the quiet erosion of the trust that makes our homes feel like havens.</p>
                <!-- Full article body from previous response -->
                <div class="table-responsive mt-4">
                    <table class="table table-striped">
                        <thead>
                            <tr>
                                <th>Ottawa Crime Trend</th>
                                <th>2021-2023 Average</th>
                                <th>2025 Projection</th>
                                <th>Impact on Residents</th>
                                <th>Homesafe Mitigation</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>Break-and-Enter Incidents</td>
                                <td>~2,500/year</td>
                                <td>Stable (2,400-2,600)</td>
                                <td>Emotional distress from opportunistic thefts in suburbs</td>
                                <td>Visible patrols reduce by 20-30% via deterrence</td>
                            </tr>
                            <!-- Full table rows as needed -->
                        </tbody>
                    </table>
                </div>
                <p>As October's harvest moon rises over the Ottawa River, let it illuminate not shadows, but solidarity. Homesafe Community Allies beckons: in a city of quiet strengths, let's author the next chapter of unbreakable homes.</p>
                <a href="#contact" class="btn btn-primary">Read More & Join</a>
            </article>
        </div>
    </section>

    <!-- Contact Section -->
    <section id="contact" class="py-5">
        <div class="container">
            <h2 class="text-center mb-5">Get in Touch</h2>
            <div class="row">
                <div class="col-md-6">
                    <h5>Contact Info</h5>
                    <p><strong>Email:</strong> info@homesafeallies.com</p>
                    <p><strong>Phone:</strong> (613) 555-1234</p>
                    <p><strong>Address:</strong> Ottawa, ON</p>
                    <p><strong>Social:</strong> @HomesafeOttawa</p>
                </div>
                <div class="col-md-6">
                    <form>
                        <div class="mb-3">
                            <label for="name" class="form-label">Name</label>
                            <input type="text" class="form-control" id="name">
                        </div>
                        <div class="mb-3">
                            <label for="email" class="form-label">Email</label>
                            <input type="email" class="form-control" id="email">
                        </div>
                        <div class="mb-3">
                            <label for="message" class="form-label">Message</label>
                            <textarea class="form-control" id="message" rows="3"></textarea>
                        </div>
                        <button type="submit" class="btn btn-primary">Send Message</button>
                    </form>
                </div>
            </div>
        </div>
    </section>

    <!-- Footer -->
    <footer>
        <div class="container text-center">
            <p>&copy; 2025 Homesafe Community Allies. Licensed & Insured. Serving Ottawa Neighborhoods.</p>
        </div>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        const stripe = Stripe('pk_test_51...your_key_here...');
        const elements = stripe.elements();
        const cardElement = elements.create('card');
        cardElement.mount('#card-element');

        let isLoggedIn = sessionStorage.getItem('isLoggedIn') === 'true';

        document.addEventListener('DOMContentLoaded', () => {
            const paywallElements = document.querySelectorAll('[data-paywall]');
            const paywallOverlay = document.getElementById('paywallOverlay');
            const loginForm = document.getElementById('loginForm');
            const subscriptionForm = document.getElementById('subscriptionForm');
            const cardErrors = document.getElementById('card-errors');
            const submitBtn = document.getElementById('submitBtn');

            if (!isLoggedIn) {
                paywallElements.forEach(el => el.style.display = 'none');
                paywallOverlay.classList.add('active');
            }

            loginForm.addEventListener('submit', (e) => {
                e.preventDefault();
                const username = document.getElementById('username').value;
                const password = document.getElementById('password').value;
                if (username === 'member' && password === 'pass123') {
                    sessionStorage.setItem('isLoggedIn', 'true');
                    isLoggedIn = true;
                    paywallOverlay.classList.remove('active');
                    paywallElements.forEach(el => el.style.display = 'block');
                    bootstrap.Modal.getInstance(document.getElementById('loginModal')).hide();
                } else {
                    alert('Invalid credentials.');
                }
            });

            subscriptionForm.addEventListener('submit', async (e) => {
                e.preventDefault();
                submitBtn.disabled = true;
                submitBtn.textContent = 'Processing...';

                const {error, paymentMethod} = await stripe.createPaymentMethod({
                    type: 'card',
                    card: cardElement,
                });

                if (error) {
                    cardErrors.textContent = error.message;
                    submitBtn.disabled = false;
                    submitBtn.textContent = 'Subscribe Now';
                } else {
                    const response = await fetch('http://localhost:3000/subscribe', {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify({ paymentMethodId: paymentMethod.id, email: document.getElementById('email').value, plan: document.getElementById('plan').value }),
                    });
                    const result = await response.json();
                    if (result.success) {
                        sessionStorage.setItem('isLoggedIn', 'true');
                        isLoggedIn = true;
                        paywallOverlay.classList.remove('active');
                        paywallElements.forEach(el => el.style.display = 'block');
                        bootstrap.Modal.getInstance(document.getElementById('loginModal')).hide();
                        alert('Subscription successful! Welcome to Homesafe.');
                    } else {
                        cardErrors.textContent = result.error;
                        submitBtn.disabled = false;
                        submitBtn.textContent = 'Subscribe Now';
                    }
                }
            });

            window.showPreview = () => {
                const preview = document.createElement('div');
                preview.innerHTML = '<h5>Article Preview</h5><p>Over 2,500 annual burglaries in Ottawa... (Full access for members)</p>';
                preview.className = 'alert alert-info';
                document.querySelector('.paywall-content').appendChild(preview);
                setTimeout(() => preview.remove(), 5000);
            };

            document.querySelectorAll('[data-paywall]').forEach(link => {
                link.addEventListener('click', (e) => {
                    if (!isLoggedIn) {
                        e.preventDefault();
                        paywallOverlay.classList.add('active');
                    }
                });
            });

            document.addEventListener('keydown', (e) => {
                if (e.key === 'Escape' && paywallOverlay.classList.contains('active')) {
                    paywallOverlay.classList.remove('active');
                }
            });
        });
    </script>
</body>
</html>
