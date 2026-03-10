# grayquest-product-assignment
Product Teardown: Google Pay UPI Journey.
#Google Pay — UPI Payment Teardown
GrayQuest Product Assignment | 2026

>Table of contents
1. Why I Picked Google Pay
2. What GPay Actually Gets Right
3. Where It Falls Short
4. What I'd Fix and How I'd Measure It
5. Roadmap — What to Build First
6. How AI Could Make This Journey Better
7. How I Used AI to Write This


________________________________________
1. Why I Picked Google Pay
Honestly, Google Pay was an obvious choice for me. I use it almost every day — splitting bills, paying at shops, sending money home. And the more I used it, the more I noticed small things that frustrated me. That's exactly what a product teardown is about.
GPay is the most used UPI app in India right now, somewhere around 35% of all UPI transactions go through it. That's hundreds of millions of people. So even a tiny improvement in the experience has a massive real-world impact. That's why I picked it.
The specific journey I focused on is the Send Money flow — when you want to pay someone using their UPI ID or by scanning a QR code. It sounds simple, but there are at least 4 or 5 moments in that flow where things can go wrong, and GPay handles some of them really poorly.
The flow looks like this: you open the app, you either type in a UPI ID or scan a QR code, you enter the amount, you put in your UPI PIN, and then you either get a success screen or some version of an error. That last part — the error — is where most of the real problems are hiding.
________________________________________
2. What GPay Actually Gets Right
Before pointing out problems, it's only fair to acknowledge what works well, because GPay does get a lot of things right.
The speed is genuinely impressive. Most payments go through in under 2 seconds. When it works, it's almost magic — you scan, you pay, done. There's no loading screen drama, no redirects, nothing. That core experience is excellent.
The UPI PIN input is also done well. It uses the native keyboard built into Android, which means there's no ugly pop-up or third-party overlay. It just feels native and smooth. A lot of smaller apps mess this up.
The QR scanner is universal — it works with any QR code from any bank or merchant, whether it's a BharatQR sticker on a vegetable cart or a dynamic code from a restaurant POS. That's not easy to build and GPay does it reliably.
The recent contacts feature is something I genuinely appreciate. If I've paid someone before, they show up on my home screen and I can pay them again in literally one tap. For recurring payments — rent, splitting food with friends — this saves real time every week.
And the regional language support is underrated. Hindi plus seven other languages means GPay is actually usable by people who aren't English-comfortable. That's a real product decision that required real effort.
________________________________________
 3. Where It Falls Short
