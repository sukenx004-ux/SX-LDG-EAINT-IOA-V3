# PROOF PORTAL V3 — Fly.io (Free Trial) လမ်းညွှန်

ဤ package သည် အရင် Render-ready Server Edition ကို Fly.io အတွက် ပြင်ဆင်ထားခြင်းဖြစ်သည်။ Android Chrome မှ ဝင်သုံးနိုင်ရန် Fly.io တွင် Deploy လုပ်မည်။ Termux မလိုပါ။ GitHub Actions ကို deploy runner အဖြစ် အသုံးပြုထားသည်။

## အရေးကြီး — အမြဲတမ်း Free မဟုတ်ပါ

Fly.io တွင် permanent free tier မရှိပါ။ တရားဝင် Free Trial သည် 2 machine hours သို့မဟုတ် 7 days (အရင်ပြည့်သည့်အရာ) ဖြစ်ပြီး limit ပြည့်ပါက payment method မထည့်မချင်း app ရပ်သွားမည်။ Persistent Volume ကို စက်ရပ်ထားသော်လည်း billed ဖြစ်နိုင်သည်။ 1 GB volume သည် လက်ရှိစာရင်းအရ $0.15/GB/month ဖြစ်သည်။ Trial ကုန်ပြီးနောက် အမြဲတမ်းအခမဲ့ website အဖြစ် ဆက်မလည်နိုင်ပါ။

ထို့ကြောင့် ဤ setup ကို **စမ်းသပ်ရန်သာ** သုံးပါ။ အမှန်တကယ် ထိခိုက်နစ်နာသူများ၏ တိုင်ကြားချက်၊ ကိုယ်ရေးအချက်အလက်နှင့် Evidence များကို Production အဖြစ် မသုံးပါနှင့်။ Backup မရှိသောအခြေအနေတွင် အရေးကြီး Evidence မတင်ပါနှင့်။

## ပါဝင်သော Fly ပြင်ဆင်မှု

- `fly.toml` — Singapore (`sin`), HTTPS, health check, one 512 MB shared VM
- SQLite database ကို `/data/proof.db` သို့ထားပြီး Fly Volume တွင် သိမ်းမည်
- Evidence နှင့် local encryption key path ကို `/data` တွင်ထားမည်
- Idle ဖြစ်လျှင် Machine stop၊ request ရှိမှ auto-start (ပထမ request အနည်းငယ်နှေးနိုင်သည်)
- GitHub Actions workflow ဖြင့် ZIP ကို extract လုပ်ပြီး Fly deploy လုပ်မည်
- ပထမဆုံး deploy တွင် `PROOF_ENCRYPTION_KEY` ကို တစ်ကြိမ်ဖန်တီးပြီး Fly secret အဖြစ် သိမ်းမည်
- `INITIAL_ADMIN_PASSWORD` GitHub Secret ပေးထားလျှင် ပထမဆုံး boot တွင် admin `admin` ကို ဖန်တီးပြီး Fly secret ကို ဖယ်ရှားမည်

## Android Chrome မှ Deploy လုပ်ရန်

### 1. Fly.io account ဖွင့်ပါ

1. Chrome ဖြင့် https://fly.io/app/sign-up ကိုဖွင့်ပါ။
2. Account ဖွင့်ပြီး Dashboard ဝင်ပါ။
3. Free trial limit နှင့် billing ကို ဖတ်ပြီးမှ ဆက်လုပ်ပါ။ Payment method ထည့်ပါက Trial အဆုံးသတ်ပြီး usage billing စတင်နိုင်သည်။
4. Dashboard မှ app အသစ်ဖန်တီးပါ။ App name သည် ကမ္ဘာတစ်ဝန်း unique ဖြစ်ရမည်။ ဥပမာ `proof-portal-v3-mm-xxxx`။ Region ရွေးနိုင်ပါက Singapore (`sin`) ရွေးပါ။
5. App page > Tokens မှ အဲဒီ App အတွက် Deploy Token ဖန်တီးပြီး Copy လုပ်ပါ။ Token ကို မည်သူ့ကိုမျှ မပေးပါနှင့်။

### 2. GitHub repository ဖန်တီးပါ

1. https://github.com/new ကိုဖွင့်ပါ။
2. Repository name ကို `proof-portal-v3-fly` လို့ပေးပါ။ Private ရွေးပါ။
3. Create repository နှိပ်ပါ။
4. `PROOF-PORTAL-V3-FLY-TRIAL.zip` ကို repository ထဲ Upload files ဖြင့် တင်ပြီး Commit လုပ်ပါ။

### 3. GitHub Actions workflow ဖန်တီးပါ

