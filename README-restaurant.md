<div align="center">

# 🍔 عمو رسول | فست‌فود ارومیه

سیستم سفارش آنلاین و پنل مدیریت برای فست‌فود «عمو رسول» — طراحی‌شده برای کار کردن در ایران **بدون نیاز به VPN**.

[![Cloudflare Workers](https://img.shields.io/badge/Cloudflare-Workers-F38020?style=flat-square&logo=cloudflare&logoColor=white)](https://workers.cloudflare.com/)
[![Supabase](https://img.shields.io/badge/Supabase-Database-3ECF8E?style=flat-square&logo=supabase&logoColor=white)](https://supabase.com/)
[![Zarinpal](https://img.shields.io/badge/Zarinpal-Payments-FFD700?style=flat-square)](https://www.zarinpal.com/)
[![License](https://img.shields.io/badge/license-Private-lightgrey?style=flat-square)]()

</div>

---

## 📖 درباره پروژه

بیشتر سرویس‌های خارجی (Supabase، فونت‌های گوگل، Unsplash، تایل‌های نقشه‌ی OpenStreetMap) از ایران بدون VPN فیلتر یا کند هستن. برای همین معماری این پروژه طوری طراحی شده که **مرورگر کاربر ایرانی هیچ‌وقت مستقیم به این سرویس‌ها وصل نمی‌شه** — همه‌چیز از طریق یک Cloudflare Worker به‌عنوان لایه‌ی پروکسی رد می‌شه.

```
مرورگر کاربر  →  Cloudflare Worker (api.arhanhatami.ir)  →  Supabase / Zarinpal / Melli Payamak / OSM
```

## ✨ امکانات

- 🛒 سبد خرید و ثبت سفارش آنلاین با تایید شماره موبایل (OTP)
- 💳 پرداخت آنلاین با درگاه زرین‌پال
- 📦 پیگیری لحظه‌ای وضعیت سفارش برای مشتری
- 📱 پیامک تایید سفارش با ملی‌پیامک
- 🖼 پروکسی و کش عکس‌ها (منو، آواتار، نقشه) — بدون اتصال مستقیم به سرویس‌های فیلتر
- 🛠 پنل مدیریت کامل: سفارش‌ها، منو (با آپلود عکس)، میزها، پیک‌ها، مشتریان، گزارش‌ها، تخفیف‌ها
- 🩺 مانیتورینگ و لاگ رویدادهای بحرانی + هشدار تلگرام
- 🔒 احراز هویت نقش‌محور (role-based) برای پنل ادمین

## 🧱 استک تکنولوژی

| لایه | تکنولوژی |
|---|---|
| Frontend سایت مشتری | HTML / CSS / Vanilla JS |
| پنل ادمین | HTML / CSS / Vanilla JS |
| Backend / API | Cloudflare Workers |
| دیتابیس و Storage | Supabase (Postgres + Storage) |
| پرداخت | Zarinpal |
| پیامک | ملی‌پیامک (SMS + OTP اختصاصی) |
| فونت | Vazirmatn (self-hosted, variable font) |
| Rate limiting / OTP state | Cloudflare KV |

## 📂 ساختار پروژه

```
├── index.html      # سایت مشتری
├── admin.html      # پنل مدیریت
├── worker.js       # Cloudflare Worker (API + پروکسی تصاویر/نقشه)
└── fonts/
    └── Vazirmatn[wght].woff2
```

## 🚀 دیپلوی

1. **Worker**: کد `worker.js` رو با Wrangler یا مستقیم تو داشبورد Cloudflare دیپلوی کن.
2. **Environment variables / Secrets** موردنیاز:
   - `SUPABASE_URL`, `SUPABASE_KEY`
   - `ZARINPAL_MERCHANT_ID`, `SITE_URL`
   - `MELLIPAYAMAK_USERNAME`, `MELLIPAYAMAK_PASSWORD`, `MELLIPAYAMAK_SENDER`, `MELLIPAYAMAK_OTP_TOKEN`
   - (اختیاری) `TELEGRAM_BOT_TOKEN`, `TELEGRAM_CHAT_ID`
3. **KV Namespace**: یک namespace به اسم `RATE_KV` بساز و به Worker بایند کن (برای rate limit و OTP).
4. **فایل‌های استاتیک** (`index.html`, `admin.html`, `fonts/`) رو روی هاست دلخواه (مثلاً GitHub Pages یا Cloudflare Pages) آپلود کن.
5. دامنه‌ی Worker رو روی `api.<your-domain>` ست کن.

## 🔐 نکات امنیتی

- هیچ کلید سرویسی (Supabase service key و...) تو کد فرانت‌اند نیست — همه از Worker رد می‌شن.
- قیمت نهایی سفارش همیشه سمت سرور از دیتابیس محاسبه می‌شه، نه از ورودی کاربر.
- ثبت سفارش نیازمند تایید شماره موبایل با OTP هست.
- دسترسی پنل ادمین بر اساس نقش (`role = admin`) در جدول `user_roles` کنترل می‌شه.

---

<div align="center">
ساخته‌شده با ❤️ برای عمو رسول — ارومیه
</div>
