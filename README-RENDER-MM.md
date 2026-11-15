# PROOF PORTAL V3 — Render Hosting လမ်းညွှန် (မြန်မာ)

ဤ package သည် `PROOF-PORTAL-SERVER-V3-1.zip` မှ ရရှိသော FastAPI Server Edition ကို Render ပေါ်တင်ရန် ပြင်ဆင်ထားခြင်းဖြစ်သည်။ မူရင်း server package တွင် API/Database ဖြင့် အလုပ်လုပ်သော Feature 32 ခု ပါဝင်သည်။ သီးခြား 100 Feature Demo Lab ကို ဤ source archive တွင် မပါဝင်သဖြင့် မထည့်ထားပါ။

## အရေးကြီး — Production အတွက် Paid resources လိုအပ်သည်

ဤ Portal သည် တိုင်ကြားချက်အချက်အလက်နှင့် Evidence ဖိုင်များ သိမ်းဆည်းသည်။ Render Free web service ၏ local filesystem သည် ephemeral ဖြစ်ပြီး Persistent Disk မရနိုင်ပါ။ Free Postgres သည် 30 ရက်အကြာ သက်တမ်းကုန်သည်။ ထို့ကြောင့် အမှန်တကယ် အသုံးပြုမည့်အခါ ဤ Blueprint က Paid Web Service (`starter`), Paid Postgres (`basic-256mb`) နှင့် Persistent Disk (1 GB) ကို Singapore region တွင် ဖန်တီးရန် သတ်မှတ်ထားသည်။ Deploy မလုပ်မီ Render Dashboard တွင် လက်ရှိဈေးနှုန်းကို စစ်ဆေးပါ။

## Deploy လုပ်ရန်

### 1. GitHub repository ဖန်တီးပါ

1. Android Chrome မှ https://github.com ကိုဖွင့်ပြီး Sign in လုပ်ပါ (အကောင့်မရှိလျှင် Create account လုပ်ပါ)။
2. New repository ကိုနှိပ်ပါ။ Repository name ကို `proof-portal-v3` ဟုထားပါ။ **Private** ကိုရွေးပါ။
3. README, .gitignore, License ကို အလိုအလျောက် မဖန်တီးပါနှင့်။ Empty repository အဖြစ် Create လုပ်ပါ။
4. ZIP ကို ဖုန်းထဲ Extract လုပ်ပြီး Project ထဲက ဖိုင်/ဖိုလ်ဒါအားလုံးကို Repository root ထဲ Upload လုပ်ပါ။ Root ထဲတွင် `Dockerfile`, `render.yaml`, `requirements.txt`, `app/`, `static/` တို့ တိုက်ရိုက်ရှိရမည်။ `PROOF-Portal/PROOF-Portal/...` ဟု Folder နှစ်ထပ် မဖြစ်စေရ။
5. Commit changes လုပ်ပါ။

### 2. Render နဲ့ GitHub ချိတ်ပါ

1. https://dashboard.render.com ကိုဖွင့်ပြီး Sign up / Log in လုပ်ပါ။
2. Render Dashboard တွင် **New → Blueprint** ကိုရွေးပါ။
3. GitHub ကို Connect လုပ်ပြီး `proof-portal-v3` repository ကိုရွေးပါ။
4. `render.yaml` ကိုတွေ့ပါက Blueprint preview တွင် အောက်ပါ resources သုံးခုကို စစ်ပါ:
   - Web Service: `proof-portal-v3` (Singapore, Starter)
   - PostgreSQL: `proof-portal-v3-db` (Singapore, basic-256mb)
   - Persistent Disk: `/var/data`, 1 GB
5. Costs ကိုဖတ်ပြီး သဘောတူမှ Apply လုပ်ပါ။ Render က Build/Deploy စတင်မည်။

### 3. Deploy အောင်မြင်မှု စစ်ပါ