1. Repository ထဲတွင် **Add file → Create new file** ကိုနှိပ်ပါ။
2. File name အဖြစ် `.github/workflows/fly-deploy.yml` ထည့်ပါ။
3. ဒီ ZIP ထဲက `fly-deploy.yml.example` ကို Text Editor ဖြင့်ဖွင့်ပြီး အကြောင်းအရာအပြည့်အစုံ Copy/Paste လုပ်ပါ။
4. Workflow ထဲက `APP_NAME: proof-portal-v3-change-this` ကို ကိုယ်ပိုင် unique app name ဖြင့် ပြောင်းပါ (ဥပမာ `proof-portal-mm-2026-x7k9`)။
5. `FLY_ORG: change-this-org-slug` ကို Dashboard URL ထဲက Organization slug အတိအကျဖြင့် ပြောင်းပါ။
6. Commit changes လုပ်ပါ။ Workflow သည် Fly app ကို လိုအပ်ပါက ဖန်တီးပြီး Singapore Volume ကို ပြင်ဆင်ကာ Deploy လုပ်မည်။

### 4. GitHub Secrets ထည့်ပါ

Repository → **Settings → Secrets and variables → Actions → New repository secret** သို့သွားပါ။

Secret ၂ ခု ထည့်ပါ။

- `FLY_API_TOKEN` — ပထမဆုံး deploy အတွက် Fly Dashboard ရှိ Organization > Tokens မှ ရယူထားသော Org-scoped deploy token အပြည့်အစုံ
- `INITIAL_ADMIN_PASSWORD` — Admin အတွက် ကိုယ်ပိုင် password (အနည်းဆုံး 12 characters)။ GitHub ထဲတွင် Secret အဖြစ်သာ ထည့်ပါ။ Chat, code သို့မဟုတ် ZIP ထဲ မရေးပါနှင့်။

### 5. Deploy လုပ်ပါ

1. GitHub repository → Actions ကိုဖွင့်ပါ။
2. `Deploy PROOF Portal to Fly.io` workflow ကိုရွေးပါ။
3. Run workflow ကိုနှိပ်ပါ။
4. အောင်မြင်လျှင် GitHub Actions log အဆုံးတွင် deployment success ကို မြင်ရမည်။
5. ပထမဆုံး deploy အောင်မြင်ပြီးနောက် Fly Dashboard > App > Tokens မှ **App-scoped deploy token** အသစ်တစ်ခု ဖန်တီးပါ။ GitHub `FLY_API_TOKEN` Secret ကို ထို token အသစ်ဖြင့် အစားထိုးပြီး Org-scoped token ကို revoke လုပ်ပါ။
6. GitHub Settings → Secrets and variables → Actions မှ `INITIAL_ADMIN_PASSWORD` Secret ကို ကိုယ်တိုင် Delete လုပ်ပါ။ Fly ဘက်ရှိ one-time bootstrap secret ကို workflow က ဖယ်ရှားပေးသည်။

### 6. Chrome မှ Website ဖွင့်ပါ

`https://YOUR-FLY-APP-NAME.fly.dev`

`YOUR-FLY-APP-NAME` ကို သင့် Fly app name ဖြင့် အစားထိုးပါ။ Admin username သည် `admin` ဖြစ်ပြီး password သည် GitHub Secret ထဲ သတ်မှတ်ခဲ့သော password ဖြစ်သည်။

## Data မပျောက်စေရန် သတိပြုရန်

- Database နှင့် Evidence များသည် `proof_data` Volume ပေါ်တွင် ရှိသည်။ App ကို destroy လုပ်ခြင်း၊ Volume ကို delete လုပ်ခြင်း၊ Volume ကို detach လုပ်ခြင်း မလုပ်ပါနှင့်။
- SQLite သုံးထားသောကြောင့် Machine တစ်လုံးတည်းသာ အသုံးပြုပါ။ Scale count တိုးမထားပါနှင့်။
- Fly Volume သည် Machine တစ်လုံးနှင့် Region တစ်ခုအတွက် local storage ဖြစ်သည်။ Snapshot ကို အပြင်ဘက် backup အဖြစ် အစားမထိုးပါနှင့်။ သီးခြား encrypted backup လုပ်ပါ။
- `PROOF_ENCRYPTION_KEY` ကို Fly secret ထဲတွင် ထားပါ။ Secret ကို ဖျက်/ပြောင်းလဲလျှင် ရှိပြီးသား encrypted Evidence များကို ပြန်ဖတ်မရနိုင်ပါ။
- Fly trial ကုန်လျှင် Machine ရပ်သွားမည်။ Website 24/7 လိုအပ်ပါက paid plan သို့မဟုတ် အမြဲတမ်း free အဆင့်ရှိသည့် အခြား hosting ကို ရွေးပါ။

## Troubleshooting

