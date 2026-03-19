# E-book Checkout — Clickable Prototype

A 3-step checkout flow for a $5.99 AI e-book. Built as a single-file HTML prototype with no build step — open directly in any browser.

## Flow

1. **Product** — Book cover, description, feature list, "Buy now" CTA
2. **Payment** — Email, card fields (auto-formatting expiry), order summary sidebar
3. **Confirmation** — Success state, receipt, animated download button

## Prototype source

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>E-book Checkout — Prototype</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=DM+Sans:ital,opsz,wght@0,9..40,400;0,9..40,500;0,9..40,600;0,9..40,700;1,9..40,400&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #0f1419;
      --surface: #1a222d;
      --surface2: #243040;
      --border: rgba(255, 255, 255, 0.08);
      --text: #e8eef4;
      --muted: #8b9cb0;
      --accent: #3d9cf5;
      --accent-dim: #2b7fd4;
      --success: #34d399;
      --radius: 10px;
      --font: "DM Sans", system-ui, -apple-system, sans-serif;
      --shadow: 0 12px 40px rgba(0, 0, 0, 0.35);
    }

    *, *::before, *::after { box-sizing: border-box; }
    html { scroll-behavior: smooth; }

    body {
      margin: 0;
      min-height: 100vh;
      font-family: var(--font);
      background: var(--bg);
      color: var(--text);
      line-height: 1.55;
      -webkit-font-smoothing: antialiased;
    }

    a { color: var(--accent); text-decoration: none; }
    a:hover { text-decoration: underline; }

    .site-header {
      position: sticky; top: 0; z-index: 40;
      display: flex; align-items: center; justify-content: space-between; gap: 1rem;
      padding: 0.75rem 1.25rem;
      background: color-mix(in oklab, var(--bg) 88%, transparent);
      backdrop-filter: blur(12px);
      border-bottom: 1px solid var(--border);
    }
    .site-header a.back { color: var(--muted); font-size: 0.9rem; }
    .site-header a.back:hover { color: var(--text); }
    .site-title { font-size: 0.95rem; font-weight: 600; margin: 0; }

    .wrap { max-width: 900px; margin: 0 auto; padding: 1.5rem 1.25rem 3rem; }

    .card {
      background: var(--surface);
      border: 1px solid var(--border);
      border-radius: var(--radius);
      padding: 1.25rem 1.35rem;
      box-shadow: var(--shadow);
    }

    .btn {
      display: inline-flex; align-items: center; justify-content: center; gap: 0.35rem;
      padding: 0.6rem 1.1rem; font-family: inherit; font-size: 0.9rem; font-weight: 600;
      border-radius: 8px; border: none; cursor: pointer;
      background: var(--accent); color: #fff;
      transition: background 0.15s, transform 0.1s;
    }
    .btn:hover { background: var(--accent-dim); }
    .btn:active { transform: scale(0.98); }
    .btn--ghost { background: transparent; color: var(--text); border: 1px solid var(--border); }
    .btn--ghost:hover { background: var(--surface2); }

    .form-row { margin-bottom: 1rem; }
    .form-row label { display: block; font-size: 0.85rem; font-weight: 500; margin-bottom: 0.35rem; color: var(--muted); }
    input[type="text"], input[type="email"] {
      width: 100%; max-width: 400px; padding: 0.6rem 0.75rem;
      font: inherit; color: var(--text); background: var(--surface2);
      border: 1px solid var(--border); border-radius: 8px;
    }

    .steps { display: flex; gap: 0.5rem; flex-wrap: wrap; margin-bottom: 1.25rem; }
    .step-pill {
      padding: 0.35rem 0.75rem; border-radius: 999px; font-size: 0.8rem;
      background: var(--surface2); color: var(--muted); border: 1px solid var(--border);
    }
    .step-pill.is-current {
      background: color-mix(in oklab, var(--accent) 25%, transparent);
      color: var(--text); border-color: var(--accent);
    }
    .step-pill.is-done { color: var(--success); border-color: var(--success); }

    .hidden { display: none !important; }

    .ebook-layout { display: grid; gap: 1.5rem; align-items: start; }
    @media (min-width: 640px) { .ebook-layout { grid-template-columns: 200px 1fr; } }

    .ebook-cover {
      width: 100%; max-width: 200px; aspect-ratio: 3 / 4; border-radius: 8px;
      background: linear-gradient(145deg, #3d9cf5 0%, #6c5ce7 50%, #a55eea 100%);
      display: flex; flex-direction: column; align-items: center; justify-content: center;
      padding: 1.25rem; text-align: center;
      box-shadow: 4px 6px 20px rgba(0, 0, 0, 0.4);
      position: relative; overflow: hidden;
    }
    .ebook-cover::before {
      content: ""; position: absolute; inset: 6px;
      border: 1px solid rgba(255, 255, 255, 0.2); border-radius: 5px; pointer-events: none;
    }
    .ebook-cover__title { font-size: 1.15rem; font-weight: 700; color: #fff; line-height: 1.25; margin: 0 0 0.35rem; letter-spacing: -0.02em; }
    .ebook-cover__subtitle { font-size: 0.7rem; font-weight: 500; color: rgba(255, 255, 255, 0.75); text-transform: uppercase; letter-spacing: 0.08em; }

    .ebook-details h2 { margin: 0 0 0.25rem; font-size: 1.35rem; }
    .ebook-price { font-size: 1.5rem; font-weight: 700; color: var(--accent); margin: 0.75rem 0; }
    .ebook-features { list-style: none; padding: 0; margin: 0 0 1.25rem; }
    .ebook-features li { padding: 0.3rem 0; font-size: 0.9rem; color: var(--muted); }
    .ebook-features li::before { content: "\2713\00a0"; color: var(--success); font-weight: 700; }

    .pay-grid { display: grid; gap: 1rem; }
    @media (min-width: 640px) { .pay-grid { grid-template-columns: 1fr 260px; } }
    .pay-fields { min-width: 0; }
    .card-row { display: grid; grid-template-columns: 1fr 1fr; gap: 0.75rem; }

    .order-summary {
      background: var(--surface2); border: 1px solid var(--border);
      border-radius: var(--radius); padding: 1.15rem;
    }
    .order-summary h3 { margin: 0 0 0.85rem; font-size: 0.85rem; font-weight: 600; text-transform: uppercase; letter-spacing: 0.05em; color: var(--muted); }
    .summary-item { display: flex; justify-content: space-between; align-items: center; gap: 0.5rem; font-size: 0.9rem; padding-bottom: 0.75rem; margin-bottom: 0.75rem; border-bottom: 1px solid var(--border); }
    .summary-item:last-child { border-bottom: none; margin-bottom: 0; padding-bottom: 0; }
    .summary-total { font-weight: 700; font-size: 1rem; }

    .confirm-hero { text-align: center; padding: 1rem 0 0.5rem; }
    .confirm-hero .checkmark { width: 56px; height: 56px; border-radius: 50%; background: color-mix(in oklab, var(--success) 20%, transparent); display: inline-flex; align-items: center; justify-content: center; font-size: 1.75rem; margin-bottom: 0.75rem; }
    .confirm-hero h2 { margin: 0 0 0.25rem; font-size: 1.3rem; }
    .confirm-hero p { color: var(--muted); margin: 0; font-size: 0.92rem; }

    .receipt { margin: 1.25rem 0; font-size: 0.9rem; }
    .receipt-row { display: flex; justify-content: space-between; padding: 0.45rem 0; border-bottom: 1px solid var(--border); }
    .receipt-row:last-child { border-bottom: none; }
    .receipt-label { color: var(--muted); }

    .btn--pay { width: 100%; padding: 0.75rem 1.25rem; font-size: 1rem; margin-top: 0.5rem; }
    .btn--download { width: 100%; padding: 0.75rem 1.25rem; font-size: 1rem; background: var(--success); margin-top: 0.75rem; }
    .btn--download:hover { background: color-mix(in oklab, var(--success) 85%, #000); }
    .secure-note { display: flex; align-items: center; gap: 0.4rem; font-size: 0.78rem; color: var(--muted); margin-top: 0.75rem; justify-content: center; }
  </style>
</head>
<body>
  <header class="site-header">
    <a class="back" href="#">&larr; Back</a>
    <h1 class="site-title">E-book checkout</h1>
    <span></span>
  </header>
  <main class="wrap">
    <div class="steps" id="pills">
      <span class="step-pill is-current" data-p="1">Product</span>
      <span class="step-pill" data-p="2">Payment</span>
      <span class="step-pill" data-p="3">Confirmation</span>
    </div>

    <!-- Step 1: Product -->
    <div id="step-1" class="card">
      <div class="ebook-layout">
        <div class="ebook-cover">
          <span class="ebook-cover__title">Mastering AI</span>
          <span class="ebook-cover__subtitle">A practical guide</span>
        </div>
        <div class="ebook-details">
          <h2>Mastering AI</h2>
          <p style="color:var(--muted);margin:0 0 0.5rem;font-size:0.92rem;">The complete guide to understanding and applying artificial intelligence in your work and life.</p>
          <div class="ebook-price">$5.99</div>
          <ul class="ebook-features">
            <li>12 chapters, 180+ pages</li>
            <li>Real-world case studies &amp; prompts</li>
            <li>PDF + EPUB formats included</li>
            <li>Free lifetime updates</li>
          </ul>
          <button type="button" class="btn" id="to-pay">Buy now &mdash; $5.99</button>
        </div>
      </div>
    </div>

    <!-- Step 2: Payment -->
    <div id="step-2" class="card hidden">
      <div class="pay-grid">
        <div class="pay-fields">
          <h2 style="margin-top:0">Payment details</h2>
          <div class="form-row">
            <label for="email">Email (for delivery)</label>
            <input id="email" type="email" placeholder="you@example.com" autocomplete="email" style="max-width:none">
          </div>
          <div class="form-row">
            <label for="card">Card number</label>
            <input id="card" type="text" placeholder="4242 4242 4242 4242" autocomplete="cc-number" style="max-width:none">
          </div>
          <div class="card-row">
            <div class="form-row">
              <label for="exp">Expiry</label>
              <input id="exp" type="text" placeholder="MM / YY" autocomplete="cc-exp" inputmode="numeric" maxlength="7" style="max-width:none">
            </div>
            <div class="form-row">
              <label for="cvc">CVC</label>
              <input id="cvc" type="text" placeholder="123" autocomplete="cc-csc" style="max-width:none">
            </div>
          </div>
          <button type="button" class="btn btn--ghost" id="back-1">Back</button>
          <button type="button" class="btn btn--pay" id="to-confirm">Pay $5.99</button>
          <div class="secure-note">
            <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="3" y="11" width="18" height="11" rx="2"/><path d="M7 11V7a5 5 0 0 1 10 0v4"/></svg>
            Secure checkout &mdash; prototype only, no real charges
          </div>
        </div>
        <div class="order-summary">
          <h3>Order summary</h3>
          <div class="summary-item">
            <span>Mastering AI (e-book)</span>
            <span>$5.99</span>
          </div>
          <div class="summary-item summary-total">
            <span>Total</span>
            <span>$5.99</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Step 3: Confirmation -->
    <div id="step-3" class="hidden">
      <div class="card">
        <div class="confirm-hero">
          <div class="checkmark">&#10003;</div>
          <h2>Payment successful</h2>
          <p id="order-id"></p>
        </div>
        <div class="receipt">
          <div class="receipt-row">
            <span class="receipt-label">Email</span>
            <span id="receipt-email">you@example.com</span>
          </div>
          <div class="receipt-row">
            <span class="receipt-label">Item</span>
            <span>Mastering AI (e-book)</span>
          </div>
          <div class="receipt-row">
            <span class="receipt-label">Amount</span>
            <span>$5.99</span>
          </div>
        </div>
        <button type="button" class="btn btn--download" id="download-btn">Download your e-book</button>
        <div class="secure-note" style="margin-top:1rem;">A download link was also sent to your email.</div>
      </div>
    </div>
  </main>

  <script>
    function setStep(n) {
      document.getElementById("step-1").classList.toggle("hidden", n !== 1);
      document.getElementById("step-2").classList.toggle("hidden", n !== 2);
      document.getElementById("step-3").classList.toggle("hidden", n !== 3);
      document.querySelectorAll(".step-pill").forEach(function (p) {
        var pn = parseInt(p.getAttribute("data-p"), 10);
        p.classList.remove("is-current", "is-done");
        if (pn < n) p.classList.add("is-done");
        if (pn === n) p.classList.add("is-current");
      });
      window.scrollTo({ top: 0, behavior: "smooth" });
    }

    document.getElementById("exp").addEventListener("input", function (e) {
      var digits = this.value.replace(/\D/g, "").slice(0, 4);
      if (digits.length >= 3) {
        this.value = digits.slice(0, 2) + " / " + digits.slice(2);
      } else {
        this.value = digits;
      }
    });

    document.getElementById("to-pay").addEventListener("click", function () { setStep(2); });
    document.getElementById("back-1").addEventListener("click", function () { setStep(1); });
    document.getElementById("to-confirm").addEventListener("click", function () {
      var emailVal = document.getElementById("email").value.trim() || "you@example.com";
      document.getElementById("receipt-email").textContent = emailVal;
      var id = "EB-" + Math.random().toString(36).slice(2, 10).toUpperCase();
      document.getElementById("order-id").textContent = "Order " + id;
      setStep(3);
    });
    document.getElementById("download-btn").addEventListener("click", function () {
      var btn = this;
      btn.textContent = "Downloading\u2026";
      btn.disabled = true;
      btn.style.opacity = "0.7";
      setTimeout(function () {
        btn.textContent = "Download complete!";
        btn.style.background = "var(--success)";
        btn.style.opacity = "1";
      }, 1200);
    });
  </script>
</body>
</html>
```

## How to view

Save the HTML block above to a `.html` file and open it in any browser -- no server required. All styles are inlined.
