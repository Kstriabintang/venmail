<div align="center">

# 📨 Venmail — Anonymous Temporary Email

### A privacy-first disposable inbox that actually receives the verification codes other temp-mail services can't.

[![Live](https://img.shields.io/badge/Live-venmail.my.id-34d399?style=for-the-badge)](https://venmail.my.id)
[![Next.js](https://img.shields.io/badge/Next.js_14-000000?style=for-the-badge&logo=next.js&logoColor=white)](#)
[![Cloudflare](https://img.shields.io/badge/Cloudflare_Workers-F38020?style=for-the-badge&logo=cloudflare&logoColor=white)](#)
[![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)](#)

**🔗 Live demo — [venmail.my.id](https://venmail.my.id)**

<br/>

<img src="docs/screenshots/inbox.png" alt="Venmail inbox" width="900"/>

<sub>Get a working email address in one second — no signup — and watch verification codes land in real time.</sub>

</div>

---

> **About this repository**
> This is a **public showcase / case study** of a private project — product write-up and screenshots only. **The source code is proprietary and not included.** See [License](#-license).

---

## ⚡ Why Venmail?

Most disposable-email services share a handful of well-known "throwaway" domains that big platforms have **blacklisted** — so the verification email never arrives, and you're stuck.

**Venmail runs on its own private domain** (`venmail.my.id`) over real Cloudflare Email Routing — so it looks like an ordinary mailbox, **not** a flagged temp-mail domain.

> ✅ **The result:** Venmail reliably receives OTP / verification codes from services that *reject* typical disposable email — tested with **ChatGPT (OpenAI)**, **Booking.com**, and more. You can actually complete the signup.

---

## ✨ Features

- ⚡ **Instant inbox, zero signup** — a random anonymous address is created automatically on your first visit.
- 📥 **Real-time mail** — new messages arrive within ~1 second, with desktop notifications and a sound alert.
- 🛡️ **Gets through where others are blocked** — private domain = OTP codes from major services actually land.
- ✍️ **Custom address (optional)** — pick `you@venmail.my.id` and an optional password.
- 🔁 **Reusable across devices** — set a password and log back into the same inbox anywhere.
- 📧 **Full email reader** — clean HTML rendering (safely sandboxed), inline images, plain-text view.
- ⏳ **Auto-expiry for privacy** — inboxes and messages self-delete; nothing lingers.
- 🌙 **Polished dark UI** — responsive, animated, mobile-first.

---

## 📸 See it in action

**Receiving a verification code, then using it to create a real account:**

| 📨 OTP arrives instantly | ✅ Real ChatGPT account created with a Venmail address |
|:---:|:---:|
| <img src="docs/screenshots/verification.png" width="430"/> | <img src="docs/screenshots/in-use.png" width="430"/> |

**Reading a full HTML email (safely sandboxed):**

<div align="center">
<img src="docs/screenshots/email.png" alt="Email reader" width="760"/>
</div>

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | Next.js 14 (App Router), React 18, TypeScript, Tailwind CSS |
| **Hosting** | Cloudflare Pages (edge runtime, `@cloudflare/next-on-pages`) |
| **API / Backend** | Cloudflare Workers (TypeScript) |
| **Database** | Cloudflare D1 (SQLite) |
| **Inbound email** | Cloudflare Email Routing → Worker `email()` handler, MIME parsing with `postal-mime` |
| **Auth** | Stateless JWT (HMAC), HttpOnly cookies, PBKDF2 password hashing |
| **Tooling** | Wrangler, strict TypeScript, ESLint |

---

## 🏗️ Architecture

```
                    ┌──────────────────────────────┐
   Browser  ───────▶│  Next.js on Cloudflare Pages │
                    │  (edge runtime, dark UI)     │
                    └───────────────┬──────────────┘
                                    │  same-origin proxy
                                    │  (Service Binding, HttpOnly cookie ↔ JWT)
                                    ▼
                    ┌──────────────────────────────┐
   Inbound mail ───▶│      Cloudflare Worker        │───▶  D1 (SQLite)
   (Email Routing)  │  REST API + email() handler   │      accounts · inboxes · messages
                    └──────────────────────────────┘
```

**Design highlights**
- A **real custom domain on Cloudflare Email Routing** — the key reason verification emails from strict providers actually get delivered.
- The browser never talks to the Worker directly — a **same-origin proxy backed by a Service Binding** forwards requests Worker-to-Worker. This sidesteps the cross-origin blocking, ad-blockers, and DNS filters that break disposable-mail domains, and bridges the **HttpOnly JWT cookie** so tokens never touch JavaScript (XSS-safe).
- Clean **Account → Inbox → Message** model with a sliding expiry that keeps active inboxes alive and quietly retires idle ones.
- Real-world **email engineering**: MIME + inline-image handling and sender-address normalization for a clean, readable inbox.

---

## 🔒 Privacy & Security

- **No signup, no personal data** — anonymous by default.
- **Auto-expiry** — inboxes and messages are deleted automatically; data doesn't pile up.
- **HttpOnly, Secure, SameSite cookies** — the JWT is never exposed to client-side JavaScript.
- **PBKDF2-HMAC-SHA256** hashing for optional passwords (per-user salt).
- **Sandboxed email rendering** — untrusted HTML runs in an `<iframe sandbox>` with scripts disabled.

---

## 📈 Engineering Highlights

- 100% **serverless on the edge** — sub-second responses, globally distributed, near-zero idle cost.
- **Strict TypeScript** across the frontend and the Worker; clean typecheck + build gates before every deploy.
- **Failure-safe by design** — email parsing and background cleanup degrade gracefully and never block delivery.

---

## 📄 License

**© 2026 Ksatria Bintang Samudra. All rights reserved.**

This repository is a portfolio showcase. The source code of Venmail is **proprietary and not licensed for reuse, copying, or distribution**. The text and screenshots here are provided for evaluation purposes only.

---

<div align="center">

**Built & maintained by Ksatria Bintang Samudra**

🌐 [venmail.my.id](https://venmail.my.id)

</div>
