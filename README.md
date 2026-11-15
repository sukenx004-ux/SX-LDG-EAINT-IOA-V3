# PROOF Accountability Portal — Server Edition

သင်ရွေးချယ်ထားသော Feature နံပါတ် **1–16 နှင့် 26–41 (စုစုပေါင်း 32 ခု)** ကိုသာ ထည့်သွင်းထားပြီး 17–25 ကို မထည့်ထားပါ။ လက်ရှိဒီဇိုင်းကို အခြေခံထားသည်။

ဤဗားရှင်းသည် browser demo မဟုတ်ပါ။ FastAPI backend + PostgreSQL/SQLite database + encrypted evidence storage ပါဝင်သော self-hosted web application ဖြစ်သည်။ Browser တစ်ခုတည်းရှိ Local Storage ကို Case Database အဖြစ် မသုံးတော့ပါ။

## ပါဝင်သော Feature များ

| နံပါတ် | လုပ်ဆောင်ချက် | အလုပ်လုပ်ပုံ |
|---|---|---|
| 1 | Anonymous / confidential report | အမည်မတောင်းဘဲတင်နိုင်၊ report content/contact ကို application encryption ဖြင့် သိမ်း |
| 2 | Emergency / email notifications | Hotline ခေါ်ဆိုမှု၊ optional submitter email၊ SMTP admin alert |
| 3 | Evidence edit/delete | Admin က file အသစ်ထည့်၊ ဖိုင်ဖျက်၊ original/preview ကို ရယူနိုင် |
| 4 | Category/date filters | Admin list တွင် category, status, date range, keyword filter |
| 5, 28 | Dark/Light theme | Browser preference ဖြင့် ပြောင်းလဲသိမ်း |
| 6 | PDF export | Admin-authenticated PDF export |
| 7 | Status alerts | In-app admin notifications၊ email opt-in submitter alert၊ Case status lookup |
| 8 | Public/Private | Verified + Public Summary ရှိမှသာ public ထုတ်ပြန်နိုင် |
| 9 | FAQ | Public FAQ page |
| 10 | Cloud server/API | FastAPI REST API; PostgreSQL production / SQLite development |
| 11 | Case comments | Submitter comments, internal/public admin comments |
| 12 | Panic button | Local draft ဖျက်၊ page မှ ချက်ချင်းထွက်; server report မဖျက် |
| 13 | Multi-language | မြန်မာ/English switch; navigation, common UI labels, FAQ နှင့် templates; user-entered content သည် မူရင်းဘာသာဖြင့်သာ ပြမည် |
| 14 | File compression | Image/video preview ကို re-encode/compress; evidence integrity အတွက် original ကို encrypted အတိုင်းထိန်းသိမ်း (upload bandwidth ကို မလျှော့ပါ) |
| 15 | Multiple files | Image/PDF/TXT/MP4/WebM, configurable max count/size |
| 16 | Voice-to-Text | Browser Speech Recognition ပံ့ပိုးပါက အသုံးပြုနိုင် |
| 26 | Admin password recovery | SMTP ချိတ်ထားလျှင် one-time email link; SMTP မရှိလျှင် server CLI reset |
| 27 | Offline Mode | App shell cache + offline draft save/restore; final submit ကို Internet ရှိမှသာ server သို့ပို့ |
| 29 | Automatic forwarding | Category routing email ကို admin settings မှ သတ်မှတ်၊ SMTP ဖြင့် forward |
| 30 | Auto moderation | Honeypot, in-memory rate limit, duplicate/link heuristics; suspicious report ကို Flag လုပ်၍ human review |
| 31 | IP/location privacy | App က IP/Device Location ကို database ထဲ မသိမ်း၊ browser geolocation မတောင်း၊ image preview မှ EXIF/GPS metadata ဖယ် |
| 32 | QR Code | Case ID lookup link QR |
| 33 | Audio feedback | Submit/status အောင်မြင်မှုကို browser speech synthesis ဖြင့် ဖတ်ပြ |
| 34 | Audit trail | Status, visibility, comments, files, export, login စသည့် admin actions မှတ်တမ်း |
| 35 | Hotline buttons | Admin သတ်မှတ်ထားသော ဖုန်းနံပါတ်များကို tap-to-call |
| 36 | Templates | Report form အတွက် အသင့်သုံး templates |
| 37 | Dashboard charts | Status/category/month charts |
| 38 | Tags | Case တစ်ခုချင်းစီတွင် tags ထည့်/ပြင် |
| 39 | Watermark | Image/PDF/video အတွက် watermarked preview ထုတ်; original evidence နှင့် original filename ကို encrypted သိမ်း |
| 40 | Feedback form | Public feedback + admin review |
| 41 | Session timeout | Server-side idle timeout + browser auto logout; default 15 minutes |

