# 🚀 دليل تشغيل TeleSwiftVPN على Railway
## خطوة بخطوة بالعربي

---

## 📋 المتطلبات قبل البدء

- حساب على https://railway.app
- بوت تيليجرام من @BotFather (واسمه TeleSwiftVPN)
- دومين (اختياري - Railway بيعطيك subdomain مجاني)
- حساب على Cryptomus / CryptoPay / YooKassa حسب طرق الدفع المطلوبة

---

## 🗂️ هيكل الملفات

```
teleswiftvpn/
├── remnawave/          ← بانيل إدارة VPN
│   ├── .env
│   └── docker-compose.yml
└── remnashop/          ← بوت التيليجرام
    ├── .env
    └── docker-compose.yml
```

---

## 🔑 الخطوة 0: توليد المفاتيح السرية

افتح Terminal وشغّل الأوامر دي:

```bash
# مفتاح 1: JWT_AUTH_SECRET للبانيل
openssl rand -hex 64

# مفتاح 2: JWT_API_TOKENS_SECRET للبانيل
openssl rand -hex 64

# مفتاح 3: WEBHOOK_SECRET_HEADER (نفس القيمة في البانيل والبوت)
openssl rand -hex 32   # ⚠️ اضبط الناتج بالضبط 64 حرف

# مفتاح 4: APP_CRYPT_KEY للبوت
openssl rand -base64 32

# مفتاح 5: BOT_SECRET_TOKEN للبوت
openssl rand -hex 64

# كلمة سر DB للبانيل
openssl rand -hex 24

# كلمة سر DB للبوت
openssl rand -hex 24

# كلمة سر Redis للبوت
openssl rand -hex 24

# METRICS_PASS للبانيل
openssl rand -hex 32
```

احفظ كل القيم دي في مكان آمن.

---

## 🎯 الخطوة 1: إنشاء بوت تيليجرام

1. افتح @BotFather على تيليجرام
2. أرسل `/newbot`
3. الاسم: `TeleSwiftVPN`
4. اليوزرنيم: `TeleSwiftVPN_bot` (أو أي اسم متاح)
5. **احفظ التوكن** - هيبدو زي: `1234567890:ABCdef...`
6. اعرف الـ Telegram ID بتاعك عن طريق @userinfobot

---

## 🌐 الخطوة 2: رفع Remnawave Panel على Railway

### 2.1 إنشاء Project جديد
1. افتح https://railway.app
2. اضغط "New Project"
3. اختار "Deploy from Docker Compose"
4. ارفع ملف `remnawave/docker-compose.yml`

### 2.2 ضبط المتغيرات في Railway
في لوحة تحكم Railway، روح لـ Variables وضيف:

```
APP_PORT=3000
DATABASE_URL=postgresql://postgres:YOUR_DB_PASSWORD@remnawave-db:5432/postgres
REDIS_HOST=remnawave-redis
REDIS_PORT=6379
JWT_AUTH_SECRET=← المفتاح اللي ولّدته
JWT_API_TOKENS_SECRET=← المفتاح اللي ولّدته
IS_TELEGRAM_NOTIFICATIONS_ENABLED=false
FRONT_END_DOMAIN=*
SUB_PUBLIC_DOMAIN=← سيكون متاح بعد نشر Railway
IS_DOCS_ENABLED=false
METRICS_USER=admin
METRICS_PASS=← المفتاح اللي ولّدته
WEBHOOK_ENABLED=true
WEBHOOK_URL=https://← رابط بوت Railway/api/v1/remnawave
WEBHOOK_SECRET_HEADER=← المفتاح 64 حرف
POSTGRES_USER=postgres
POSTGRES_PASSWORD=← كلمة سر DB
POSTGRES_DB=postgres
```

### 2.3 ضبط الـ Port
في إعدادات Service > Settings:
- Port: `3000`

### 2.4 احصل على الدومين
بعد النشر، Railway سيعطيك رابط زي:
`remnawave-panel-production.railway.app`

---

## 🤖 الخطوة 3: إعداد البانيل (أول تشغيل)

1. افتح رابط البانيل في المتصفح
2. سجّل حساب المدير
3. روح لـ **Settings > API Tokens**
4. أنشئ Token جديد واحفظه - ده هو `REMNAWAVE_TOKEN` للبوت
5. روح لـ **Nodes** وضيف السيرفر بتاعك (محتاج VPS منفصل للـ VPN nodes)

---

## 🚂 الخطوة 4: رفع البوت TeleSwiftVPN على Railway

### 4.1 Project جديد للبوت
1. في Railway، أنشئ Project جديد
2. ارفع ملف `remnashop/docker-compose.yml`

### 4.2 ضبط المتغيرات

بعد ما عرفت رابط البوت من Railway، ضيف كل المتغيرات:

