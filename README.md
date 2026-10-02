# الموظف الذكي — AI Smart Employee

منصة SaaS متعددة الأنشطة (Multi-Tenant) — موظف واتساب ذكي واحد يخدم أي نشاط تجاري
(مطاعم، عيادات، صالونات، متاجر، عقارات...) عبر AI Engine واحد + Business Data مختلفة لكل Tenant.

## البنية (Monorepo)

```
ai-smart-employee/
  backend/     Node.js + Express + TypeScript + Prisma (PostgreSQL)
  frontend/    React + Vite + TypeScript (RTL, Mobile First)
```

## حالة المشروع (حتى Step 4)

- **Step 1** — سكيلتون المشروع + Database Schema (Prisma) + AIProvider abstraction (Gemini/OpenAI/Claude، لم تُفعَّل بعد).
- **Step 2** — Auth حقيقي (تسجيل/دخول/خروج) + Multi-Tenant تلقائي + RBAC (owner/admin/agent) + عزل تام بين الأنشطة (Tenant Isolation).
- **Step 3** — Business Profile + Services + Products + Offers، CRUD كامل + Zod validation + واجهات عربية RTL (Mobile First): `/business` `/services` `/products` `/offers`.
- **Step 4** — تحويل التخزين من الذاكرة إلى **PostgreSQL حقيقي عبر Prisma** (repositories، schema، migrations، seed).

**لم يُبنَ بعد:** WhatsApp Cloud API الفعلي، تفعيل Gemini/OpenAI/Claude الفعلي، Make.com، Subscriptions، Human Handoff الكامل.

## اختبارات

```bash
cd backend
npm test                  # Unit — منطق كامل بدون قاعدة بيانات (يعمل في أي بيئة)
npm run test:integration  # Integration — Express حقيقي عبر HTTP فعلي
npm run test:db           # Database — PostgreSQL حقيقي (يحتاج DATABASE_URL صالح)
```

## النشر على Render

راجع `backend/prisma/RUNNING_THE_DATABASE.md` للتفاصيل الكاملة. باختصار، عند إنشاء
Web Service على Render يشير إلى هذا المستودع:

- **Build Command:** `cd backend && npm run render:build`
- **Start Command:** `cd backend && npm run render:start`
- **Environment Variables المطلوبة:** `DATABASE_URL` (اربطها بقاعدة PostgreSQL الموجودة
  لديك على Render مباشرة من واجهة Render — لا تُكتب هنا ولا في أي ملف)، و`SESSION_SECRET`
  (قيمة عشوائية 32 حرفًا على الأقل).
- بعد أول نشر ناجح: `GET /health` يعيد حالة العملية، و`GET /ready` يعيد حالة الاتصال
  الفعلي بقاعدة البيانات.

## قيود بيئة التطوير الحالية (هذه المحادثة تحديدًا)

بيئة كلود هنا بلا اتصال إنترنت إطلاقًا (لا `npm install`، لا الاتصال المباشر بأي
قاعدة بيانات، لا أي API خارجي) — الكود المكتوب هنا إنتاجي حقيقي وليس Prototype، لكن
تشغيله الفعلي (migrations، seed، build كامل، تشغيل السيرفر) يحدث فقط في بيئة متصلة
بالإنترنت مثل Render، كما هو موضّح أعلاه.