## Local development (Windows / macOS / Linux)

Python 3.11+ လိုအပ်သည်။

```bash
cd PROOF-Portal
python -m venv .venv
# Windows: .venv\\Scripts\\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
python -m app.cli generate-secrets
python -m app.cli create-admin --username admin --email admin@example.org
uvicorn app.main:app --host 127.0.0.1 --port 8000 --no-access-log
```

Browser တွင် `http://127.0.0.1:8000` ဖွင့်ပါ။ Admin login အတွက် create-admin အဆင့်တွင် သင်သတ်မှတ်ထားသော username/password ကို အသုံးပြုပါ။ Default password မပါဝင်ပါ။

## Render Hosting (Android Chrome မှ အသုံးပြုရန်)

Render Blueprint နှင့် မြန်မာလို အဆင့်လိုက်လမ်းညွှန်ကို `render.yaml` နှင့် `README-RENDER-MM.md` တွင် ထည့်ထားသည်။ Evidence ဖိုင်များ မပျောက်စေရန် Paid Web Service + Paid PostgreSQL + Persistent Disk လိုအပ်သည်။ Free plan ကို Production data အတွက် မသုံးပါနှင့်။

## Production deployment (Docker + PostgreSQL)

1. Server/VPS တစ်ခုတွင် Docker + Docker Compose တပ်ဆင်ပါ။ Domain ၏ DNS A/AAAA record ကို VPS IP သို့ ချိတ်ပြီး inbound port 80/443 ဖွင့်ပါ။ ပါဝင်ပြီးသား Caddy သည် HTTPS certificate ကို အလိုအလျောက် ရယူပါမည်။
2. `.env.example` ကို `.env` အဖြစ်ကူးပြီး အောက်ပါအချက်များ ဖြည့်ပါ။
   - `POSTGRES_PASSWORD`: ခန့်မှန်းရခက်ပြီး URL-safe (A–Z, a–z, 0–9, `_`, `-`) ဖြစ်သော database password
   - `PROOF_ENCRYPTION_KEY`: `python -c "from cryptography.fernet import Fernet; print(Fernet.generate_key().decode())"` ဖြင့် ထုတ်ပါ
   - `DOMAIN`: `your-domain.example`
   - `APP_BASE_URL`: `https://your-domain.example`
   - SMTP settings: Email notification/password recovery လိုအပ်ပါက ဖြည့်ပါ
3. Compose file သည် `POSTGRES_PASSWORD` ကိုလည်း လိုအပ်သဖြင့် `.env` ထဲတွင် သီးခြားထည့်ပါ။
4. Run:

```bash
docker compose up -d --build
docker compose exec web python -m app.cli create-admin --username admin --email admin@your-domain.example
```

5. Caddy သည် `web:8000` သို့ reverse proxy လုပ်ပြီး HTTPS ကို အလိုအလျောက် ဖွင့်ပေးပါမည်။ Uvicorn access log ကို ပိတ်ထားသည်။ Caddy/Hosting provider logs ထဲတွင် client IP မသိမ်းစေရန် provider policy ကိုလည်း သီးခြားစစ်ဆေးပါ။

## Email Routing

Admin Login → System Settings → Category Email Routing ထဲတွင် JSON ထည့်ပါ။ ဥပမာ:

```json
{
  "အဂတိလိုက်စားမှု / လိမ်လည်မှု": "integrity-team@example.org",
  "လုံခြုံရေး စည်းမျဉ်းဖောက်ဖျက်မှု": "safety-team@example.org",
  "default": "admin@example.org"
}
```

SMTP မချိတ်ထားပါက database/in-app notifications အလုပ်လုပ်မည်၊ Email forward/recovery မပို့နိုင်ပါ။

## Staff accounts

