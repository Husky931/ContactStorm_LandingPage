<script>
  import { onMount } from 'svelte';

  let scrollY = 0;
  let mobileMenuOpen = false;
  let mounted = false;
  let activeFaq = null;
  let darkMode = true;

  const plans = [
    {
      name: 'Pay As You Go',
      price: null,
      badge: 'Flexible',
      features: [
        'No commitment',
        'Pay per submission',
        'Email support',
        "1 AI bot running",
        '1 messaging template',
        "1 live viewing window",
        '3 Lead Profiles',
        "Real-time tracking",
        'Basic analytics'
      ],
      cta: 'Get Started',
      highlighted: false
    },
    {
      name: 'Mini Storm',
      price: 29,
      period: '/week',
      badge: null,
      submissions: '140/week',
      daily: '20/day',
      bots: 1,
      windows: 1,
      features: [
        '140 submissions per week',
        '20 submissions per day',
        '1 AI bot running',
        '1 live viewing window',
        '3 messaging templates',
        '5 Lead Profiles',
        'Real-time tracking',
        'Email notifications',
        'Advanced analytics'
      ],
      cta: 'Start Mini Storm',
      highlighted: false
    },
    {
      name: 'Mega Storm',
      price: 49,
      period: '/week',
      badge: 'Most Popular',
      submissions: '280/week',
      daily: '40/day',
      bots: 2,
      windows: 2,
      features: [
        '280 submissions per week',
        '40 submissions per day',
        '2 AI bots running',
        '2 live viewing windows',
        '10 messaging templates',
        '10 Lead Profiles',
        'Real-time tracking',
        'Priority support',
        'Advanced analytics'
      ],
      cta: 'Start Mega Storm',
      highlighted: true
    },
    {
      name: 'Contact Storm',
      price: 99,
      period: '/week',
      badge: 'Maximum Power',
      submissions: '700/week',
      daily: '100/day',
      bots: 4,
      windows: 3,
      features: [
        '700 submissions per week',
        '100 submissions per day',
        '4 AI bots simultaneously',
        '4 live viewing windows',
        "Unlimited Lead Profiles",
        'Unlimited messaging templates',
        "Download all target business contacts (email, phone, whatsapp, etc.) as a CSV file",
        'Real-time tracking',
        'Dedicated support',
        'Full analytics suite',
        'Priority support'
      ],
      cta: 'Start Contact Storm',
      highlighted: false
    }
  ];

  const steps = [
    {
      num: '01',
      title: 'Create Your Lead Profile',
      desc: 'Tell us about your business, your ideal customers, and what services you offer. Our AI uses this to give you the best possible leads.'
    },
    {
      num: '02',
      title: 'AI Finds Your Targets',
      desc: 'Our intelligent engine scans thousands of business domains, marketplaces, business directories, and more to find companies that fit your lead profile. Important - we can target geo specific locations, if you want to target customers from certain areas. '
    },
    {
      num: '03',
      title: 'Watch Leads Roll In with Real-Time Live Viewing',
      desc: 'Sit back and watch in real-time as our bots contact businesses for you. Track every submission, message, and response live. Everything is out in the open for you to see.'
    }
  ];

  const features = [
    {
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><circle cx="12" cy="12" r="3"/><path d="M12 1v2M12 21v2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42M1 12h2M21 12h2M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42"/></svg>`,
      title: 'Real-Time Live View',
      desc: 'Watch your AI bots fill out contact forms in real-time. See every submission as it happens across multiple simultaneous windows.'
    },
    {
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M21 15a2 2 0 01-2 2H7l-4 4V5a2 2 0 012-2h14a2 2 0 012 2z"/></svg>`,
      title: 'Custom Messaging',
      desc: 'Create multiple message templates, toggle between them, and track which messages generate the most responses and conversions.'
    },
    {
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M9 19v-6a2 2 0 00-2-2H5a2 2 0 00-2 2v6a2 2 0 002 2h2a2 2 0 002-2zm0 0V9a2 2 0 012-2h2a2 2 0 012 2v10m-6 0a2 2 0 002 2h2a2 2 0 002-2m0 0V5a2 2 0 012-2h2a2 2 0 012 2v14a2 2 0 01-2 2h-2a2 2 0 01-2-2z"/></svg>`,
      title: 'Smart Analytics',
      desc: 'Track success rates, monitor which templates perform best, and optimize your outreach strategy with data-driven insights.'
    },
    {
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M17 21v-2a4 4 0 00-4-4H5a4 4 0 00-4 4v2"/><circle cx="9" cy="7" r="4"/><path d="M23 21v-2a4 4 0 00-3-3.87M16 3.13a4 4 0 010 7.75"/></svg>`,
      title: 'AI Lead Matching',
      desc: 'Our AI analyzes your profile and intelligently matches you with businesses most likely to convert into paying customers.'
    },
    {
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><rect x="2" y="3" width="20" height="14" rx="2" ry="2"/><line x1="8" y1="21" x2="16" y2="21"/><line x1="12" y1="17" x2="12" y2="21"/></svg>`,
      title: 'Multi-Bot Power',
      desc: 'Run up to 4 AI bots simultaneously, contacting multiple businesses at once. Scale your outreach without scaling your effort.'
    },
    {
      icon: `<svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5"><path d="M4 4h16c1.1 0 2 .9 2 2v12c0 1.1-.9 2-2 2H4c-1.1 0-2-.9-2-2V6c0-1.1.9-2 2-2z"/><polyline points="22,6 12,13 2,6"/></svg>`,
      title: 'Email Integration',
      desc: 'Track all conversations directly on our platform or in your own email inbox. Never miss a lead response again.'
    }
  ];

  const faqs = [
    {
      q: 'How does Contact Storm generate leads?',
      a: 'You create a lead profile describing your business and ideal customers. Our AI then finds relevant business domains, fills out their contact forms with your custom messaging, and tracks all responses. You can watch the entire process in real-time.'
    },
    {
      q: 'Can I watch the bots in real-time?',
      a: 'Yes! This is one of our standout features. Depending on your plan, you get 1-4 live viewing windows where you can watch your personal army filling out contact forms as they happen. It\'s like having a virtual sales team you can observe.'
    },
    {
      q: 'What are messaging templates?',
      a: 'Messaging templates are custom messages you write that our bots use when filling out contact forms. You can create multiple templates, A/B test them, and track which ones get the best response rates.'
    },
    {
      q: 'How do I track responses?',
      a: 'All conversations can be tracked directly on our platform dashboard. You can also have responses forwarded to your email inbox so you never miss a potential lead.'
    },
    {
      q: 'Can I switch plans anytime?',
      a: 'Absolutely. You can upgrade, downgrade, or switch to Pay As You Go at any time. Changes take effect at the start of your next billing cycle.'
    }
  ];

  function toggleFaq(index) {
    activeFaq = activeFaq === index ? null : index;
  }

  function applyTheme() {
    document.documentElement.setAttribute('data-theme', darkMode ? 'dark' : 'light');
  }

  function toggleTheme() {
    darkMode = !darkMode;
    localStorage.setItem('theme', darkMode ? 'dark' : 'light');
    applyTheme();
  }

  function reveal(node) {
    return { destroy() {} };
  }

  // Simulated live form-fill demo data
  let demoStep = 0;
  const demoFields = [
    { label: 'Company', value: 'IsoBeam Laserworks' },
    { label: 'Name', value: 'Jonathan Cruz' },
    { label: 'Email', value: 'jonathan@isobeamlaserwork.com' },
    { label: 'Message', value: 'Hi, we are manufacturers of carbon laser engraving and cutting machines...' }
  ];

  onMount(() => {
    const saved = localStorage.getItem('theme');
    darkMode = saved ? saved === 'dark' : true;
    applyTheme();

    const interval = setInterval(() => {
      demoStep = (demoStep + 1) % (demoFields.length + 2);
    }, 1800);
    return () => clearInterval(interval);
  });
