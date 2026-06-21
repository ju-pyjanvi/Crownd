👑 CROWND
Where Every Purchase Is Your Crowning Moment

Influencer Wallets    Verified UGC    Purchase-Linked Cashback    Brand Campaign Tiers    In Progress

Real Buyers • Real Posts • Instagram-Verified Cashback • Crownd Score Matching • FastAPI + Supabase Backend

TRY IT OUT — frontend/index.html (open locally, see Getting Started below)

Backend API — http://127.0.0.1:8000 (local) · Render deployment in progress

Repository — https://github.com/ju-pyjanvi/Crownd

📌 System Overview
Crownd turns micro-influencers — anyone with 1,000+ followers — into a brand's cheapest and most authentic marketing channel. They shop at partner brands using a Crownd Wallet, post about it on Instagram, and get real cashback credited back. Brands pay only when a real purchase and a real verified post both happen — not for impressions, not for reach promises.

Traditional influencer marketing pays creators a flat fee whether or not the content performs, and brands have no way to confirm a post is tied to an actual purchase. Crownd closes that loop: purchase, post, and reach all flow through one platform, so cashback is only ever paid out against something that's been independently verified.

✨ Key Features
Feature	Description
Crownd Wallet	An in-app balance every influencer earns into and spends from — funded by cashback, loaded via Razorpay for purchases at partner brands.
Purchase → Post → Cashback Loop	Influencer logs a purchase against a partner brand, posts on Instagram tagging the brand, and once the post is verified, cashback is calculated and credited automatically.
Crownd Score	A single score (0–1000) built from verified post count, engagement rate, and brand diversity — not loyalty to one brand. Higher score unlocks access to premium brand tiers.
Three Brand Campaign Tiers	Coronet (cashback model for small/mid brands), Regalia (barter/free product seeding), and Sovereign (premium curated campaigns, Crownd Score 750+ only).
Instagram Graph API Verification	Real post verification via Instagram's Graph API (Business Login + Facebook Login flow) — checks brand tagging and account engagement before cashback releases.
JWT Auth	Email/password signup and OAuth2-password-flow login, with bearer-token-protected routes across every wallet, purchase, and post endpoint.
Razorpay Wallet Top-Ups	Real test-mode payment orders, client-side checkout widget, and server-side signature verification before any balance is credited.
🖥️ Platform Walkthrough
Landing — Shop. Post. Get Paid.
The homepage opens on a soft mint-and-blob aesthetic in Crownd's maroon, marigold, and mehendi-green palette. A "For Creators" / "For Brands" toggle switches the entire page between two audiences without a reload. The hero headline runs Shop. / Post. / Get Paid. in a bold sans stack, with "Get Paid." set in maroon serif italic — the same italic face echoed throughout the site's headings.

How It Works
A three-step flow card explains the loop in plain language: shop at a partner brand using the Crownd Wallet, post a real opinion tagging the brand, and once the post is verified, cashback lands back in the wallet — spendable at any other partner brand.

Where You Can Earn — The Solitaire Deck
Three category cards — Fashion & Beauty, Food & Dining, Lifestyle & Tech — rest fanned like a held hand of cards. Hovering the deck spreads them across the table, each card finished in a liquid-glass effect with a soft sheen and a worked rupee-cashback example underneath.

Your Crownd Score — The Rising River
Instead of four boxed tiers, the Crownd Score progression (Signet → Coronet → Royal → Sovereign) is rendered as a single flowing river line climbing left to right — because a score is a progression, not four separate things.

For Brands — Choose How You Want to Grow
A dedicated brands page lists the three partnership tiers as claymorphic cards with soft puffy shadows: Coronet for pay-per-verified-outcome cashback campaigns, Regalia for product seeding with no wallet transaction, and Sovereign for Crownd Score-gated curated collaborations with a featured, visually lifted card.

Dashboard — Wallet, Score, and Live Campaigns
Once signed in, influencers see their real wallet balance, Crownd Score breakdown, and a campaign ladder of eligible vs. locked brand partnerships pulled live from the backend — starting a campaign calls the purchase endpoint directly and refreshes wallet/score state on completion.

Wallet — Top-Ups and Transaction History
A dedicated wallet view triggers the full Razorpay flow: requesting an order from the backend, opening the real Razorpay checkout widget, and verifying the payment signature server-side before the balance updates. Transactions are tagged cashback / topup / payment for filtering, inferred from the backend's credit/debit + description fields.