Admin login ဝင်ပြီး Admin Dashboard ၏ Staff Access Form မှ staff ဖန်တီးနိုင်သည်။ API `POST /api/admin/users` လည်း အသုံးပြုနိုင်သည်။ Role များမှာ `admin`, `reviewer`, `publisher` ဖြစ်သည်။ Reviewer သည် report/evidence ကို စိစစ်ပြီး internal note ရေးနိုင်သည်။ Publisher သည် contact email, original evidence နှင့် internal comments မမြင်ဘဲ public summary/visibility ကိုသာ ထိန်းချုပ်နိုင်သည်။ Admin သည် staff/settings အားလုံးကို စီမံနိုင်သည်။ ပထမ Admin ကို CLI ဖြင့် ဖန်တီးပါ။

Password ကို email link ဖြင့် reset လုပ်ရန် SMTP ကို configure လုပ်ထားရမည်။ SMTP မရှိလျှင် server shell မှ:

```bash
docker compose exec web python -m app.cli reset-password --username admin
```

## Data / Security notes — မဖြစ်မနေဖတ်ရန်

- Report body, contact email, comments, feedback, evidence files နှင့် original filenames များကို application encryption ဖြင့် သိမ်းဆည်းသည်။ `PROOF_ENCRYPTION_KEY` ပျောက်သွားပါက ထို encrypted data များကို ပြန်ဖတ်မရနိုင်ပါ။ Key ကို password manager/offline secure backup ထဲတွင် သိမ်းပါ။ Key ကို database backup နှင့်အတူ တစ်နေရာတည်း မထားပါနှင့်။
- Database နှင့် `/app/data` evidence volume ကို သီးခြား encrypted backup လုပ်ပါ။ Restore လုပ်ရန် encryption key လိုအပ်သည်။
- Application သည် IP address သို့မဟုတ် GPS location ကို database ထဲ မသိမ်းပါ။ Rate limiting အတွက် request host ကို memory ထဲ ယာယီအသုံးပြုပြီး persist မလုပ်ပါ။ သို့သော် Hosting Provider, CDN, reverse proxy, ISP သို့မဟုတ် browser က IP/network metadata ကို log လုပ်နိုင်သည်။ အမည်မဖော်မှုကို လုံးဝအာမမခံပါ။
- Original evidence ကို မပြောင်းလဲဘဲ encrypt လုပ်သိမ်းထားသည်။ Watermark/metadata-stripped/compressed files များသည် preview derivatives ဖြစ်သည်။ Original download ကို Audit Log တွင် မှတ်တမ်းတင်သည်။
- Offline mode သည် စာသား/ဖောင်အချက်အလက် draft ကိုသာ local browser ထဲ သိမ်းသည်။ Evidence file များကို offline draft နှင့်အတူ မသိမ်းပါ။ Offline ဖြစ်နေချိန်တွင် report ကို server သို့ မပို့နိုင်ပါ။ Shared device တွင် draft မထားပါနှင့်။
- Auto moderation သည် spam heuristic သာဖြစ်ပြီး AI/human investigation အစားမထိုးပါ။ Flagged report များကို staff က စိစစ်ရမည်။
- ဤ project ကို public internet ပေါ် မတင်မီ threat model, legal/privacy review, backup restore test, mail delivery test, malware scanning strategy, penetration test, admin access review တို့ လုပ်ဆောင်ပါ။
- လက်ရှိ deployment package သည် server software ကို ပေးထားခြင်းဖြစ်သည်။ OpenAI/ChatGPT မှ သင့်အတွက် domain, hosting account, SMTP, TLS certificate သို့မဟုတ် cloud database ကို အလိုအလျောက် provision မလုပ်ထားပါ။ Server ပေါ် deploy လုပ်ပြီးမှ အများသုံးအဖြစ် တကယ်အသုံးပြုနိုင်မည်။

## Automated tests

```bash
python -m pytest -q
```

## API

- Public: `/api/health`, `/api/public/config`, `/api/public/stats`, `/api/public/records`, `/api/reports`, `/api/cases/{case_id}`
- Admin: `/api/admin/reports`, `/api/admin/stats`, `/api/admin/audit`, `/api/admin/export.csv`, `/api/admin/export.pdf`, `/api/admin/settings`
- Development API docs: `/api/docs`

Production mode တွင် interactive API docs ပိတ်ထားသည်။