</script>

<svelte:window bind:scrollY />
<svelte:head>
  <title>Contact Storm - AI-Powered Lead Generation on Autopilot</title>
  <meta name="description" content="Generate leads automatically with AI. Contact Storm finds businesses, fills out contact forms, and delivers leads while you watch in real-time." />
</svelte:head>

<!-- NAVBAR -->
<nav class="nav" class:scrolled={scrollY > 50}>
  <div class="nav-inner">
    <a href="/" class="logo">
      <span class="logo-icon">
        <svg viewBox="0 0 28 28" fill="none">
          <path d="M14 2L4 8v12l10 6 10-6V8L14 2z" fill="url(#logoGrad)" opacity="0.15"/>
          <path d="M14 2L4 8v12l10 6 10-6V8L14 2z" stroke="url(#logoGrad)" stroke-width="1.5" fill="none"/>
          <path d="M17.5 9L12 15l-1.5-1.5M14 15l4-4" stroke="url(#logoGrad)" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/>
          <defs>
            <linearGradient id="logoGrad" x1="4" y1="2" x2="24" y2="26">
              <stop stop-color="#007AFF"/>
              <stop offset="1" stop-color="#00C6FF"/>
            </linearGradient>
          </defs>
        </svg>
      </span>
      <span class="logo-text">Contact<span class="logo-accent">Storm</span></span>
    </a>

    <div class="nav-links" class:open={mobileMenuOpen}>
      <a href="#features" on:click={() => mobileMenuOpen = false}>Features</a>
      <a href="#how-it-works" on:click={() => mobileMenuOpen = false}>How It Works</a>
      <a href="#pricing" on:click={() => mobileMenuOpen = false}>Pricing</a>
      <a href="#faq" on:click={() => mobileMenuOpen = false}>FAQ</a>
    </div>

    <div class="nav-actions">
      <button class="theme-toggle" on:click={toggleTheme} aria-label="Toggle theme">
        {#if darkMode}
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="5"/><path d="M12 1v2M12 21v2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42M1 12h2M21 12h2M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42"/></svg>
        {:else}
          <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 12.79A9 9 0 1111.21 3 7 7 0 0021 12.79z"/></svg>
        {/if}
      </button>
      <a href="#" class="btn-ghost">Log In</a>
      <a href="#" class="btn-primary-sm">Get Started</a>
    </div>

    <button class="theme-toggle mobile-theme" on:click={toggleTheme} aria-label="Toggle theme">
      {#if darkMode}
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="5"/><path d="M12 1v2M12 21v2M4.22 4.22l1.42 1.42M18.36 18.36l1.42 1.42M1 12h2M21 12h2M4.22 19.78l1.42-1.42M18.36 5.64l1.42-1.42"/></svg>
      {:else}
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M21 12.79A9 9 0 1111.21 3 7 7 0 0021 12.79z"/></svg>
      {/if}
    </button>
    <button class="mobile-toggle" on:click={() => mobileMenuOpen = !mobileMenuOpen} aria-label="Toggle menu">
      <span class="hamburger" class:open={mobileMenuOpen}>
        <span></span><span></span><span></span>
      </span>
    </button>
  </div>
</nav>

<!-- HERO -->
<section class="hero">
  <div class="hero-bg">
    <div class="grid-pattern"></div>
    <div class="hero-glow"></div>
    <div class="hero-glow-2"></div>
  </div>

  <div class="hero-content">
    <div class="hero-badge animate-in" use:reveal>
      <span class="badge-dot"></span>
      AI-Powered Lead Generation
    </div>

    <h1 class="hero-title animate-in" use:reveal>
      Generate Leads on<br/>
      <span class="gradient-text">Complete Autopilot</span>
    </h1>

    <p class="hero-subtitle animate-in" use:reveal>
      Create your customer target lead profile, and let our AI match you with target businesses from any specific location in the world. Then craft personalized messages, 
      and our army will contact the businesses for you. You watch it all happen in real-time.
    </p>

    <div class="hero-ctas animate-in" use:reveal>
      <a href="#pricing" class="btn-primary">
        Start Generating Leads
        <svg viewBox="0 0 20 20" fill="currentColor" class="btn-arrow"><path fill-rule="evenodd" d="M3 10a.75.75 0 01.75-.75h10.638L10.23 5.29a.75.75 0 111.04-1.08l5.5 5.25a.75.75 0 010 1.08l-5.5 5.25a.75.75 0 11-1.04-1.08l4.158-3.96H3.75A.75.75 0 013 10z"/></svg>
      </a>
      <a href="#how-it-works" class="btn-secondary">
        See How It Works
      </a>
    </div>

    <div class="hero-stats animate-in" use:reveal>
      <div class="stat">
        <span class="stat-num">700+</span>
        <span class="stat-label">Weekly submissions</span>
      </div>
      <div class="stat-divider"></div>
      <div class="stat">
        <span class="stat-num">4x</span>
        <span class="stat-label">Simultaneous bots</span>
      </div>
      <div class="stat-divider"></div>
      <div class="stat">
        <span class="stat-num">24/7</span>
        <span class="stat-label">Automated outreach</span>
      </div>
    </div>
  </div>

  <!-- Hero Visual: Live Demo Mock -->
  <div class="hero-visual animate-in" use:reveal>
    <div class="demo-browser">
      <div class="browser-header">
        <div class="browser-dots">
          <span class="dot red"></span>
          <span class="dot yellow"></span>
          <span class="dot green"></span>
        </div>
        <div class="browser-url">
          <svg viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="1.5" class="lock-icon"><rect x="3" y="7" width="10" height="7" rx="1.5"/><path d="M5 7V5a3 3 0 016 0v2"/></svg>
          targetcustomer.com/contact-us
        </div>
        <div class="live-badge">
          <span class="live-dot"></span>
          LIVE
        </div>
      </div>
      <div class="browser-body">
        <div class="demo-form">
          <div class="demo-form-title">Get In Touch</div>
          {#each demoFields as field, i}
            <div class="demo-field" class:active={demoStep === i} class:filled={demoStep > i}>
              <label>{field.label}</label>
              <div class="demo-input">
                {#if demoStep > i}
                  <span class="typed-text">{field.value}</span>
                {:else if demoStep === i}
                  <span class="typing-text">{field.value.substring(0, Math.min(field.value.length, 28))}<span class="cursor-blink">|</span></span>
                {:else}
                  <span class="placeholder-text"></span>
                {/if}
              </div>
            </div>
          {/each}
          <div class="demo-submit" class:ready={demoStep >= demoFields.length} class:sent={demoStep > demoFields.length}>
            {demoStep > demoFields.length ? '✓ Submitted!' : 'Send Message'}
          </div>
        </div>
        <div class="bot-indicator">
          <div class="bot-avatar">
            <svg viewBox="0 0 24 24" fill="none" stroke="var(--brand)" stroke-width="1.5"><rect x="3" y="8" width="18" height="12" rx="2"/><path d="M12 8V5M7 8V6M17 8V6"/><circle cx="8.5" cy="14" r="1.5" fill="var(--brand)"/><circle cx="15.5" cy="14" r="1.5" fill="var(--brand)"/></svg>
          </div>
          <div class="bot-info">
            <span class="bot-name">Storm Bot #1</span>
            <span class="bot-status">Filling form...</span>
          </div>
        </div>
      </div>
    </div>
    <div class="floating-card card-leads">
      <div class="fc-icon">
        <svg viewBox="0 0 24 24" fill="none" stroke="#10B981" stroke-width="2"><polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/><polyline points="16 7 22 7 22 13"/></svg>
      </div>
      <div class="fc-content">
        <span class="fc-label">Leads Today</span>
        <span class="fc-value">+18</span>
      </div>
    </div>
    <div class="floating-card card-rate">
      <div class="fc-icon">
        <svg viewBox="0 0 24 24" fill="none" stroke="var(--brand)" stroke-width="2"><path d="M22 11.08V12a10 10 0 11-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
      </div>
      <div class="fc-content">
        <span class="fc-label">Delivery Rate</span>
        <span class="fc-value">100%</span>
      </div>
    </div>
  </div>
</section>

<!-- LOGOS/TRUST BAR -->
<section class="trust-bar">
  <div class="container">
    <p class="trust-label animate-in" use:reveal>Trusted by growing businesses worldwide</p>
    <div class="trust-logos animate-in" use:reveal>
      {#each ['Hilliby', 'Shaoyang Yuantong', 'Kenetai-Agro', 'Tansar Capital', 'NGDEM Global Ltd'] as company}
        <div class="trust-logo">{company}</div>
      {/each}
    </div>
  </div>
</section>

<!-- HOW IT WORKS -->
<section id="how-it-works" class="section">
  <div class="container">
    <div class="section-header animate-in" use:reveal>
      <span class="section-tag">How It Works</span>
      <h2 class="section-title section-title-lg">Three Steps to <span class="gradient-text">Automated Leads</span></h2>
      <p class="section-desc">No cold calling. No manual outreach. Just set up your profile and let Contact Storm do the work for you.</p>
    </div>

    <div class="steps-grid">
      {#each steps as step, i}
        <div class="step-card animate-in" use:reveal style="--delay: {i * 0.15}s">
          <div class="step-num">{step.num}</div>
          <div class="step-line" class:last={i === steps.length - 1}></div>
          <h3>{step.title}</h3>
          <p>{step.desc}</p>
        </div>
      {/each}
    </div>
  </div>
</section>

<!-- WHY IT WORKS -->
<section class="section why-section">
  <div class="container">
    <div class="why-layout animate-in" use:reveal>
      <div class="why-content">
        <span class="section-tag">Why It Works</span>
        <h2 class="section-title section-title-lg">Every Message Lands.<br/><span class="gradient-text">Guaranteed.</span></h2>
        <p class="why-text">
          Unlike email outreach that ends up in spam folders, Contact Storm delivers your message 
          <strong>directly through business contact forms</strong> — meaning 100% delivery, every single time. 
          No spam filters. No junk folders. Your message lands right where it matters.
        </p>
        <p class="why-text">
          Whether they respond depends on your offer and their interest in doing business with you. 
          But our job? <strong>Getting your message in front of them.</strong> That part is done — successfully, every time.
        </p>
      </div>
      <div class="why-visual">
        <div class="delivery-card">
          <div class="delivery-icon">
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5">
              <path d="M22 11.08V12a10 10 0 11-5.93-9.14"/>
              <polyline points="22 4 12 14.01 9 11.01"/>
            </svg>
          </div>
          <div class="delivery-stat">100%</div>
          <div class="delivery-label">Delivery Rate</div>
          <div class="delivery-sub">via contact forms, not email</div>
        </div>
        <div class="delivery-pills">
          <div class="pill pill-green">
            <svg viewBox="0 0 16 16" fill="currentColor"><path d="M8 1a7 7 0 100 14A7 7 0 008 1zm3.7 5.7l-4 4a1 1 0 01-1.4 0l-2-2a1 1 0 011.4-1.4L7 8.58l3.3-3.3a1 1 0 011.4 1.42z"/></svg>
            No Spam Filters
          </div>
          <div class="pill pill-green">
            <svg viewBox="0 0 16 16" fill="currentColor"><path d="M8 1a7 7 0 100 14A7 7 0 008 1zm3.7 5.7l-4 4a1 1 0 01-1.4 0l-2-2a1 1 0 011.4-1.4L7 8.58l3.3-3.3a1 1 0 011.4 1.42z"/></svg>
            No Junk Folder
          </div>
          <div class="pill pill-green">
            <svg viewBox="0 0 16 16" fill="currentColor"><path d="M8 1a7 7 0 100 14A7 7 0 008 1zm3.7 5.7l-4 4a1 1 0 01-1.4 0l-2-2a1 1 0 011.4-1.4L7 8.58l3.3-3.3a1 1 0 011.4 1.42z"/></svg>
            Direct to Inbox
          </div>
        </div>
      </div>
    </div>
  </div>
</section>

<!-- FEATURES -->
<section id="features" class="section features-section">
  <div class="container">
    <div class="section-header animate-in" use:reveal>
      <span class="section-tag">Features</span>
      <h2 class="section-title">Everything You Need to <span class="gradient-text">Scale Outreach</span></h2>
      <p class="section-desc">Powerful tools designed to automate your lead generation pipeline from start to finish.</p>
    </div>

    <div class="features-grid">
      {#each features as feature, i}
        <div class="feature-card animate-in" use:reveal style="--delay: {i * 0.1}s">
          <div class="feature-icon">
            {@html feature.icon}
          </div>
          <h3>{feature.title}</h3>
          <p>{feature.desc}</p>
        </div>
      {/each}
    </div>
  </div>
</section>

<!-- LIVE VIEW SHOWCASE -->
<section class="section showcase-section">
  <div class="container">
    <div class="showcase-layout">
      <div class="showcase-text animate-in" use:reveal>
        <span class="section-tag">Real-Time Viewing</span>
        <h2 class="section-title">Watch Your Army<br/><span class="gradient-text">In Action</span></h2>
        <p class="showcase-desc">
          Unlike any other lead generation tool, Contact Storm gives you a live window 
          into every form submission. Watch as your bots navigate to businesses, fill out 
          their contact forms, and submit your custom messages.
        </p>
        <ul class="showcase-list">
          <li>
            <svg viewBox="0 0 20 20" fill="var(--brand)"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"/></svg>
            Up to 4 bots running simultaneously
          </li>
          <li>
            <svg viewBox="0 0 20 20" fill="var(--brand)"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"/></svg>
            Up to 4 live viewing windows
          </li>
          <li>
            <svg viewBox="0 0 20 20" fill="var(--brand)"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"/></svg>
            See submissions as they happen
          </li>
          <li>
            <svg viewBox="0 0 20 20" fill="var(--brand)"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"/></svg>
            Track success and failure rates live
          </li>
        </ul>
        <a href="#pricing" class="btn-primary">
          See Plans
          <svg viewBox="0 0 20 20" fill="currentColor" class="btn-arrow"><path fill-rule="evenodd" d="M3 10a.75.75 0 01.75-.75h10.638L10.23 5.29a.75.75 0 111.04-1.08l5.5 5.25a.75.75 0 010 1.08l-5.5 5.25a.75.75 0 11-1.04-1.08l4.158-3.96H3.75A.75.75 0 013 10z"/></svg>
        </a>
      </div>
      <div class="showcase-visual animate-in" use:reveal>
        <div class="multi-window">
          {#each [1, 2, 3] as botNum}
            <div class="mini-browser">
              <div class="mini-header">
                <div class="mini-dots"><span></span><span></span><span></span></div>
                <div class="mini-live"><span class="live-dot"></span>Bot #{botNum}</div>
              </div>
              <div class="mini-body">
                <div class="mini-form-line" style="width: {70 + botNum * 8}%; animation-delay: {botNum * 0.3}s"></div>
                <div class="mini-form-line" style="width: {50 + botNum * 12}%; animation-delay: {botNum * 0.3 + 0.2}s"></div>
                <div class="mini-form-line short" style="animation-delay: {botNum * 0.3 + 0.4}s"></div>
                <div class="mini-btn-line" style="animation-delay: {botNum * 0.3 + 0.6}s"></div>
              </div>
            </div>
          {/each}
        </div>
      </div>
    </div>
  </div>
</section>

<!-- PRICING -->
<section id="pricing" class="section pricing-section">
  <div class="container">
    <div class="section-header animate-in" use:reveal>
      <span class="section-tag">Pricing</span>
      <h2 class="section-title">Choose Your <span class="gradient-text">Storm Level</span></h2>
      <p class="section-desc">Start small or go all in. Scale your lead generation as your business grows.</p>
    </div>

    <div class="pricing-grid">
      {#each plans as plan, i}
        <div class="pricing-card animate-in" use:reveal class:highlighted={plan.highlighted} style="--delay: {i * 0.1}s">
          {#if plan.badge}
            <div class="pricing-badge" class:pop={plan.highlighted}>{plan.badge}</div>
          {/if}
          <h3 class="pricing-name">{plan.name}</h3>
          <div class="pricing-price">
            {#if plan.price}
              <span class="price-currency">$</span>
              <span class="price-amount">{plan.price}</span>
              <span class="price-period">{plan.period}</span>
            {:else}
              <span class="price-amount payg">PAYG</span>
            {/if}
          </div>
          {#if plan.bots}
            <div class="pricing-bots">
              <span>{plan.bots} bot{plan.bots > 1 ? 's' : ''}</span>
              <span class="sep">·</span>
              <span>{plan.windows} window{plan.windows > 1 ? 's' : ''}</span>
            </div>
          {/if}
          <ul class="pricing-features">
            {#each plan.features as feature}
              <li>
                <svg viewBox="0 0 20 20" fill="currentColor"><path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z"/></svg>
                {feature}
              </li>
            {/each}
          </ul>
          <a href="#" class="pricing-cta" class:primary={plan.highlighted}>{plan.cta}</a>
        </div>
      {/each}
    </div>
  </div>
</section>

<!-- FAQ -->
<section id="faq" class="section">
  <div class="container container-sm">
    <div class="section-header animate-in" use:reveal>
      <span class="section-tag">FAQ</span>
      <h2 class="section-title">Frequently Asked <span class="gradient-text">Questions</span></h2>
    </div>

    <div class="faq-list">
      {#each faqs as faq, i}
        <div class="faq-item animate-in" use:reveal class:open={activeFaq === i} style="--delay: {i * 0.08}s">
          <button class="faq-question" on:click={() => toggleFaq(i)}>
            <span>{faq.q}</span>
            <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" class="faq-chevron">
              <polyline points="6 9 12 15 18 9"/>
            </svg>
          </button>
          {#if activeFaq === i}
            <div class="faq-answer">
              <p>{faq.a}</p>
            </div>
          {/if}
        </div>
      {/each}
    </div>
  </div>
</section>

<!-- CTA -->
<section class="cta-section">
  <div class="container">
    <div class="cta-card animate-in" use:reveal>
      <div class="cta-glow"></div>
      <h2>Ready to Unleash the Storm?</h2>
      <p>Start generating leads on autopilot today. No credit card required to get started. 20 Free credits on us to get you started.</p>
      <div class="cta-buttons">
        <a href="#" class="btn-primary btn-lg">
          Get Started Free
          <svg viewBox="0 0 20 20" fill="currentColor" class="btn-arrow"><path fill-rule="evenodd" d="M3 10a.75.75 0 01.75-.75h10.638L10.23 5.29a.75.75 0 111.04-1.08l5.5 5.25a.75.75 0 010 1.08l-5.5 5.25a.75.75 0 11-1.04-1.08l4.158-3.96H3.75A.75.75 0 013 10z"/></svg>
        </a>
        <a href="#pricing" class="btn-secondary">View Pricing</a>
      </div>
    </div>
  </div>
</section>

<!-- FOOTER -->
<footer class="footer">
  <div class="container">
    <div class="footer-top">
      <div class="footer-brand">
        <a href="/" class="logo">
          <span class="logo-icon">
            <svg viewBox="0 0 28 28" fill="none">
              <path d="M14 2L4 8v12l10 6 10-6V8L14 2z" fill="url(#logoGrad2)" opacity="0.15"/>
              <path d="M14 2L4 8v12l10 6 10-6V8L14 2z" stroke="url(#logoGrad2)" stroke-width="1.5" fill="none"/>
              <path d="M17.5 9L12 15l-1.5-1.5M14 15l4-4" stroke="url(#logoGrad2)" stroke-width="1.8" stroke-linecap="round" stroke-linejoin="round"/>
              <defs>
                <linearGradient id="logoGrad2" x1="4" y1="2" x2="24" y2="26">
                  <stop stop-color="#007AFF"/>
                  <stop offset="1" stop-color="#00C6FF"/>
                </linearGradient>
              </defs>
            </svg>
          </span>
          <span class="logo-text">Contact<span class="logo-accent">Storm</span></span>
        </a>
        <p class="footer-tagline">AI-powered lead generation on autopilot.</p>
      </div>
      <div class="footer-links-wrapper">
        <div class="footer-links">
          <div class="footer-col">
            <h4>Product</h4>
            <a href="#features">Features</a>
            <a href="#pricing">Pricing</a>
            <a href="#how-it-works">How It Works</a>
            <a href="#faq">FAQ</a>
          </div>
          <div class="footer-col">
            <h4>Legal</h4>
            <a href="#">Privacy Policy</a>
            <a href="#">Terms of Service</a>
            <a href="#">Cookie Policy</a>
          </div>
        </div>
      </div>
    </div>
    <div class="footer-bottom">
      <p>&copy; {new Date().getFullYear()} Contact Storm. All rights reserved.</p>
    </div>
  </div>
</footer>

<style>
  /* ========== NAVBAR ========== */
  .nav {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 100;
    padding: 1rem 2rem;
    transition: all 0.3s ease;
  }
  .nav.scrolled {
    background: var(--nav-scrolled-bg);
    backdrop-filter: blur(20px);
    border-bottom: 1px solid var(--dark-border);
    padding: 0.75rem 2rem;
  }
  .nav-inner {
    max-width: 1200px;
    margin: 0 auto;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .logo {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    z-index: 101;
  }
  .logo-icon {
    width: 32px;
    height: 32px;
    display: flex;
  }
  .logo-icon svg { width: 100%; height: 100%; }
  .logo-text {
    font-family: var(--font-display);
    font-size: 1.35rem;
    font-weight: 700;
    letter-spacing: -0.02em;
  }
  .logo-accent { color: var(--brand); }
  .nav-links {
    display: flex;
    gap: 2rem;
  }
  .nav-links a {
    font-size: 0.9rem;
    font-weight: 500;
    color: var(--text-secondary);
    transition: color 0.2s;
  }
  .nav-links a:hover { color: var(--text-primary); }
  .nav-actions {
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }
  .btn-ghost {
    font-size: 0.9rem;
    font-weight: 500;
    color: var(--text-secondary);
    padding: 0.5rem 1rem;
    transition: color 0.2s;
  }
  .btn-ghost:hover { color: var(--text-primary); }
  .btn-primary-sm {
    font-size: 0.85rem;
    font-weight: 600;
    color: #fff;
    background: var(--brand);
    padding: 0.55rem 1.25rem;
    border-radius: 8px;
    transition: all 0.2s;
  }
  .btn-primary-sm:hover {
    background: var(--brand-dark);
    transform: translateY(-1px);
  }
  .theme-toggle {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 36px;
    height: 36px;
    border-radius: 8px;
    background: var(--overlay-medium);
    border: 1px solid var(--dark-border);
    color: var(--text-secondary);
    transition: all 0.25s;
    cursor: pointer;
  }
  .theme-toggle:hover {
    background: var(--overlay-strong);
    color: var(--text-primary);
  }
  .theme-toggle svg { width: 18px; height: 18px; }
  .mobile-theme { display: none; }
  .mobile-toggle {
    display: none;
    background: none;
    z-index: 101;
  }
  .hamburger {
    display: flex;
    flex-direction: column;
    gap: 5px;
    width: 24px;
  }
  .hamburger span {
    display: block;
    height: 2px;
    background: var(--text-primary);
    border-radius: 2px;
    transition: all 0.3s;
  }
  .hamburger.open span:nth-child(1) { transform: rotate(45deg) translate(5px, 5px); }
  .hamburger.open span:nth-child(2) { opacity: 0; }
  .hamburger.open span:nth-child(3) { transform: rotate(-45deg) translate(5px, -5px); }

  /* ========== HERO ========== */
  .hero {
    position: relative;
    min-height: 100vh;
    padding: 8rem 2rem 4rem;
    display: flex;
    flex-direction: column;
    align-items: center;
    overflow: hidden;
  }
  .hero-bg {
    position: absolute;
    inset: 0;
    pointer-events: none;
  }
  .grid-pattern {
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(var(--hero-grid-color) 1px, transparent 1px),
      linear-gradient(90deg, var(--hero-grid-color) 1px, transparent 1px);
    background-size: 60px 60px;
    mask-image: radial-gradient(ellipse 80% 60% at 50% 30%, black 20%, transparent 70%);
  }
  .hero-glow {
    position: absolute;
    top: -20%;
    left: 50%;
    transform: translateX(-50%);
    width: 800px;
    height: 600px;
    background: radial-gradient(ellipse, var(--hero-glow-color) 0%, transparent 70%);
  }
  .hero-glow-2 {
    position: absolute;
    top: 30%;
    right: -10%;
    width: 400px;
    height: 400px;
    background: radial-gradient(ellipse, var(--hero-glow-2-color) 0%, transparent 70%);
  }
  .hero-content {
    position: relative;
    text-align: center;
    max-width: 720px;
    margin: 0 auto;
  }
  .hero-badge {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    background: rgba(0, 122, 255, 0.1);
    border: 1px solid rgba(0, 122, 255, 0.2);
    padding: 0.4rem 1rem;
    border-radius: 100px;
    font-size: 0.8rem;
    font-weight: 600;
    color: var(--brand-light);
    margin-bottom: 1.5rem;
    letter-spacing: 0.02em;
  }
  .badge-dot {
    width: 6px;
    height: 6px;
    background: var(--brand);
    border-radius: 50%;
    animation: pulse-dot 2s infinite;
  }
  @keyframes pulse-dot {
    0%, 100% { opacity: 1; box-shadow: 0 0 0 0 rgba(0, 122, 255, 0.4); }
    50% { opacity: 0.8; box-shadow: 0 0 0 6px rgba(0, 122, 255, 0); }
  }
  .hero-title {
    font-family: var(--font-display);
    font-size: clamp(2.5rem, 6vw, 4.2rem);
    font-weight: 800;
    line-height: 1.1;
    letter-spacing: -0.03em;
    margin-bottom: 1.5rem;
  }
  .gradient-text {
    background: var(--gradient-brand);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .hero-subtitle {
    font-size: 1.15rem;
    color: var(--text-secondary);
    max-width: 560px;
    margin: 0 auto 2.5rem;
    line-height: 1.7;
  }
  .hero-ctas {
    display: flex;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
    margin-bottom: 3rem;
  }
  .btn-primary {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    background: var(--brand);
    color: #fff;
    font-weight: 600;
    font-size: 1rem;
    padding: 0.85rem 1.75rem;
    border-radius: 10px;
    transition: all 0.25s;
    box-shadow: 0 0 20px rgba(0, 122, 255, 0.3);
  }
  .btn-primary:hover {
    background: var(--brand-dark);
    transform: translateY(-2px);
    box-shadow: 0 4px 30px rgba(0, 122, 255, 0.4);
  }
  .btn-primary .btn-arrow {
    width: 18px;
    height: 18px;
    transition: transform 0.2s;
  }
  .btn-primary:hover .btn-arrow { transform: translateX(3px); }
  .btn-secondary {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    background: var(--overlay-medium);
    border: 1px solid var(--dark-border);
    color: var(--text-primary);
    font-weight: 600;
    font-size: 1rem;
    padding: 0.85rem 1.75rem;
    border-radius: 10px;
    transition: all 0.25s;
  }
  .btn-secondary:hover {
    background: var(--overlay-strong);
    border-color: var(--overlay-hover);
    transform: translateY(-2px);
  }
  .btn-lg { padding: 1rem 2rem; font-size: 1.05rem; }
  .hero-stats {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 2rem;
  }
  .stat { text-align: center; }
  .stat-num {
    display: block;
    font-family: var(--font-display);
    font-size: 1.6rem;
    font-weight: 700;
    color: var(--text-primary);
  }
  .stat-label {
    font-size: 0.8rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }
  .stat-divider {
    width: 1px;
    height: 40px;
    background: var(--dark-border);
  }

  /* ========== HERO VISUAL ========== */
  .hero-visual {
    position: relative;
    width: 100%;
    max-width: 700px;
    margin: 4rem auto 0;
    contain: layout style;
  }
  .demo-browser {
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 12px;
    overflow: hidden;
    box-shadow: var(--shadow-heavy);
  }
  .browser-header {
    display: flex;
    align-items: center;
    gap: 1rem;
    padding: 0.75rem 1rem;
    background: var(--overlay-subtle);
    border-bottom: 1px solid var(--dark-border);
  }
  .browser-dots {
    display: flex;
    gap: 6px;
  }
  .dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
  }
  .dot.red { background: #FF5F57; }
  .dot.yellow { background: #FEBC2E; }
  .dot.green { background: #28C840; }
  .browser-url {
    flex: 1;
    display: flex;
    align-items: center;
    gap: 0.4rem;
    background: var(--overlay-light);
    padding: 0.35rem 0.75rem;
    border-radius: 6px;
    font-size: 0.78rem;
    color: var(--text-muted);
    font-family: 'SF Mono', 'Fira Code', monospace;
  }
  .lock-icon { width: 12px; height: 12px; color: #28C840; }
  .live-badge {
    display: flex;
    align-items: center;
    gap: 0.4rem;
    font-size: 0.7rem;
    font-weight: 700;
    color: #28C840;
    letter-spacing: 0.05em;
  }
  .live-dot {
    width: 6px;
    height: 6px;
    background: #28C840;
    border-radius: 50%;
    animation: pulse-dot 1.5s infinite;
  }
  .browser-body { padding: 1.5rem; }
  .demo-form {
    max-width: 400px;
    margin: 0 auto;
  }
  .demo-form-title {
    font-family: var(--font-display);
    font-size: 1.15rem;
    font-weight: 600;
    margin-bottom: 1.25rem;
    color: var(--text-primary);
  }
  .demo-field {
    margin-bottom: 0.85rem;
    transition: opacity 0.3s;
  }
  .demo-field label {
    display: block;
    font-size: 0.75rem;
    color: var(--text-muted);
    margin-bottom: 0.25rem;
    font-weight: 500;
    text-transform: uppercase;
    letter-spacing: 0.04em;
  }
  .demo-input {
    background: var(--overlay-light);
    border: 1px solid var(--dark-border);
    border-radius: 6px;
    padding: 0.6rem 0.75rem;
    height: 38px;
    font-size: 0.85rem;
    transition: border-color 0.3s, box-shadow 0.3s;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  .demo-field.active .demo-input {
    border-color: var(--brand);
    box-shadow: 0 0 0 3px var(--brand-glow);
  }
  .demo-field.filled .demo-input {
    border-color: rgba(40, 200, 64, 0.3);
  }
  .typed-text { color: var(--text-primary); display: inline-block; max-width: 100%; overflow: hidden; text-overflow: ellipsis; }
  .typing-text { color: var(--brand-light); display: inline-block; max-width: 100%; overflow: hidden; text-overflow: ellipsis; }
  .cursor-blink { animation: blink 0.8s step-end infinite; color: var(--brand); }
  @keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0; } }
  .placeholder-text {
    display: block;
    height: 1em;
    width: 60%;
    background: var(--overlay-light);
    border-radius: 3px;
  }
  .demo-submit {
    margin-top: 1rem;
    background: var(--overlay-medium);
    color: var(--text-muted);
    text-align: center;
    padding: 0.65rem;
    border-radius: 8px;
    font-weight: 600;
    font-size: 0.85rem;
    height: 38px;
    transition: background 0.4s, color 0.4s, box-shadow 0.4s;
  }
  .demo-submit.ready {
    background: var(--brand);
    color: #fff;
    box-shadow: 0 0 20px rgba(0, 122, 255, 0.3);
  }
  .demo-submit.sent {
    background: #28C840;
    color: #fff;
  }
  .bot-indicator {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    margin-top: 1.25rem;
    padding-top: 1rem;
    border-top: 1px solid var(--dark-border);
  }
  .bot-avatar {
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(0, 122, 255, 0.1);
    border-radius: 8px;
  }
  .bot-avatar svg { width: 20px; height: 20px; }
  .bot-info { display: flex; flex-direction: column; }
  .bot-name { font-size: 0.8rem; font-weight: 600; }
  .bot-status { font-size: 0.72rem; color: var(--brand-light); }

  /* Floating cards */
  .floating-card {
    position: absolute;
    display: flex;
    align-items: center;
    gap: 0.7rem;
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    padding: 0.75rem 1rem;
    border-radius: 10px;
    box-shadow: var(--shadow-float);
    animation: float 6s ease-in-out infinite;
  }
  .card-leads {
    top: 15%;
    right: -40px;
    animation-delay: 0s;
  }
  .card-rate {
    bottom: 20%;
    left: -40px;
    animation-delay: 3s;
  }
  @keyframes float {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-10px); }
  }
  .fc-icon {
    width: 36px;
    height: 36px;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 8px;
    background: var(--overlay-light);
  }
  .fc-icon svg { width: 20px; height: 20px; }
  .fc-content { display: flex; flex-direction: column; }
  .fc-label { font-size: 0.7rem; color: var(--text-muted); }
  .fc-value { font-family: var(--font-display); font-size: 1.1rem; font-weight: 700; }

  /* ========== TRUST BAR ========== */
  .trust-bar {
    padding: 3rem 2rem;
    border-top: 1px solid var(--dark-border);
    border-bottom: 1px solid var(--dark-border);
  }
  .trust-label {
    text-align: center;
    font-size: 0.8rem;
    color: var(--text-muted);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 1.5rem;
  }
  .trust-logos {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 3rem;
    flex-wrap: wrap;
  }
  .trust-logo {
    font-family: var(--font-display);
    font-size: 1.15rem;
    font-weight: 700;
    color: var(--text-muted);
    opacity: 0.4;
    transition: opacity 0.2s;
  }
  .trust-logo:hover { opacity: 0.7; }

  /* ========== SECTIONS ========== */
  .section { padding: 6rem 2rem; }
  .container { max-width: 1200px; margin: 0 auto; }
  .container-sm { max-width: 750px; }
  .section-header { text-align: center; margin-bottom: 4rem; }
  .section-tag {
    display: inline-block;
    font-size: 0.78rem;
    font-weight: 700;
    color: var(--brand);
    text-transform: uppercase;
    letter-spacing: 0.1em;
    margin-bottom: 0.75rem;
  }
  .section-title {
    font-family: var(--font-display);
    font-size: clamp(1.8rem, 4vw, 2.8rem);
    font-weight: 800;
    letter-spacing: -0.02em;
    line-height: 1.15;
    margin-bottom: 1rem;
  }
  .section-title-lg {
    font-size: clamp(2.2rem, 5vw, 3.4rem);
  }
  .section-desc {
    font-size: 1.05rem;
    color: var(--text-secondary);
    max-width: 550px;
    margin: 0 auto;
    line-height: 1.7;
  }

  /* ========== STEPS ========== */
  .steps-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 2rem;
  }
  .step-card {
    position: relative;
    padding: 2rem;
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 16px;
    transition: all 0.3s;
    animation-delay: var(--delay);
    box-shadow: var(--shadow-card);
  }
  .step-card:hover {
    border-color: rgba(0, 122, 255, 0.2);
    transform: translateY(-4px);
    box-shadow: var(--shadow-medium);
  }
  .step-num {
    font-family: var(--font-display);
    font-size: 2.5rem;
    font-weight: 800;
    background: var(--gradient-brand);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    margin-bottom: 1rem;
  }
  .step-card h3 {
    font-family: var(--font-display);
    font-size: 1.2rem;
    font-weight: 700;
    margin-bottom: 0.75rem;
  }
  .step-card p {
    font-size: 0.92rem;
    color: var(--text-secondary);
    line-height: 1.6;
  }
  .step-line { display: none; }

  /* ========== WHY IT WORKS ========== */
  .why-section {
    background: var(--surface);
    border-top: 1px solid var(--dark-border);
    border-bottom: 1px solid var(--dark-border);
  }
  .why-layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: center;
  }
  .why-content .section-tag { margin-bottom: 0.75rem; }
  .why-content .section-title { text-align: left; margin-bottom: 1.5rem; }
  .why-text {
    font-size: 1.05rem;
    color: var(--text-secondary);
    line-height: 1.8;
    margin-bottom: 1rem;
  }
  .why-text strong {
    color: var(--text-primary);
    font-weight: 600;
  }
  .why-visual {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 1.5rem;
  }
  .delivery-card {
    text-align: center;
    padding: 2.5rem 2rem;
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 20px;
    width: 100%;
    max-width: 320px;
    position: relative;
    overflow: hidden;
    box-shadow: var(--shadow-card);
  }
  .delivery-card::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 3px;
    background: var(--gradient-brand);
  }
  .delivery-icon {
    width: 48px;
    height: 48px;
    margin: 0 auto 1.25rem;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(16, 185, 129, 0.1);
    border-radius: 12px;
    color: #10B981;
  }
  .delivery-icon svg { width: 26px; height: 26px; }
  .delivery-stat {
    font-family: var(--font-display);
    font-size: 3.5rem;
    font-weight: 800;
    background: linear-gradient(135deg, #10B981 0%, #34D399 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
    line-height: 1;
    margin-bottom: 0.5rem;
  }
  .delivery-label {
    font-family: var(--font-display);
    font-size: 1.1rem;
    font-weight: 700;
    margin-bottom: 0.35rem;
  }
  .delivery-sub {
    font-size: 0.8rem;
    color: var(--text-muted);
  }
  .delivery-pills {
    display: flex;
    gap: 0.6rem;
    flex-wrap: wrap;
    justify-content: center;
  }
  .pill {
    display: flex;
    align-items: center;
    gap: 0.4rem;
    padding: 0.45rem 0.85rem;
    border-radius: 100px;
    font-size: 0.8rem;
    font-weight: 600;
  }
  .pill svg { width: 14px; height: 14px; }
  .pill-green {
    background: rgba(16, 185, 129, 0.1);
    color: #34D399;
    border: 1px solid rgba(16, 185, 129, 0.2);
  }

  /* ========== FEATURES ========== */
  .features-section { background: var(--surface); }
  .features-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1.5rem;
  }
  .feature-card {
    padding: 2rem;
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 14px;
    transition: all 0.3s;
    animation-delay: var(--delay);
    box-shadow: var(--shadow-card);
  }
  .feature-card:hover {
    border-color: rgba(0, 122, 255, 0.15);
    transform: translateY(-4px);
    box-shadow: var(--shadow-medium);
  }
  .feature-icon {
    width: 44px;
    height: 44px;
    display: flex;
    align-items: center;
    justify-content: center;
    background: rgba(0, 122, 255, 0.1);
    border-radius: 10px;
    margin-bottom: 1.25rem;
    color: var(--brand);
  }
  .feature-icon svg { width: 22px; height: 22px; }
  .feature-card h3 {
    font-family: var(--font-display);
    font-size: 1.1rem;
    font-weight: 700;
    margin-bottom: 0.6rem;
  }
  .feature-card p {
    font-size: 0.9rem;
    color: var(--text-secondary);
    line-height: 1.65;
  }

  /* ========== SHOWCASE ========== */
  .showcase-layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 4rem;
    align-items: center;
  }
  .showcase-desc {
    font-size: 1rem;
    color: var(--text-secondary);
    line-height: 1.7;
    margin-bottom: 2rem;
  }
  .showcase-text .section-title { text-align: left; margin-bottom: 1.25rem; }
  .showcase-text .section-tag { margin-bottom: 0.75rem; }
  .showcase-list {
    list-style: none;
    margin-bottom: 2rem;
  }
  .showcase-list li {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    padding: 0.5rem 0;
    font-size: 0.95rem;
    color: var(--text-secondary);
  }
  .showcase-list svg { width: 18px; height: 18px; flex-shrink: 0; }
  .multi-window {
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }
  .mini-browser {
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 10px;
    overflow: hidden;
    transition: all 0.3s;
    box-shadow: var(--shadow-card);
  }
  .mini-browser:hover {
    border-color: rgba(0, 122, 255, 0.2);
    box-shadow: var(--shadow-medium);
  }
  .mini-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.5rem 0.75rem;
    border-bottom: 1px solid var(--dark-border);
    background: var(--overlay-subtle);
  }
  .mini-dots {
    display: flex;
    gap: 4px;
  }
  .mini-dots span {
    width: 7px;
    height: 7px;
    border-radius: 50%;
    background: var(--overlay-hover);
  }
  .mini-live {
    font-size: 0.68rem;
    font-weight: 700;
    color: #28C840;
    display: flex;
    align-items: center;
    gap: 0.35rem;
  }
  .mini-body { padding: 0.85rem; display: flex; flex-direction: column; gap: 0.5rem; }
  .mini-form-line {
    height: 8px;
    background: linear-gradient(90deg, var(--brand-glow-strong), var(--brand-glow));
    border-radius: 4px;
    animation: shimmer 2s ease-in-out infinite;
  }
  .mini-form-line.short { width: 40%; }
  .mini-btn-line {
    height: 10px;
    width: 30%;
    background: var(--brand);
    border-radius: 4px;
    opacity: 0.6;
    margin-top: 0.25rem;
  }
  @keyframes shimmer {
    0%, 100% { opacity: 0.4; }
    50% { opacity: 1; }
  }

  /* ========== PRICING ========== */
  .pricing-section { background: var(--surface); }
  .pricing-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1.25rem;
    align-items: start;
  }
  .pricing-card {
    position: relative;
    padding: 2rem 1.5rem;
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 16px;
    transition: all 0.3s;
    animation-delay: var(--delay);
    box-shadow: var(--shadow-card);
  }
  .pricing-card:hover {
    transform: translateY(-4px);
    box-shadow: var(--shadow-medium);
  }
  .pricing-card.highlighted {
    border-color: var(--brand);
    box-shadow: 0 0 40px rgba(0, 122, 255, 0.15);
    transform: scale(1.03);
    z-index: 2;
  }
  .pricing-card.highlighted:hover {
    transform: scale(1.03) translateY(-4px);
  }
  .pricing-badge {
    display: inline-block;
    font-size: 0.7rem;
    font-weight: 700;
    text-transform: uppercase;
    letter-spacing: 0.08em;
    padding: 0.3rem 0.75rem;
    border-radius: 100px;
    background: var(--overlay-medium);
    color: var(--text-muted);
    margin-bottom: 1rem;
  }
  .pricing-badge.pop {
    background: rgba(0, 122, 255, 0.15);
    color: var(--brand-light);
  }
  .pricing-name {
    font-family: var(--font-display);
    font-size: 1.15rem;
    font-weight: 700;
    margin-bottom: 1rem;
  }
  .pricing-price {
    display: flex;
    align-items: baseline;
    gap: 0.15rem;
    margin-bottom: 0.5rem;
  }
  .price-currency {
    font-size: 1.25rem;
    font-weight: 600;
    color: var(--text-secondary);
  }
  .price-amount {
    font-family: var(--font-display);
    font-size: 2.8rem;
    font-weight: 800;
    letter-spacing: -0.02em;
  }
  .price-amount.payg {
    font-size: 1.6rem;
    color: var(--brand);
  }
  .price-period {
    font-size: 0.9rem;
    color: var(--text-muted);
    font-weight: 500;
  }
  .pricing-bots {
    display: flex;
    align-items: center;
    gap: 0.5rem;
    font-size: 0.8rem;
    color: var(--brand-light);
    margin-bottom: 1.25rem;
    padding: 0.4rem 0.75rem;
    background: rgba(0, 122, 255, 0.06);
    border-radius: 6px;
    width: fit-content;
  }
  .pricing-bots .sep { color: var(--text-muted); }
  .pricing-features {
    list-style: none;
    margin-bottom: 1.75rem;
    padding-top: 1.25rem;
    border-top: 1px solid var(--dark-border);
  }
  .pricing-features li {
    display: flex;
    align-items: flex-start;
    gap: 0.6rem;
    padding: 0.35rem 0;
    font-size: 0.85rem;
    color: var(--text-secondary);
  }
  .pricing-features svg {
    width: 16px;
    height: 16px;
    color: var(--brand);
    flex-shrink: 0;
    margin-top: 2px;
  }
  .pricing-cta {
    display: block;
    text-align: center;
    padding: 0.75rem;
    border-radius: 10px;
    font-weight: 600;
    font-size: 0.9rem;
    background: var(--overlay-medium);
    border: 1px solid var(--dark-border);
    color: var(--text-primary);
    transition: all 0.25s;
  }
  .pricing-cta:hover {
    background: var(--overlay-strong);
    transform: translateY(-1px);
  }
  .pricing-cta.primary {
    background: var(--brand);
    border-color: var(--brand);
    color: #fff;
    box-shadow: 0 0 20px rgba(0, 122, 255, 0.25);
  }
  .pricing-cta.primary:hover {
    background: var(--brand-dark);
    box-shadow: 0 4px 30px rgba(0, 122, 255, 0.35);
  }

  /* ========== FAQ ========== */
  .faq-list {
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
  }
  .faq-item {
    background: var(--dark-card);
    border: 1px solid var(--dark-border);
    border-radius: 12px;
    overflow: hidden;
    transition: all 0.3s;
    animation-delay: var(--delay);
    box-shadow: var(--shadow-card);
  }
  .faq-item:hover { border-color: var(--overlay-hover); }
  .faq-item.open { border-color: rgba(0, 122, 255, 0.2); }
  .faq-question {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    padding: 1.15rem 1.25rem;
    background: none;
    color: var(--text-primary);
    font-size: 0.95rem;
    font-weight: 600;
    text-align: left;
  }
  .faq-chevron {
    width: 20px;
    height: 20px;
    color: var(--text-muted);
    transition: transform 0.3s;
    flex-shrink: 0;
  }
  .faq-item.open .faq-chevron { transform: rotate(180deg); color: var(--brand); }
  .faq-answer {
    padding: 0 1.25rem 1.25rem;
  }
  .faq-answer p {
    font-size: 0.9rem;
    color: var(--text-secondary);
    line-height: 1.7;
  }

  /* ========== CTA ========== */
  .cta-section { padding: 4rem 2rem 6rem; }
  .cta-card {
    position: relative;
    text-align: center;
    padding: 4rem 2rem;
    background: linear-gradient(135deg, rgba(0, 122, 255, 0.12) 0%, rgba(0, 198, 255, 0.06) 100%);
    border: 1px solid rgba(0, 122, 255, 0.2);
    border-radius: 24px;
    overflow: hidden;
  }
  .cta-glow {
    position: absolute;
    top: -50%;
    left: 50%;
    transform: translateX(-50%);
    width: 600px;
    height: 400px;
    background: radial-gradient(ellipse, rgba(0, 122, 255, 0.15) 0%, transparent 70%);
    pointer-events: none;
  }
  .cta-card h2 {
    position: relative;
    font-family: var(--font-display);
    font-size: clamp(1.8rem, 4vw, 2.5rem);
    font-weight: 800;
    letter-spacing: -0.02em;
    margin-bottom: 1rem;
  }
  .cta-card p {
    position: relative;
    font-size: 1.05rem;
    color: var(--text-secondary);
    margin-bottom: 2rem;
    max-width: 450px;
    margin-left: auto;
    margin-right: auto;
  }
  .cta-buttons {
    position: relative;
    display: flex;
    gap: 1rem;
    justify-content: center;
    flex-wrap: wrap;
  }

  /* ========== FOOTER ========== */
  .footer {
    padding: 4rem 2rem 2rem;
    border-top: 1px solid var(--dark-border);
  }
  .footer-top {
    display: flex;
    align-items: flex-start;
    gap: 3rem;
    margin-bottom: 3rem;
  }
  .footer-brand { max-width: 280px; }
  .footer-links-wrapper {
    flex: 1;
    display: flex;
    justify-content: center;
  }
  .footer-tagline {
    color: var(--text-muted);
    font-size: 0.88rem;
    margin-top: 0.75rem;
    line-height: 1.6;
  }
  .footer-links {
    display: flex;
    justify-content: center;
    gap: 4rem;
  }
  .footer-col {
    display: flex;
    flex-direction: column;
    gap: 0.6rem;
  }
  .footer-col h4 {
    font-family: var(--font-display);
    font-size: 0.85rem;
    font-weight: 700;
    margin-bottom: 0.5rem;
  }
  .footer-col a {
    font-size: 0.85rem;
    color: var(--text-muted);
    transition: color 0.2s;
  }
  .footer-col a:hover { color: var(--text-primary); }
  .footer-bottom {
    padding-top: 2rem;
    border-top: 1px solid var(--dark-border);
    text-align: center;
  }
  .footer-bottom p {
    font-size: 0.8rem;
    color: var(--text-muted);
  }

  /* animations are in app.css */

  /* ========== RESPONSIVE ========== */
  @media (max-width: 1024px) {
    .pricing-grid {
      grid-template-columns: repeat(2, 1fr);
    }
    .features-grid {
      grid-template-columns: repeat(2, 1fr);
    }
    .why-layout {
      grid-template-columns: 1fr;
      gap: 2.5rem;
    }
    .why-content .section-title { text-align: center; }
    .why-content { text-align: center; }
    .showcase-layout {
      grid-template-columns: 1fr;
      gap: 3rem;
    }
    .showcase-text .section-title { text-align: center; }
    .showcase-text { text-align: center; }
    .showcase-list { display: inline-block; text-align: left; }
    .showcase-text .btn-primary { margin: 0 auto; }
  }

  @media (max-width: 768px) {
    .nav-links {
      display: none;
      position: fixed;
      inset: 0;
      background: var(--dark);
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 2rem;
      z-index: 100;
    }
    .nav-links.open { display: flex; }
    .nav-links a { font-size: 1.25rem; }
    .nav-actions { display: none; }
    .mobile-theme { display: flex; z-index: 101; }
    .mobile-toggle { display: block; }
    .hero { padding: 7rem 1.25rem 3rem; }
    .hero-stats { gap: 1.25rem; }
    .stat-num { font-size: 1.3rem; }
    .steps-grid { grid-template-columns: 1fr; }
    .features-grid { grid-template-columns: 1fr; }
    .pricing-grid { grid-template-columns: 1fr; max-width: 400px; margin: 0 auto; }
    .pricing-card.highlighted { transform: none; }
    .pricing-card.highlighted:hover { transform: translateY(-4px); }
    .footer-top { flex-direction: column; align-items: center; text-align: center; }
    .footer-links { gap: 2rem; }
    .floating-card { display: none; }
    .section { padding: 4rem 1.25rem; }
    .hero-visual { margin-top: 3rem; }
  }

  @media (max-width: 480px) {
    .hero-title { font-size: 2rem; }
    .hero-ctas { flex-direction: column; align-items: center; }
    .hero-stats { flex-direction: column; gap: 1rem; }
    .stat-divider { width: 40px; height: 1px; }
    .footer-links { flex-wrap: wrap; }
  }
</style>