🛠️ Architecture & Tech Stack
Crownd combines a FastAPI backend, a managed Postgres database, and a dependency-free frontend.

Backend: FastAPI (Python) — REST API across 5 routers: influencer (auth/signup/login/score), brand (listing, tiers, eligibility), wallet (balance, Razorpay load/verify, transactions), purchase (initiate, tier-based cashback calculation), post (Instagram verification, cashback credit)
Database: PostgreSQL via Supabase — SQLAlchemy ORM models for Users, Wallets, Transactions, Brands, Purchases, and Crownd Scores
Auth: JWT bearer tokens (python-jose), OAuth2PasswordRequestForm-based login (form-encoded, matching Swagger/standard OAuth2 conventions), bcrypt password hashing via passlib
Payments: Razorpay — test-mode order creation, HMAC SHA-256 signature verification on payment confirmation, wallet credit only after server-side verification passes
Social Verification: Instagram Graph API — Business Login via linked Facebook Page, instagram_basic / instagram_manage_insights / pages_show_list / pages_read_engagement scopes, mockable via INSTAGRAM_ACCESS_TOKEN=mock for demo/dev environments
Frontend: Vanilla HTML/CSS/JS, no build step, no framework — five standalone pages (index, auth, dashboard, wallet, partner) linked by plain navigation, deployable anywhere static files are served
Hosting: Render (FastAPI backend) — root directory backend, build pip install -r requirements.txt, start uvicorn main:app --host 0.0.0.0 --port $PORT
Version Control: GitHub — .env and all secrets excluded via .gitignore, requirements.txt generated via pip freeze
🔬 How The Cashback Loop Works
Crownd verifies three things before a single rupee of cashback moves — purchase, post, and reach:

Purchase: Influencer initiates a purchase against a partner brand inside the app (POST /purchase/initiate). The backend checks the influencer's Crownd Score against the brand's min_crownd_score, confirms wallet balance, and checks the brand-specific redemption cap before logging the purchase as post_required.

Post: Influencer posts on Instagram tagging the brand and submits the post URL (POST /post/submit). The backend calls the Instagram Graph API to confirm the post exists, is tagged correctly, and meets engagement thresholds — falling back to a mock check (valid-URL pattern match) when no live token is configured.

Cashback: Once the post verifies, cashback is calculated against the brand's campaign tier rules — a percentage of purchase value, capped at the brand's max_cashback_cap — and credited directly into the influencer's Crownd Wallet as a transaction. The Crownd Score is recalculated at the same time, factoring in the new verified post and updated brand diversity.

Brand Funding: Brands never pay influencers directly. They pre-commit a monthly campaign budget to Crownd; cashback is paid out of that budget only against verified purchase+post pairs, and Crownd separately earns its own commission on the underlying transaction value.

📊 Revenue Model
Stream	Mechanism
Brand Commission	Crownd takes a percentage of the purchase value on every verified, cashback-eligible transaction — billed to the brand separately from the cashback budget itself.
Card / Wallet Fee	A small platform fee on in-app wallet spends at partner brands (near-zero marginal cost, since it never touches outside payment rails).
Visa Interchange Share	On external virtual/physical card spends (Visa rails via a licensed PPI partner), Crownd earns a negotiated share of the interchange fee the card issuer collects — standard for programme-manager fintech arrangements.
Optional Physical Card	A ₹499 one-time fee for influencers who want a physical co-branded card shipped — not a core revenue driver, mainly an activation/onboarding filter.
🪙 Wallet & Card Roadmap — Mock to Licensed PPI
Crownd's wallet is deliberately built in phases, since holding real user balances in India requires a Prepaid Payment Instrument (PPI) license from the RBI — something no early-stage team can realistically obtain in hackathon or MVP timelines (12–18 months and significant compliance investment).

Phase	Wallet Implementation	What's Real
Hackathon / Prototype (current)	Razorpay test-mode order creation + signature verification, balances tracked in Crownd's own Postgres database	The full purchase→post→cashback flow, auth, and UI are fully real; money movement is simulated via Razorpay's sandbox
MVP Launch (next)	Partnership with an RBI-licensed PPI issuer (e.g. M2P, Setu, or Decentro) as programme manager — Crownd operates on top of their license rather than holding funds directly	Real wallet balances, real virtual/physical Visa card issuance, real interchange revenue share
Scale (later)	Potential direct PPI license application once transaction volume justifies the compliance cost	Full in-house custody of the payment stack
This phased approach is standard practice for Indian fintech-adjacent startups — Crownd is the platform and programme-manager layer; the licensed partner is the regulated rail underneath it.