```
APP_DOMAIN=← رابط البوت من Railway بدون https://
APP_HOST=0.0.0.0
APP_PORT=5000
APP_LOCALES=en,ru
APP_DEFAULT_LOCALE=en
APP_CRYPT_KEY=← المفتاح اللي ولّدته

BOT_TOKEN=← توكن البوت من BotFather
BOT_SECRET_TOKEN=← المفتاح اللي ولّدته
BOT_DEV_ID=← Telegram ID بتاعك
BOT_SUPPORT_USERNAME=← يوزرنيم الدعم بدون @
BOT_MINI_APP=false
BOT_RESET_WEBHOOK=false
BOT_DROP_PENDING_UPDATES=false
BOT_SETUP_COMMANDS=true
BOT_USE_BANNERS=true

REMNAWAVE_HOST=← دومين البانيل بدون https://
REMNAWAVE_PORT=
REMNAWAVE_TOKEN=← التوكن من البانيل
REMNAWAVE_WEBHOOK_SECRET=← نفس WEBHOOK_SECRET_HEADER في البانيل

DATABASE_HOST=remnashop-db
DATABASE_PORT=5432
DATABASE_NAME=remnashop
DATABASE_USER=remnashop
DATABASE_PASSWORD=← كلمة سر DB البوت
DATABASE_ECHO=false
DATABASE_POOL_SIZE=30
DATABASE_MAX_OVERFLOW=30
DATABASE_POOL_TIMEOUT=10
DATABASE_POOL_RECYCLE=3600

REDIS_HOST=remnashop-redis
REDIS_PORT=6379
REDIS_NAME=0
REDIS_PASSWORD=← كلمة سر Redis
```

### 4.3 طرق الدفع

```
# Telegram Stars (لا يحتاج إعداد)

# Cryptomus
CRYPTOMUS_API_KEY=← من cryptomus.com/settings
CRYPTOMUS_MERCHANT_ID=← من cryptomus.com/settings

# CryptoPay - تواصل مع @CryptoBot على تيليجرام
CRYPTO_PAY_TOKEN=← من @CryptoBot

# YooKassa
YOOKASSA_TOKEN=← secret_key من yookassa.ru
YOOKASSA_SHOP_ID=← shop_id من yookassa.ru

# YooMoney
YOOMONEY_WALLET_ID=← رقم المحفظة
YOOMONEY_NOTIFICATION_SECRET=← من yoomoney.ru/transfer/myservices
```

---

## 💳 الخطوة 5: إعداد Webhooks للدفع

### YooKassa:
1. افتح https://yookassa.ru/my/merchant/integration/http-notifications
2. Notification URL: `https://YOUR_BOT.railway.app/yookassa`
3. فعّل: `payment.succeeded`, `payment.canceled`

### YooMoney:
1. افتح https://yoomoney.ru/transfer/myservices/http-notification
2. Notification URL: `https://YOUR_BOT.railway.app/yoomoney`
3. فعّل الـ HTTP notifications واحفظ الـ Secret

### Cryptomus:
1. افتح https://cryptomus.com/settings
2. أضف Webhook URL: `https://YOUR_BOT.railway.app/cryptomus`

### CryptoPay:
1. أرسل `/pay` لـ @CryptoBot
2. أنشئ App وخد التوكن

---

## ✅ الخطوة 6: التحقق من التشغيل

1. افتح بوت TeleSwiftVPN على تيليجرام
2. أرسل `/start`
3. يجب أن يرد البوت

### لو في مشكلة:
```bash
# شوف لوجز Railway من Dashboard > Logs
```

---

## 🔧 ضبط الخطط في البانيل

بعد التشغيل، روح للبانيل وضيف:
1. **Inbound** (بروتوكول VLESS أو VMess)
2. **Plans** (خطط الاشتراك وأسعارها)
3. **Nodes** (سيرفرات الـ VPN)

---

## ⚠️ ملاحظات مهمة

1. **Railway مش مجاني بالكامل** - البيئة المجانية بها حدود، للإنتاج استخدم الخطة المدفوعة (~$5-20/شهر)
2. **سيرفر VPN منفصل** - البانيل بيدير السيرفرات، لكن محتاج VPS حقيقي يشغّل الـ nodes (Remnawave Node)
3. **الـ WEBHOOK_SECRET_HEADER** يجب أن يكون **نفس القيمة بالضبط** في البانيل والبوت
4. **احفظ كل المفاتيح** - لا يمكن استرجاعها

---

## 📞 روابط مفيدة

- Remnawave Docs: https://docs.rw
- BotFather: https://t.me/BotFather
- Railway: https://railway.app
- Cryptomus: https://cryptomus.com
- CryptoPay: https://t.me/CryptoBot
- YooKassa: https://yookassa.ru
- YooMoney: https://yoomoney.ru
