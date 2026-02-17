# ✅ TeleSwiftVPN - Checklist البيانات المطلوبة

## قبل ما تبدأ، جمّع البيانات دي:

---

### 🤖 بيانات البوت
- [ ] BOT_TOKEN → من @BotFather
- [ ] BOT_DEV_ID → Telegram ID بتاعك (من @userinfobot)
- [ ] BOT_SUPPORT_USERNAME → يوزرنيم الدعم

---

### 🌐 بيانات Railway
- [ ] رابط البانيل (بعد نشر Remnawave)
- [ ] رابط البوت (بعد نشر remnashop)

---

### 🔑 مفاتيح سرية (ولّدها بـ openssl)
- [ ] JWT_AUTH_SECRET (64 hex)
- [ ] JWT_API_TOKENS_SECRET (64 hex)
- [ ] WEBHOOK_SECRET_HEADER (64 chars) ← **نفس القيمة في البانيل والبوت**
- [ ] APP_CRYPT_KEY (base64 32)
- [ ] BOT_SECRET_TOKEN (64 hex)
- [ ] POSTGRES_PASSWORD (للبانيل)
- [ ] DATABASE_PASSWORD (للبوت)
- [ ] REDIS_PASSWORD (للبوت)
- [ ] METRICS_PASS (للبانيل)

---

### 💳 بيانات الدفع
- [ ] Cryptomus API Key + Merchant ID
- [ ] CryptoPay Token (من @CryptoBot)
- [ ] YooKassa Secret Key + Shop ID
- [ ] YooMoney Wallet ID + Notification Secret

---

### 🔑 من بانيل Remnawave (بعد التشغيل)
- [ ] REMNAWAVE_TOKEN (من Settings > API Tokens في البانيل)

---

## ترتيب الخطوات:

```
1. ولّد المفاتيح السرية
2. أنشئ البوت من BotFather
3. انشر Remnawave Panel على Railway
4. خد رابط البانيل + أنشئ API Token
5. انشر البوت (remnashop) على Railway
6. اضبط Webhooks للدفع
7. شغّل وتأكد
```