- `App not found`: workflow ထဲက `APP_NAME` နဲ့ Fly Dashboard app name တူ/မတူ စစ်ပါ။
- `Unauthorized`: ပထမဆုံး deploy အတွက် Org-scoped deploy token အပြည့်အစုံ ထည့်ထားကြောင်း၊ နောက်ပိုင်း deploy များအတွက် App-scoped deploy token အသုံးပြုထားကြောင်း စစ်ပါ။
- Volume create error: Fly trial limit, billing status, `sin` region capacity နှင့် app token permissions စစ်ပါ။
- `Machine out of memory`: 512 MB အတွက် အလွန်ကြီးသော ဖိုင်များကို မတင်ပါနှင့်။ အမြဲတမ်း memory တိုးခြင်းသည် ကုန်ကျစရိတ် တိုးစေသည်။
- `Encryption key mismatch`: `PROOF_ENCRYPTION_KEY` ကို ပြောင်းမထားပါနှင့်။ Fly secret ကို မဖျက်ပါနှင့်။
- Workflow failed: GitHub Actions → failed run → error log screenshot ပို့ပါ။ Token နှင့် Password များကို ဖျောက်ပါ။

## Official Links

- Fly pricing: https://fly.io/pricing/
- Fly free trial: https://fly.io/docs/about/free-trial/
- Fly volumes: https://fly.io/docs/volumes/
- Fly tokens: https://fly.io/docs/security/tokens/
- Fly GitHub Actions: https://fly.io/docs/launch/continuous-deployment-with-github-actions/

## V3.1 အရေးကြီး Security / Backup ပြင်ဆင်ချက်များ

- Login, report submit, feedback, password recovery စသည့် rate limit များကို DB တွင် မှတ်တမ်းတင်သည်။ Server restart ပြီးလည်း limit window မပျောက်ပါ။ IP အစစ်ကို မသိမ်းဘဲ SHA-256 digest သာသိမ်းသည်။
- Admin account တစ်ခုလျှင် login session တစ်ခုသာ အသက်ဝင်စေသည်။ Password reset ပြုလုပ်လျှင် ထို account ၏ session အားလုံးနှင့် အခြား recovery token များကို ပိတ်သည်။
- Evidence encrypted file များကို temporary file မှ atomic rename ဖြင့် သိမ်းသည်။
- `/api/health` သည် DB နှင့် data storage နှစ်ခုလုံးကို စစ်ဆေးသည်။
- Additive schema migration ledger ကို `schema_migrations` တွင် မှတ်တမ်းတင်သည်။ ဒီ release သည် table အသစ်များကိုသာ safe create လုပ်သည်။ Existing column/type ပြောင်းလဲမှုအတွက် သီးသန့် migration မထည့်ဘဲ ကိုယ်တိုင် DB schema မပြင်ပါနှင့်။

### Encrypted Backup / Restore (SQLite deployment)

Web service ကို run နေစဉ် SQLite online backup API ဖြင့် backup ယူနိုင်သည်။ Production တွင် CLI ကို container အတွင်း run ပါ။

```bash
python -m app.cli backup --output /data/proof-backup.proofbak
```

Restore မလုပ်မီ Web service ကို ရပ်ပါ။ Backup ကိုဖန်တီးခဲ့သော **တူညီသည့် `PROOF_ENCRYPTION_KEY`** ကို အသုံးပြုရမည်။ ထို key ပျောက်ပါက encrypted backup ကို decrypt မလုပ်နိုင်ပါ။

```bash
python -m app.cli restore /data/proof-backup.proofbak
```

Restore သည် SQLite integrity check အောင်မြင်မှသာ database/evidence ကို အစားထိုးသည်။ Restore ပြီးလျှင် service ပြန်ဖွင့်ပြီး `/api/health` နှင့် test Case များကို စစ်ပါ။ ဤ CLI backup သည် SQLite အတွက်သာဖြစ်ပြီး PostgreSQL deployment တွင် `pg_dump`/`pg_restore` သုံးရမည်။ Backup ဖိုင်ကို application data volume တစ်ခုတည်းတွင်သာ မထားဘဲ encrypted off-site location တစ်ခုသို့ ကူးထားပါ။

### Production မတင်မီ မဖြစ်မနေ လုပ်ရန်

- `APP_ENV=production`, strong `PROOF_ENCRYPTION_KEY`, `COOKIE_SECURE=true` သတ်မှတ်ပါ။
- SMTP host/username/password/from ကို configure လုပ်ပြီး password recovery နှင့် notification ကို စမ်းသပ်ပါ။ SMTP မချိတ်ထားလျှင် email မပို့နိုင်ပါ။
- HTTPS/host platform logs, access controls, backup restore drill နှင့် security review ကို တကယ့် deployment ပေါ်တွင် စမ်းသပ်ပါ။
- Fly.io trial သည် အမြဲတမ်း free မဟုတ်ပါ။ လက်ရှိ billing/volume cost ကို Fly Dashboard တွင် စစ်ပါ။
