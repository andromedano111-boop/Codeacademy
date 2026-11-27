# CodeAcademy GH

A responsive programming-learning platform starter with course cards, lesson pages, a JavaScript playground, a challenge, local progress tracking and subscription pricing.

## Included
- Python, JavaScript, HTML/CSS, C++, Java, SQL, PHP and TypeScript course catalogue.
- Sample lessons and code examples.
- Browser JavaScript playground (not a multi-language compiler).
- Quiz challenge and XP progress saved in localStorage.
- Subscription pricing UI.
- Express + Paystack checkout initialization starter.
- Paystack webhook signature verification starter.
- Supabase SQL schema.

## Run the learning website
No build step is required for the frontend.

```bash
python3 -m http.server 8080
```
Open http://localhost:8080

Or use VS Code Live Server.

## Run the backend
Requires Node.js 20+.

```bash
npm install
cp .env.example .env
npm start
```

The server is at http://localhost:3000. Check http://localhost:3000/health.

Set `PAYSTACK_SECRET_KEY` in `.env` using a test key from your Paystack dashboard. Never put the secret key in frontend JavaScript or commit `.env`.

## Paystack setup
1. Create and verify a Paystack merchant account.
2. Use test keys first.
3. Set `SITE_URL` to your public site URL.
4. Configure the webhook URL as `https://YOUR_DOMAIN/api/webhooks/paystack`.
5. Test card and Mobile Money flows.
6. Request live mode after completing provider onboarding.

The checkout endpoint accepts `email` and `planId` (`student_pro` or `annual_pro`). It uses server-side prices, not prices supplied by the browser.

Example request:
```bash
curl -X POST http://localhost:3000/api/checkout \
  -H 'Content-Type: application/json' \
  -d '{"email":"student@example.com","planId":"student_pro"}'
```

## IMPORTANT: production work still required
This is a functional front-end MVP and payment integration scaffold, not a finished production SaaS. Before charging customers:
- Add real authentication and user identity checks to checkout.
- Connect Supabase from trusted server code.
- Create pending payment/subscription rows before initializing checkout.
- Verify transactions server-side against Paystack's verify endpoint, checking reference, amount, currency and status.
- Process webhook events idempotently and update subscription periods atomically.
- Add cancellation, renewal, refund, grace-period and failed-payment handling.
- Add complete Supabase RLS policies and test them with separate users.
- Add rate limiting, input validation, logging, backups and error monitoring.
- Add privacy policy, terms, refund policy and contact/support details.
- Do not issue certificates until course completion rules are validated.
- Mobile Money recurring renewal should be treated as a new customer-initiated payment unless an eligible recurring debit mandate is separately implemented.

## Deploy
### Static frontend
Deploy this folder to Netlify. Set the publish directory to the project root. For a production application with server endpoints, deploy the Express backend to a Node-capable host or migrate routes to Netlify Functions. Configure the Paystack webhook to point to the backend's public URL.

### Database
1. Create a Supabase project.
2. Run `database/schema.sql` in the SQL editor.
3. Review the RLS policies before connecting the browser.
4. Add a secure server-side Supabase client for privileged operations.

## Project structure
```text
codeacademy-gh/
├── index.html
├── styles.css
├── app.js
├── package.json
├── .env.example
├── README.md
├── database/
│   └── schema.sql
└── server/
    └── server.js
```