Now the honest part.
The pending problem is genuinely bad. When a payment fails or gets stuck, GPay shows you "Payment Pending" and that's it. No estimate of when it'll resolve. No explanation of what happened. Just pending. I've had payments sit in that state for over an hour, and during that time I had no idea if the money left my account or not. I ended up calling my bank. That's a product failure — not a bank failure. GPay should be telling me what's happening.
Error messages tell you nothing. If a payment fails, the message you get is something like "Payment Declined" or "Transaction Failed." That's it. But declined why? Was it because I hit my daily UPI limit? Wrong PIN? My bank's server was down? Paytm and PhonePe at least try to give you a reason. GPay just shrugs.
You can send money to the wrong person and there's no safety net. This is a big one. UPI transfers are irreversible — once the money goes, it's gone. NPCI has no recall mechanism. But GPay doesn't show you the recipient's name before you confirm. PhonePe shows you the registered bank name. That one small thing would prevent so many accidental transfers, especially for new users who might mistype a UPI ID.
Dispute resolution is stuck in 2015. If something goes wrong with a payment — money deducted but not received — your only option in GPay is to raise a ticket that goes somewhere into an email void. There's no chat, no status tracker, no timeline. PhonePe has a proper in-app chat for disputes. Paytm has one too. GPay makes you feel abandoned the moment things go wrong, which is exactly when you need the app to help you most.
There are basically no spending insights. GPay has a transaction history, but it's just a flat list sorted by date. There's no way to see how much you spent on food this month, or how your spending compares to last month. Apps like Cred and even some banking apps do this well. For an app that processes lakhs of transactions per user per year, the analytics are almost nonexistent.
________________________________________
4. What I'd Fix and How I'd Measure It
Fix 1 — Show a real-time payment status tracker
The problem is that when a UPI payment is in progress or stuck, the user has zero visibility into what's happening. This causes panic, duplicate payments, and bank calls.
The fix is straightforward: show a three-stage progress indicator — "Bank Debit Confirmed → NPCI Processing → Recipient Bank Credit" — with a rough time estimate next to it. Something like "Usually completes in 2–4 minutes." This already exists as API data. It just needs to be surfaced in the UI.
To know if it worked: track whether duplicate payment attempts for the same recipient within a short window go down (target: 40% reduction), and check if post-payment CSAT scores improve.
Fix 2 — Contextual error messages with a next step
The problem is that "Payment Declined" is useless. Users don't know what to do next so they give up or call customer care.
The fix is to map NPCI's existing error codes — which are already returned in the API response — to plain English messages with a clear action. For example: "Your daily UPI limit of Rs. 1 lakh has been reached. It resets at midnight, or you can switch to your HDFC account to pay now." This is mostly a frontend change. The data is already there.
To know if it worked: check whether support tickets related to payment failures go down (target: 30% reduction), and whether same-session retry success rates improve after seeing the new error message.
Fix 3 — Show the recipient's name before confirming payment
The problem is that wrong UPI ID transfers are permanent. People make typos. People get scammed. There's no second chance.
The fix is simple: use the UPI Collect API — which GPay already calls in the background — to fetch and display the recipient's bank-registered name on the confirmation screen before the user enters their PIN. One extra piece of information. Takes one extra API response field. Massive reduction in wrong transfers.
To know if it worked: track wrong-transfer complaints to support (target: 60% drop), and see if first-time user retention improves because new users feel more confident.
Fix 4 — In-app AI dispute chatbot
The problem is that when a payment fails and money is deducted, users are completely stranded. The email-based support feels like shouting into a void. This is the moment users consider switching to PhonePe.
The fix is an in-app chatbot that automatically files an NPCI chargeback dispute on the user's behalf, tracks the resolution status, and sends updates. It escalates to a human only when it can't resolve automatically. The bot flow for most standard failure cases is actually quite predictable and automatable.
To know if it worked: measure average dispute resolution time (target: 48 hours down to 6), and track NPS score specifically for users who experienced a failed transaction.
________________________________________
5. Roadmap — What to Build First
Not everything can be built at once, and a good PM has to be honest about what's hard and what's easy.
Months 0 to 3 — The quick wins
Beneficiary name validation and contextual error messages should both be done in the first three months. They require no new backend infrastructure — the data is already available in existing API responses. This is mostly frontend work and some product copy decisions. These two fixes alone will meaningfully reduce support tickets and wrong transfers. I'd also run A/B
tests on error message copy during this phase to find what wording actually gets users to retry successfully.
Months 3 to 6 — The real fixes
This is when you build the payment status tracker properly — including the polling mechanism that updates the UI in real time as the transaction progresses through NPCI. The AI dispute chatbot also goes here, along with basic spending categories and push notifications for pending transactions. These need proper backend work and some ML data pipelines, so they take longer.
Months 6 to 12 — The differentiators
This phase is about pulling ahead of competitors. Voice UPI (being able to say "Pay Swiggy Rs. 250"), AI-generated spending insights that actually give you useful advice, bill-split automation for group expenses, and smarter authentication flows. These require real AI infrastructure and model training. They're not quick wins, but they're what turns GPay from a payments app into a financial companion.
The logic behind this ordering is simple: P1 items have the most impact per line of engineering effort. P2 items need backend investment but fix the most painful user moments. P3 items need AI infrastructure but create genuine competitive differentiation.
________________________________________
6. How AI Could Make This Journey Better
Beyond the improvements I listed above, there's an opportunity to build a fully automated AI workflow around the payment journey — something that handles failures end-to-end without the user having to do anything.
Here's how it would work in practice:
When a user says "pay my electricity bill" — either by typing or speaking — an AI model detects the intent and pulls up the right biller automatically from the user's payment history and a public biller database. No manual searching.
Before the payment goes through, an AI fraud scoring model checks the recipient UPI ID in under 300 milliseconds — looking at account age, transaction frequency, reported scam flags, and NPCI blacklists. If something looks suspicious, the user gets a warning before they confirm.
Then, before the PIN step, the system checks whether the user has hit their daily limit on their primary bank account, whether that bank is currently experiencing downtime, and automatically suggests switching to a backup account if needed. This prevents failures before they happen.
If the user has a history of PIN entry failures at certain times of day, an ML model can predict when they might fail and show a gentle reminder: "Double-check your PIN before confirming." Small thing, but it reduces failed auth attempts.
And if the payment does fail? The agent automatically files an NPCI dispute, monitors for resolution every 30 minutes, and notifies the user when it's resolved. The user doesn't have to do anything.
This is what "agentic" means — not just one AI feature, but a chain of AI steps that each solve one part of the problem automatically, in sequence.
The smaller AI feature I'd ship first is what I'd call a Smart Spending Coach. After every transaction, a lightweight language model generates one personalized line of insight below the receipt. Something like: "You've spent Rs. 4,200 on Zomato this month — that's 40% more than last month." Simple, specific, useful. No dashboard needed. Just one sentence at the right moment.
________________________________________
7. How I Used AI to Write This
I used Claude (by Anthropic) to help structure this analysis. I want to be transparent about exactly how.
For the teardown and improvements section, I asked: "Act as a senior Product Manager. Analyze the Google Pay UPI payment journey — what does it do well, where is there friction compared to PhonePe and Paytm, and propose improvements with measurable success metrics." Claude gave me a broad list. I removed several suggestions that don't apply to how NPCI actually works in India — for example, suggestions around "instant refunds" which aren't possible under current UPI rails. I added the beneficiary name validation point myself after remembering how PhonePe handles it differently.
For the agentic workflow, I asked: "Design a 5-step AI agentic workflow that automatically handles UPI payment failures end-to-end — from intent detection to auto-dispute filing, specific to India's NPCI infrastructure." I then adjusted Step 4 (the auth assist) to be more realistic — ML can predict failure risk but can't prevent a user from entering the wrong PIN, so I framed it as a nudge rather than an intervention.
For the roadmap, I asked: "Given these 4 improvements, create a phased 12-month GTM roadmap prioritised by impact versus engineering effort for Indian fintech." Claude suggested Beneficiary Validation in Phase 2. I moved it to Phase 1 because it only needs one additional field from an API call that GPay already makes — it's not a new backend build.
Everything in this document — the reasoning, the prioritization logic, the India-specific framing — reflects my own thinking. AI helped me structure faster and think through edge cases I might have missed. But the conclusions are mine.
________________________________________