1. Render Dashboard → `proof-portal-v3` Web Service → Events/Logs တွင် Deploy live ဖြစ်ကြောင်းစစ်ပါ။
2. Service ရဲ့ `https://....onrender.com` URL ကို Chrome မှာဖွင့်ပါ။
3. `https://....onrender.com/api/health` ကိုဖွင့်ပါ။ JSON ထဲတွင် `"status":"ok"` ဖြစ်ရမည်။
4. `APP_BASE_URL` သည် `RENDER_EXTERNAL_URL` ကို အလိုအလျောက်ယူထားသည်။ QR link နှင့် password reset link အတွက် အသုံးပြုသည်။

### 4. Admin account ဖန်တီးပါ

Render Web Service သည် Paid plan ဖြစ်သောကြောင့် Dashboard Shell ကို အသုံးပြုနိုင်သည်။

1. Render Dashboard → `proof-portal-v3` → **Shell** ကိုဖွင့်ပါ။
2. အောက်ပါ command ကို Run ပါ:

```bash
python -m app.cli create-admin --username admin --email your-email@example.com
```

3. Password ကို prompt မှာ သတ်မှတ်ပါ။ အနည်းဆုံး 12 characters လိုအပ်သည်။ Password ကို ChatGPT သို့မဟုတ် အခြားသူထံ မပို့ပါနှင့်။
4. Chrome မှ Portal URL ကိုဖွင့်ပြီး Admin Login ဝင်ပါ။

## မဖြစ်မနေ လုံခြုံရေးစစ်ဆေးရန်

- Render Environment ထဲက `PROOF_ENCRYPTION_KEY` ကို အလိုအလျောက် generate လုပ်ထားသည်။ **ဒီ Key ကို မဖျက်၊ မပြောင်းပါနှင့်။** Key ပျောက်လျှင် encrypted report/evidence ကို ပြန်ဖတ်မရနိုင်ပါ။ လုံခြုံသော Password Manager/offline location တွင် backup ထားပါ။
- `DATABASE_URL` ကို Render Postgres မှ internal connection string အဖြစ် ချိတ်ထားသည်။ Database ကို public internet မှ မဖွင့်ထားပါ။
- Evidence ဖိုင်များသည် `/var/data` Persistent Disk ထဲတွင် သိမ်းသည်။ Disk ကို detach/delete မလုပ်ပါနှင့်။
- SMTP မချိတ်ထားလျှင် Email notification နှင့် email password recovery မလုပ်နိုင်ပါ။ Admin password ပြန်သတ်မှတ်ရန် Render Shell မှ `python -m app.cli reset-password --username admin` ကိုသုံးပါ။
- Hosting provider/CDN/browser က network metadata ကို log လုပ်နိုင်သည်။ Anonymous submission သည် လုံးဝအမည်မသိဖြစ်ကြောင်း အာမမခံနိုင်ပါ။
- Production မသုံးမီ backup/restore စမ်းသပ်ခြင်း၊ staff permissions စစ်ဆေးခြင်း၊ privacy/legal/security review နှင့် malware scanning policy ပြင်ဆင်ပါ။
- Real reports မတင်မီ test data ဖြင့် report တင်ခြင်း၊ Case ID lookup, QR, Admin login, Evidence upload/download, PDF/CSV export, persistence after redeploy စမ်းပါ။

## Troubleshooting

- **Build failed:** Render Logs တွင် Docker build error ကိုဖတ်ပါ။ `Dockerfile`, `requirements.txt` နှင့် `app/`, `static/` root path မှန်/မမှန်စစ်ပါ။
- **Health check failed:** `/api/health` သည် 200 ပြန်ပေးရမည်။ Logs တွင် database connection error သို့မဟုတ် missing `PROOF_ENCRYPTION_KEY` ရှိ/မရှိစစ်ပါ။
- **Evidence ပျောက်သွားသည်:** `/var/data` တွင် Persistent Disk attach ဖြစ်/မဖြစ် စစ်ပါ။ Free service မသုံးပါနှင့်။
- **Admin Shell မပေါ်:** Free web service တွင် Shell မရပါ။ Paid Web Service လိုအပ်သည်။

## အရင်းအမြစ်များ

- Render Docker deploy: https://render.com/docs/docker
- Render Blueprints: https://render.com/docs/blueprint-spec
- Render Persistent Disks: https://render.com/docs/disks
- Render Free limits: https://render.com/docs/free
- Render Shell access: https://render.com/docs/ssh