🚀 Getting Started
Prerequisites
Python 3.10+
A Supabase project (Postgres connection string + anon key)
Razorpay test-mode API keys
A Meta Developer app with Instagram Graph API access (optional — mockable for local dev)
Installation
Clone the repository:

git clone https://github.com/ju-pyjanvi/Crownd.git
cd Crownd/backend
Create a virtual environment and install dependencies:

python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # macOS/Linux
pip install -r requirements.txt
Create your .env file in the backend folder:

DATABASE_URL=postgresql://postgres:[PASSWORD]@db.[PROJECT_REF].supabase.co:5432/postgres
SUPABASE_URL=https://[PROJECT_REF].supabase.co
SUPABASE_KEY=your_supabase_anon_key
RAZORPAY_KEY_ID=your_razorpay_test_key_id
RAZORPAY_KEY_SECRET=your_razorpay_test_key_secret
SECRET_KEY=your_jwt_signing_secret
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=10080
INSTAGRAM_ACCESS_TOKEN=mock
Run the backend:

uvicorn main:app --reload
Open the frontend — no build step required:

cd ../frontend
# open index.html directly in a browser
🎯 How It Works
Step 1: Apply → Sign up with email, password, and Instagram handle (POST /auth/signup)
Step 2: Sign In → Receive a bearer token, redirected to the dashboard (POST /auth/login)
Step 3: Load Wallet → Top up the Crownd Wallet via Razorpay (POST /wallet/load → verified via /wallet/verify-payment)
Step 4: Shop & Post → Start a campaign with an eligible brand, purchase, then submit an Instagram post URL (POST /purchase/initiate → POST /post/submit)
Step 5: Get Paid → Verified posts trigger automatic cashback credit and Crownd Score growth

📁 Repository Structure
Crownd/
├── backend/
│   ├── main.py                    # FastAPI app entrypoint, router registration, CORS
│   ├── database.py                # SQLAlchemy engine + session config
│   ├── models.py                  # User, Wallet, Transaction, Brand, Purchase, CrowndScore
│   ├── schemas.py                 # Pydantic request/response models
│   ├── auth.py                    # JWT creation/validation, password hashing, current-user deps
│   ├── requirements.txt           # Python dependencies (pip freeze)
│   ├── .gitignore                 # Excludes .env and local artifacts
│   └── routers/
│       ├── influencer.py          # Signup, login, profile, Crownd Score
│       ├── brand.py                # Brand listing, tiers, eligibility
│       ├── wallet.py               # Balance, Razorpay load/verify, transaction history
│       ├── purchase.py             # Purchase initiation, tier-based cashback rules
│       ├── post.py                 # Instagram post submission + verification + cashback credit
│       └── instagram_oauth.py      # Instagram Graph API OAuth callback handling
├── frontend/
│   ├── index.html                  # Landing page — Creators / Brands toggle
│   ├── auth.html                   # Sign up / sign in
│   ├── dashboard.html              # Wallet, Crownd Score, campaign ladder
│   ├── wallet.html                 # Top-ups, transaction history
│   ├── partner.html                # Brand partnership application
│   ├── auth_callback.html          # Instagram OAuth redirect handler
│   └── shopfirst.html              # First-purchase / voucher flow
├── CROWND.pdf                      # Hackathon pitch deck
└── README.md
👥 Built By
Designed and engineered end-to-end for the hackathon:

Business Model — Cashback economics, three-tier brand campaign structure, Crownd Score design, and the brand-funds-cashback-separately revenue split
Backend Architecture — FastAPI + Supabase, JWT auth, Razorpay payment verification, Instagram Graph API integration, tier-based cashback calculation engine
Frontend & Design — Vanilla HTML/CSS/JS across five linked pages, signature solitaire-deck and rising-river visual elements, maroon/marigold/mehendi-green identity system
Research — Market sizing, creator economy statistics, and CAC-reduction benchmarks sourced from EY, BCG, Kofluence, and Nielsen for the hackathon research annex

Built so every purchase an influencer already makes pays them back — one verified post at a time.

📄 License
MIT License — see LICENSE file for details.

Where every purchase is your crowning moment. 👑
