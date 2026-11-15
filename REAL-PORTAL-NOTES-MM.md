# PROOF Accountability Portal — Real Portal Build

ဤ package သည် 100 Feature Demo မဟုတ်ပါ။ အဓိက PROOF Accountability Portal ကို server-side application အဖြစ် အသုံးပြုရန် ပြင်ဆင်ထားသည်။ `/demo` page နှင့် 100 Feature Demo UI မပါဝင်ပါ။

## အဓိက လုပ်ဆောင်ချက်များ
- အများပြည်သူက အမည်မဖော်ဘဲ report တင်နိုင်ခြင်း
- Evidence ဖိုင်များကို server ပေါ်တွင် encrypt လုပ်သိမ်းဆည်းခြင်း
- Case ID ဖြင့် status စစ်ဆေးခြင်းနှင့် QR link ရယူခြင်း
- Public records နှင့် FAQ ကြည့်ရှုခြင်း
- Admin login နှင့် role-based staff accounts
- Admin က case စိစစ်ခြင်း၊ status ပြောင်းခြင်း၊ comment/evidence စီမံခြင်း
- Audit trail, dashboard statistics, CSV/PDF export
- Hotline, category routing, session timeout စီမံခြင်း

## အရေးကြီးသော Production လိုအပ်ချက်များ
ဤ package ကို deploy လုပ်ခြင်းတစ်ခုတည်းဖြင့် အများပြည်သူအသုံးပြုနိုင်သော live service ဖြစ်မသွားပါ။ Fly.io account/app, deploy token, persistent volume, encryption secret, admin password နှင့် domain/HTTPS စီစဉ်မှု လိုအပ်သည်။ Email notification နှင့် password reset email အတွက် SMTP credentials လိုအပ်သည်။ Telegram/Viber/SMS integration များသည် သီးခြား provider setup မရှိသေးပါ။

Fly.io Free Trial သည် အမြဲတမ်း free hosting မဟုတ်ပါ။ Trial ကုန်ဆုံးမှုနှင့် volume/compute charges ကို deploy မလုပ်မီ စစ်ဆေးပါ။

## စမ်းသပ်မှု
Package ထုတ်လုပ်စဉ် automated test suite ကို run လုပ်ခဲ့သည်။ `6 passed` ဖြစ်သည်။ ၎င်းသည် automated tests အောင်မြင်ကြောင်းသာ ဆိုလိုပြီး public production security audit သို့မဟုတ် live email delivery စမ်းသပ်ပြီးကြောင်း မဆိုလိုပါ။
