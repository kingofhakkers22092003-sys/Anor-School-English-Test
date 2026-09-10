# Anor School — AI baholash serveri

Bu papka saytingizning to'liq nusxasi + Yozish va Gapirish bo'limlarini OpenAI orqali
avtomatik baholaydigan kichik lokal server (Node.js). Internet kerak emas — faqat
maktabingizdagi asosiy kompyuterda ishga tushirasiz, boshqa o'quvchilar shu kompyuterga
tarmoq (Wi-Fi/LAN) orqali ulanishadi. Faqat AI so'rovlari uchun internet aloqasi kerak
bo'ladi (OpenAI serverlariga), sayt esa to'liq lokal ishlaydi.

## 1. Node.js o'rnatish

Agar hali o'rnatilmagan bo'lsa, https://nodejs.org saytidan **LTS** versiyasini
yuklab, asosiy kompyuterga o'rnating (bir marta qilinadi).

## 2. Fayllarni joylashtirish

Ushbu papkani (`anor-server`) asosiy kompyuterga istalgan joyga nusxalang, masalan
Ish stoliga.

## 3. Kerakli kutubxonalarni o'rnatish

Terminal (Windows'da PowerShell yoki CMD) ochib, shu papkaga o'ting:

```
cd Desktop/anor-server
npm install
```

Bu bir marta bajariladi (internet kerak). Shundan keyin `node_modules` papkasi paydo bo'ladi.

## 4. OpenAI API kalitini sozlash

1. https://platform.openai.com saytida hisob oching va API kalit (API key) oling.
   Bu kalit pullik — har bir AI so'rovi uchun ozgina to'lov olinadi (gpt-4o-mini va
   whisper-1 modellari juda arzon, oddiy matn/audio uchun bir necha sentdan kam).
2. `.env.example` faylini nusxalab, nomini `.env` ga o'zgartiring.
3. `.env` faylini oching va quyidagicha to'ldiring:

```
OPENAI_API_KEY=sk-...sizning-haqiqiy-kalitingiz...
PORT=3000
```

**Diqqat:** `.env` faylini hech kimga bermang va internetga (masalan GitHub'ga) yuklamang —
bu sizning shaxsiy kalitingiz, undan boshqalar sizning hisobingiz hisobidan foydalanishi mumkin.

## 5. Serverni ishga tushirish

Shu papkada:

```
npm start
```

Agar hammasi to'g'ri bo'lsa, konsolda shunga o'xshash yozuv chiqadi:

```
Server ishga tushdi: http://localhost:3000
Maktabdagi boshqa kompyuterlar shu tarmoqdagi IP orqali ulanadi, masalan: http://192.168.1.XX:3000
```

## 6. O'quvchilar qanday ulanadi

1. Asosiy kompyuterning lokal tarmoq (Wi-Fi/LAN) IP manzilini toping:
   - Windows: CMD'da `ipconfig` yozing, "IPv4 Address" qatoriga qarang (masalan `192.168.1.15`).
   - Mac/Linux: terminalda `ifconfig` yoki `ip addr`.
2. O'quvchilar o'z qurilmalari (kompyuter/telefon) brauzerida shu manzilni kiritadi:
   `http://192.168.1.15:3000` (raqamlar o'zingiznikiga mos bo'lishi kerak).
3. Barcha qurilmalar bitta Wi-Fi/tarmoqqa ulangan bo'lishi shart.
4. Agar ulanmasa, asosiy kompyuterning xavfsizlik devori (firewall) 3000-portni
   bloklayotgan bo'lishi mumkin — uni ruxsat bering yoki `.env` faylida `PORT`ni
   boshqasiga o'zgartiring.

## Nima o'zgardi?

- Yozish bo'limi: o'quvchi javoblarini yuborganda, matn `/api/grade-writing`
  manziliga yuboriladi, u yerda ChatGPT modeli grammatika va mazmunga qarab
  0–10 ball va qisqa izoh beradi. Natija darhol o'quvchi profiliga saqlanadi.
- Gapirish bo'limi: ovoz yozuvi `/api/grade-speaking` manziliga yuboriladi,
  avval Whisper modeli uni matnga o'giradi, so'ng ChatGPT modeli shu matn
  asosida baholaydi.
- Agar internet uzilib qolsa yoki API kaliti noto'g'ri bo'lsa, tizim xatolik
  haqida xabar beradi va javobni "o'qituvchi tekshiradi" holatida saqlaydi —
  hech qanday javob yo'qolmaydi.
- O'qituvchi panelidagi ballar ustiga sichqonchani olib borsangiz, AI izohini
  ko'rasiz (agar mavjud bo'lsa).

## Yangi: Ustoz tekshiruvi rejimi

Admin panelida ("O'quvchilar natijalari" bo'limi ustida) endi **Baholash rejimi**
tugmalari bor: **AI tekshiruvi** yoki **Ustoz tekshiruvi**.

- **AI tekshiruvi** — avvalgidek, Yozish va Gapirish javoblari OpenAI orqali
  avtomatik baholanadi.
- **Ustoz tekshiruvi** — o'quvchi javob yuborganda, u avtomatik baholanmaydi,
  balki admin panelidagi yangi **"Tekshirish"** bo'limiga tushadi. U yerda:
  - Yozish uchun — o'quvchining har bir topshiriqqa yozgan matni to'liq ko'rinadi.
  - Gapirish uchun — o'quvchining ovoz yozuvi audio pleer sifatida ko'rinadi,
    uni tinglab turib baholash mumkin.
  - Ustoz 0–10 ball va (ixtiyoriy) qisqa izoh kiritib, "Baholashni saqlash"ni bosadi.
  - Barcha 5 bo'lim baholangach, "O'quvchilar natijalari" bo'limida shu
    o'quvchi qatorida **"PDF yuklab olish"** tugmasi paydo bo'ladi — natijani
    to'g'ridan-to'g'ri shu yerdan yuklab olish mumkin.
- Rejimni istalgan vaqt almashtirish mumkin; bu faqat kelajakdagi topshiriqlarga
  ta'sir qiladi, allaqachon yuborilgan/baholangan javoblarga tegmaydi.
- **Eslatma:** ovoz yozuvlari brauzer xotirasida (localStorage) saqlanadi. Agar
  juda ko'p o'quvchi uzoq audio yozsa, xotira chegarasiga yetish ehtimoli bor —
  odatiy sinf hajmi uchun (bir necha o'nlab o'quvchi) muammo bo'lmaydi.

## Yangi: Telegramga avtomatik yuborish va natijalarni yopish qulfi

Endi Telegramga yuborish **avtomatik** ishlaydi — alohida tugma bosish shart
emas. O'quvchining barcha 5 bo'limi baholangan zahoti (ustoz "Baholashni
saqlash" ni bosgan yoki AI rejimida baholash tugagan zahoti), server
o'zi PDF natijani tayyorlab, sozlangan Telegram guruhga yuboradi.

- O'qituvchi panelida "O'quvchilar natijalari" bo'limida har bir o'quvchi
  qatorida holat ko'rinadi: **"Telegramga yuborilmoqda…"** (hali
  yuborilmagan yoki xatolik bo'lgan) yoki **"Telegramga yuborildi ✓"**.
- Agar internet vaqtincha uzilib qolsa yoki `.env` da token noto'g'ri
  bo'lsa, tizim xato holatini saqlaydi va keyingi safar shu o'quvchining
  ma'lumoti so'ralganda (masalan sahifa yangilanganda) avtomatik qayta
  urinib ko'radi — qo'lda hech narsa qilish shart emas.
- **Muhim:** o'quvchi profilidagi natijalar oynasidagi **×** (yopish)
  tugmasi endi faqat quyidagi ikkala shart bajarilgach ishlaydi:
  1. Yozish va Gapirish bo'limlari ustoz (yoki AI) tomonidan baholangan
     (endi "Tekshiruvda" holatida emas), **va**
  2. Natija Telegram guruhga muvaffaqiyatli yuborilgan.
  Shu ikkalasi bajarilmaguncha × tugmasi kulrang va bosilmaydigan holatda
  turadi, shunda natija hech qachon ustoz ko'rmasdan yoki Telegramga
  yetib bormasdan yo'qolib qolmaydi.

Buning uchun bitta marta quyidagilarni sozlash kerak:

### 1. Bot yarating (agar hali yo'q bo'lsa)

1. Telegram'da **@BotFather** ni toping va unga yozing.
2. `/newbot` buyrug'ini yuboring, botga ism va username bering (username
   "bot" bilan tugashi kerak, masalan `AnorSchoolBot`).
3. BotFather sizga bir qator harf-raqamlardan iborat **token** beradi,
   masalan: `123456789:AAExampleTokenHere`. Buni saqlab qo'ying.

### 2. Chat ID'ni toping

**Agar natijalar sizning shaxsiy chatingizga kelishi kerak bo'lsa:**
1. Yaratgan botingizni Telegram'da toping va unga istalgan xabar yuboring
   (masalan "salom") — bot javob bermasa ham xabar yetib boradi.
2. Brauzerda quyidagi manzilni oching (TOKEN o'rniga o'zingizning
   tokeningizni qo'ying):
   `https://api.telegram.org/botTOKEN/getUpdates`
3. Sahifada JSON matn ko'rinadi, ichida `"chat":{"id":123456789,...}`
   qatorini toping — shu raqam sizning chat ID'ingiz.

**Agar natijalar guruh chatiga kelishi kerak bo'lsa:**
1. Botni shu guruhga a'zo qilib qo'shing.
2. Guruhda istalgan xabar yozing.
3. Xuddi yuqoridagidek `getUpdates` manzilini oching — guruh uchun chat ID
   odatda manfiy raqam bo'ladi, masalan `-1001234567890`.

### 3. .env fayliga kiriting

`.env` faylini ochib, quyidagi qatorlarni to'ldiring:

```
TELEGRAM_BOT_TOKEN=123456789:AAExampleTokenHere
TELEGRAM_CHAT_ID=123456789
```

### 4. Serverni qayta ishga tushiring

`.env` faylini o'zgartirgandan keyin serverni to'xtatib (`Ctrl+C`), qaytadan
`npm start` bilan ishga tushiring — shundan keyin "Telegramga yuborish"
tugmasi ishlay boshlaydi.

**Eslatma:** Agar `.env`da token/chat ID kiritilmagan bo'lsa, tugma bosilganda
"Telegram sozlanmagan" degan xabar chiqadi — sayt buzilmaydi, shunchaki
Telegram funksiyasi ishlamaydi.

## Ma'lumotlar qayerda saqlanadi

`data/` papkasida endi uchta alohida fayl bor:

- `data/students.json` — ro'yxatdan o'tgan barcha o'quvchilar (ism, sinf,
  parol va natijalar).
- `data/questions.json` — barcha bo'limlar uchun savollar banki.
- `data/settings.json` — umumiy sozlamalar (masalan, baholash rejimi:
  AI yoki Ustoz).

Agar avval eski `data/store.json` fayli bo'lgan bo'lsa, server birinchi
ishga tushganda undagi ma'lumotni avtomatik shu uchta faylga ko'chiradi —
hech narsa yo'qolmaydi.

## Tinglash savollariga audio yuklash

O'qituvchi panelida "Tinglash" bo'limi uchun savol qo'shganda, endi
xitoycha matn kiritish o'rniga to'g'ridan-to'g'ri **audio fayl yuklash**
mumkin (MP3, WAV va h.k.). O'quvchi testda shu audio faylni tinglaydi —
kompyuterning sun'iy talaffuziga (text-to-speech) endi ehtiyoj yo'q, va
o'qituvchi haqiqiy diktordan yozib olingan yoki o'zi aytib yozib olgan
audio bilan test tuza oladi. Eski (matndan sun'iy talaffuz qilinadigan)
savollar ham ishlashda davom etadi.

## Xarajat haqida eslatma

Har bir baholash so'rovi OpenAI hisobingizdan ozgina pul yechadi. O'quvchilar
soni ko'p bo'lsa, https://platform.openai.com/usage sahifasidan xarajatni
kuzatib turing va zarur bo'lsa limit (usage limit) qo'ying.
