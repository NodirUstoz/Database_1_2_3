# Ma'lumotlar Bazasi va PostgreSQL — Darsimizning mukammal qo'llanmasi

> **"Ma'lumot — bu yangi neft. Lekin uni to'g'ri saqlash — bu yangi san'at."**

Bu qo'llanma sizni ma'lumotlar bazasining tarixidan tortib, PostgreSQL'da professional darajada jadval yaratish, ma'lumot kiritish, yangilash, o'chirish va himoya qilishgacha bo'lgan yo'lda bosqichma-bosqich olib boradi. Har bir tushuncha hayotiy misollar, kod namunalari va vizual sxemalar bilan mustahkamlanadi.

---

## Mundarija

1. [Ma'lumotlar bazasi nima?](#1-malumotlar-bazasi-nima)
2. [Tarix: qo'lyozmadan bulutgacha](#2-tarix-qolyozmadan-bulutgacha)
3. [Ma'lumotlar bazasi turlari](#3-malumotlar-bazasi-turlari)
4. [PostgreSQL nima va nega aynan u?](#4-postgresql-nima-va-naga-aynan-u)
5. [PostgreSQL o'rnatish va sozlash](#5-postgresql-ornatish-va-sozlash)
6. [Birinchi qadam: "Hello World"](#6-birinchi-qadam-hello-world)
7. [Jadval anatomiyasi](#7-jadval-anatomiyasi)
8. [Ma'lumot turlari (Data Types)](#8-malumot-turlari-data-types)
9. [Cheklovlar (Constraints)](#9-cheklovlar-constraints)
10. [Jadval yaratishning 3 bosqichi](#10-jadval-yaratishning-3-bosqichi)
11. [CRUD operatsiyalari](#11-crud-operatsiyalari)
12. [Amaliy loyiha: "Kutubxona tizimi"](#12-amaliy-loyiha-kutubxona-tizimi)
13. [Xatolar va ularning yechimlari](#13-xatolar-va-ularning-yechimlari)
14. [Eng muhim qoidalar va maslahatlar](#14-eng-muhim-qoidalar-va-maslahatlar)
15. [Xulosa](#15-xulosa)

---

## 1. Ma'lumotlar bazasi nima?

Ma'lumotlar bazasi (Database) — bu ma'lumotlarni **tartibli, xavfsiz va tezkor** saqlash, boshqarish va qidirish imkonini beradigan tizim.

Hayotiy misol bilan tushuntirsak:

```
Tasavvur qiling, siz katta kutubxona boshqaruvchisisiz.

Kutubxonada 50 000 ta kitob bor.
Har bir kitobning nomi, muallifi, yili, janri, javon raqami bor.

Agar siz bularni daftarga yozsangiz:
  — "Alisher Navoiy" kitobini topish uchun 50 000 ta yozuvni ko'zdan kechirasiz.
  — Yangi kitob qo'shsangiz, to'g'ri joyga qo'yish qiyin.
  — Biror kitob yo'qolsa, bilmay qolasiz.

Agar siz ma'lumotlar bazasi ishlatsangiz:
  — "Navoiy" deb yozsangiz, 0.001 soniyada topiladi.
  — Yangi kitob qo'shish 1 buyruq.
  — Har qanday o'zgarish avtomatik qayd qilinadi.
```

**Ma'lumotlar bazasi** bu shunchaki "saqlash joyi" emas — bu **aqlli saqlash tizimi**.

### Ma'lumotlar bazasining asosiy vazifalari

```
┌─────────────────────────────────────────────────────────┐
│               MA'LUMOTLAR BAZASI VAZIFALARI              │
├──────────────┬──────────────┬──────────────┬─────────────┤
│   SAQLASH    │   QIDIRISH   │  YANGILASH   │   HIMOYA    │
│              │              │              │             │
│ Ma'lumotni   │ Tez va aniq  │ O'zgartirish │ Noto'g'ri   │
│ tartibli     │ topish       │ va qo'shish  │ ma'lumotdan │
│ joylashtirish│ imkoniyati   │ qulayligi    │ saqlash     │
└──────────────┴──────────────┴──────────────┴─────────────┘
```

---

## 2. Tarix: qo'lyozmadan bulutgacha

Ma'lumotlar bazasining tarixi — bu insoniyatning "eslab qolish" ehtiyojining tarixi.

### Davr chizmasi

```
    Qadim zamonlar          1950-60           1960-lar          1970            2000+            2020+
         │                    │                 │                │               │                │
    ┌────▼────┐          ┌────▼────┐       ┌────▼────┐     ┌────▼────┐     ┌────▼────┐      ┌────▼────┐
    │ QO'LDA  │          │  FAYL   │       │ DARAXT  │     │ JADVAL  │     │  NoSQL  │      │ VECTOR  │
    │ YOZUV   │───────►  │ TIZIMI  │──────►│ (TREE)  │────►│  MODEL  │────►│  BAZALAR│─────►│  BAZALAR│
    │         │          │         │       │ IBM IMS │     │ E.Codd  │     │         │      │  AI     │
    └─────────┘          └─────────┘       └─────────┘     └─────────┘     └─────────┘      └─────────┘
     Qog'oz,               .txt,            Ierarxik,       Relyatsion      Redis,           Pinecone,
     loy taxta             .csv fayllar      tarmoqli        SQL             MongoDB          Milvus
```

### Bosqich 1: Qo'lda yozuv (qadim zamonlar)

Insoniyat ming yillar davomida ma'lumotni **loy taxtachalarga**, **papirus**ga va **qog'oz**ga yozib saqlagan. Misr fir'avnlari soliq yig'ish uchun yozuvchilar armiyasini boqishgan. Savdogarlar mol ro'yxatini yuritishgan.

**Muammolar:**
- Qidirish: minglab sahifalarni varaqlab o'tirish kerak
- Xatolar: qo'lda ko'chirishda xatolar muqarrar
- Yangilash: o'zgargan ma'lumotni qaytadan yozish kerak
- Xavfsizlik: yong'in, suv toshqini — barcha ma'lumot yo'qoladi

### Bosqich 2: Fayl tizimi (1950–60-yillar)

Kompyuterlar paydo bo'lgach, ma'lumotlar raqamli fayllarga ko'chirildi. Lekin har bir dastur o'z formatida ishlardi:

```
Buxgalteriya dasturi  ───►  hisobot.dat
Kadrlar bo'limi       ───►  xodimlar.txt
Ombor dasturi         ───►  mahsulotlar.csv
```

**Muammo:** Bu fayllar bir-birini "tushunmasdi". Masalan, buxgalteriya dasturidagi xodim IDsi va kadrlar bo'limidagi xodim IDsi har xil formatda bo'lishi mumkin edi. Ma'lumotlar takrorlanar, ziddiyatlar paydo bo'lardi.

**Hayotiy misol:**
```
Tasavvur qiling, maktabda har bir o'qituvchi o'z daftarida
o'quvchilar ro'yxatini yuritadi. 

Matematika o'qituvchisi: "Alisher Karimov, 9-A"
Ingliz tili o'qituvchisi: "Karimov A., 9 sinf"

Endi direktor "Alisher Karimov qanday o'qiydi?" deb so'rasa,
ikki daftardagi ma'lumotni solishtirish juda qiyin.
```

### Bosqich 3: Ierarxik va tarmoqli modellar (1960-lar)

IBM 1966-yilda **IMS (Information Management System)** tizimini yaratdi — NASA'ning Apollo dasturi uchun! Ma'lumotlar **daraxt** (tree) shaklida tashkil etilardi:

```
                    Kompaniya
                   /         \
              Bo'lim A      Bo'lim B
              /    \          /    \
         Xodim1  Xodim2  Xodim3  Xodim4
```

**Afzalligi:** Tuzilma aniq, ierarxiya bor.

**Kamchiligi:** Agar xodim ikki bo'limda ishlasa nima bo'ladi? Daraxtda bu mumkin emas! Tarmoqli (graph) model bu muammoni hal qildi, lekin u juda murakkab edi — dasturchilar har safar kodni qayta yozishga majbur bo'lishardi.

### Bosqich 4: Relyatsion model — inqilob (1970)

1970-yilda **Edgar Frank Codd** (IBM tadqiqotchisi) "A Relational Model of Data for Large Shared Data Banks" nomli maqolasini nashr etdi. Bu maqola butun soha tarixini o'zgartirdi.

Codd'ning g'oyasi oddiy, lekin dahshatli kuchli edi:

> **"Ma'lumotlarni jadval (table) shaklida saqlang. Jadvallarni kalitlar orqali bog'lang."**

Bu g'oya asosida yaratilgan tizimlar:

```
┌─────────────────────────────────────────────────────────┐
│              RELYATSION BAZALAR OILASI                   │
├─────────────┬──────────────┬─────────────┬──────────────┤
│  Oracle     │   MySQL      │ PostgreSQL  │  SQL Server  │
│  (1979)     │   (1995)     │   (1996)    │   (1989)     │
│             │              │             │              │
│ Korporativ  │ Web saytlar  │ Universallik│ Microsoft    │
│ tizimlar    │ uchun mashhur│ va kuch     │ ekotizimi    │
└─────────────┴──────────────┴─────────────┴──────────────┘
```

### Bosqich 5: NoSQL inqilobi (2000+)

2000-yillarda internet portladi. Facebook, Google, Amazon kuniga **petabaytlab** ma'lumot yaratardi. Relyatsion bazalar bu hajmga dosh bera olmadi. **NoSQL (Not Only SQL)** tizimlari paydo bo'ldi.

### Bosqich 6: Vektor bazalar — AI davri (2020+)

Sun'iy intellekt rivojlanishi bilan yangi turdagi ma'lumot paydo bo'ldi — **vektorlar**. Endi savollar "bu so'z aynan shumi?" emas, balki **"mazmun jihatdan o'xshashmi?"** shaklida qo'yiladi.

```
An'anaviy qidiruv:       "mushuk" = "mushuk"  ✅
                          "mushuk" = "koshka"  ❌  (boshqa so'z!)

Vektor qidiruv:           "mushuk" ≈ "koshka"  ✅  (ma'nosi o'xshash!)
                          "mushuk" ≈ "it"      ✅  (ikkalasi ham uy hayvoni)
                          "mushuk" ≈ "mashina" ❌  (ma'no boshqa)
```

Vektor bazalar (Pinecone, Milvus, Weaviate) ma'lumotni **ko'p o'lchamli fazoda nuqtalar** sifatida saqlaydi. Mazmun jihatdan o'xshash so'zlar bir-biriga yaqin joylashadi.

---

## 3. Ma'lumotlar bazasi turlari

### Umumiy ko'rinish

```
┌──────────────────────────────────────────────────────────────────┐
│                    MA'LUMOTLAR BAZASI TURLARI                     │
├──────────────────┬───────────────────┬───────────────────────────┤
│       SQL        │      NoSQL        │       Vector DB           │
│   (Relyatsion)   │  (Not Only SQL)   │    (Vektor bazalar)       │
│                  │                   │                           │
│  ✅ Qat'iy qoida │  ✅ Moslashuvchan  │  ✅ Ma'no asosida qidiruv │
│  ✅ Tranzaksiya   │  ✅ Tez va kuchli  │  ✅ AI uchun yaratilgan   │
│  ✅ Bog'liqlik    │  ✅ Katta hajm     │  ✅ Semantik izlash       │
│                  │                   │                           │
│  Oracle, MySQL,  │  Redis, MongoDB,  │  Pinecone, Milvus,       │
│  PostgreSQL      │  Cassandra, Neo4j │  Weaviate, Qdrant        │
└──────────────────┴───────────────────┴───────────────────────────┘
```

### SQL (Relyatsion) bazalar

Relyatsion bazalarda ma'lumotlar **jadvallar** (tables) shaklida saqlanadi va ular **kalitlar** (keys) orqali o'zaro bog'lanadi.

**Hayotiy misol — Onlayn do'kon:**
```
"Buyurtmalar" jadvali          "Mijozlar" jadvali
┌────┬───────────┬─────────┐   ┌────┬──────────┬───────────┐
│ id │ mijoz_id  │ summa   │   │ id │ ism      │ shahar    │
├────┼───────────┼─────────┤   ├────┼──────────┼───────────┤
│ 1  │ 101       │ 250000  │   │101 │ Azizbek  │ Toshkent  │
│ 2  │ 102       │ 180000  │   │102 │ Madina   │ Samarqand │
│ 3  │ 101       │ 95000   │   │103 │ Bobur    │ Buxoro    │
└────┴───────────┴─────────┘   └────┴──────────┴───────────┘
         │                              ▲
         │         mijoz_id = id        │
         └──────────────────────────────┘
              (Foreign Key bog'lanish)
```

Bu yerda `mijoz_id` orqali har bir buyurtma qaysi mijozga tegishli ekanligi aniqlanadi. Bu bog'lanish — relyatsion bazalarning kuchi.

**Qachon ishlatish kerak:**
- Bank tizimlari (pul o'tkazmalari aniq bo'lishi shart)
- Buxgalteriya va moliya
- Inventar boshqaruvi
- Buyurtma va to'lov tizimlari

### NoSQL bazalar

NoSQL bazalar turli xil bo'ladi va har biri o'z muammosini hal qiladi:

#### 1. Key-Value (Kalit-Qiymat) — Redis

```
Tushuncha:    xuddi lug'at yoki telefon daftari

    "user:1001"  ──►  {"ism": "Ali", "yosh": 25}
    "session:xyz" ──►  {"token": "abc123", "muddat": "1h"}
    "cache:page1" ──►  "<html>...</html>"

Xususiyatlari:
  ⚡ O'ta tez (barcha ma'lumot xotirada — RAM)
  📦 Oddiy tuzilma
  ⏰ Muddatli saqlash imkoniyati (TTL)

Qachon ishlatiladi:
  — Keshlar (cache) — tez-tez so'raladigan ma'lumotlarni saqlash
  — Sessiyalar — foydalanuvchi tizimga kirgandagi holatni saqlash
  — Real-time ma'lumotlar — o'yin ballari, onlayn foydalanuvchilar soni
```

#### 2. Document DB — MongoDB

```
Tushuncha:    xuddi papkadagi hujjatlar

    {
      "_id": "64a1b2c3d4e5f6",
      "ism": "Sardor",
      "buyurtmalar": [
        {"mahsulot": "Laptop", "narx": 5200000},
        {"mahsulot": "Sichqoncha", "narx": 85000}
      ],
      "manzil": {
        "shahar": "Toshkent",
        "tuman": "Chilonzor",
        "ko'cha": "Bunyodkor 12"
      }
    }

Xususiyatlari:
  📋 JSON/BSON formatda saqlaydi
  🔄 Sxema bo'lishi shart emas (har bir hujjat boshqacha bo'lishi mumkin)
  📊 Ichma-ich ma'lumotlar saqlash mumkin

Qachon ishlatiladi:
  — Kontent boshqaruv tizimlari (CMS)
  — Mobil ilovalar backend
  — IoT qurilmalar ma'lumotlari
  — Kataloglar va profil ma'lumotlari
```

#### 3. Wide-Column DB — Cassandra

```
Tushuncha:    xuddi katta jurnal yoki log daftari

    ROW KEY: "sensor_001:2025-01-15"
    ┌──────────┬───────────┬────────────┬──────────┐
    │ temp:08  │ temp:09   │ temp:10    │ temp:11  │
    │ 22.5°C   │ 23.1°C    │ 24.8°C     │ 25.2°C   │
    └──────────┴───────────┴────────────┴──────────┘

Xususiyatlari:
  🌍 Dunyoning turli joylarida tarqatib saqlash mumkin
  📈 Yozish tezligi juda yuqori
  📊 Milliardlab qatorlarni boshqara oladi

Qachon ishlatiladi:
  — IoT sensor ma'lumotlari
  — Log va monitoring tizimlari
  — Vaqt seriyali ma'lumotlar (time-series data)
  — Messaging tizimlar (masalan, WhatsApp Cassandra ishlatadi)
```

#### 4. Graph DB — Neo4j

```
Tushuncha:    xuddi ijtimoiy tarmoq

    (Ali)───[DO'ST]───►(Vali)
      │                  │
      │                  │
   [ISHLAYDI]        [DO'ST]
      │                  │
      ▼                  ▼
   (Google)           (Sarvar)
                         │
                      [ISHLAYDI]
                         │
                         ▼
                      (Amazon)

Xususiyatlari:
  🔗 Aloqalar birinchi o'rinda
  🔍 Murakkab bog'lanishlarni tez topadi
  🕸️  "Kim kimni taniydi?" savollariga javob beradi

Qachon ishlatiladi:
  — Ijtimoiy tarmoqlar (do'stlar, kuzatuvchilar)
  — Tavsiya tizimlari ("Buni ham yoqtirishingiz mumkin")
  — Firibgarlikni aniqlash (fraud detection)
  — Bilim grafiklari (Knowledge graphs)
```

### Zamonaviy kompaniyalar qanday kombinatsiya qiladi?

```
┌───────────────────────────────────────────────────────────────┐
│          KATTA KOMPANIYA ARXITEKTURASI (misol)                 │
│                                                               │
│   Foydalanuvchi ──► API Gateway                               │
│                         │                                     │
│            ┌────────────┼────────────────┐                    │
│            │            │                │                    │
│            ▼            ▼                ▼                    │
│     ┌────────────┐ ┌─────────┐   ┌─────────────┐             │
│     │ PostgreSQL │ │  Redis  │   │   MongoDB   │             │
│     │            │ │         │   │             │             │
│     │ Buyurtmalar│ │ Kesh    │   │ Foydalanuvchi│             │
│     │ To'lovlar  │ │ Sessiya │   │ profillari   │             │
│     │ Hisoblar   │ │ Counter │   │ Kontentlar   │             │
│     └────────────┘ └─────────┘   └─────────────┘             │
│            │                           │                      │
│            └──────────┬────────────────┘                      │
│                       ▼                                       │
│              ┌──────────────┐                                 │
│              │  Pinecone    │                                 │
│              │ (Vector DB)  │                                 │
│              │              │                                 │
│              │ AI qidiruv   │                                 │
│              │ Tavsiyalar   │                                 │
│              └──────────────┘                                 │
└───────────────────────────────────────────────────────────────┘
```

---

## 4. PostgreSQL nima va naga aynan u?

**PostgreSQL** (o'qilishi: "post-gres-kyu-el") — bu dunyodagi eng kuchli, ochiq kodli (open-source) relyatsion ma'lumotlar bazasi boshqaruv tizimi (RDBMS).

### PostgreSQL tarixi

```
1986 — UC Berkeley'da POSTGRES loyihasi boshlandi (Professor Michael Stonebraker)
1996 — PostgreSQL nomi berildi, SQL qo'llab-quvvatlash qo'shildi
2025 — PostgreSQL 17+ versiyasi, dunyoning eng ishonchli bazalaridan biri
```

### Nega PostgreSQL?

```
┌──────────────────────────────────────────────────────────────┐
│                  PostgreSQL AFZALLIKLARI                      │
├──────────────────┬───────────────────────────────────────────┤
│ Bepul va ochiq   │ Litsenziya to'lovi yo'q, kodini          │
│ kodli            │ o'zgartirishingiz mumkin                  │
├──────────────────┼───────────────────────────────────────────┤
│ ACID mos         │ Ma'lumotlar har doim izchil va            │
│                  │ ishonchli saqlanadi                       │
├──────────────────┼───────────────────────────────────────────┤
│ Kengaytiriladigan│ O'z funksiyalaringizni, turlaringizni     │
│                  │ va tillaringizni qo'shishingiz mumkin     │
├──────────────────┼───────────────────────────────────────────┤
│ JSON qo'llab-    │ NoSQL uslubida ham ishlash mumkin         │
│ quvvatlaydi      │ (JSONB turi bilan)                       │
├──────────────────┼───────────────────────────────────────────┤
│ Katta jamoa      │ 30+ yillik tajriba, faol hamjamiyat,     │
│                  │ doimiy yangilanishlar                     │
├──────────────────┼───────────────────────────────────────────┤
│ Ishonchlilik     │ Bank, sog'liqni saqlash, hukumat         │
│                  │ tizimlari ishlatadi                       │
└──────────────────┴───────────────────────────────────────────┘
```

### ACID nima?

ACID — bu ma'lumotlar bazasining ishonchliligini ta'minlaydigan 4 ta asosiy tamoyil:

```
A — Atomicity  (Bo'linmaslik)
    Tranzaksiya yoki TO'LIQ bajariladi, yoki UMUMAN bajarilmaydi.

    Misol: Bank o'tkazmasi
    ┌──────────────────────────────────────────────┐
    │  Ali hisobidan  -500,000 so'm               │
    │  Vali hisobiga  +500,000 so'm               │
    │                                              │
    │  Agar ikkinchi qadam xato bo'lsa,            │
    │  birinchi qadam ham bekor bo'ladi!            │
    │  Pul "yo'qolib" ketmaydi.                    │
    └──────────────────────────────────────────────┘

C — Consistency  (Izchillik)
    Tranzaksiyadan oldin va keyin ma'lumotlar bazasi 
    qoidalarga mos holatda bo'lishi kerak.

    Misol: Hisobda 0 so'mdan kam bo'lishi mumkin emas.

I — Isolation  (Izolyatsiya)
    Bir vaqtda ishlaydigan tranzaksiyalar bir-biriga 
    xalaqit bermaydi.

    Misol: Ali va Vali bir vaqtda bitta aviachiptani 
    sotib olmoqchi. Faqat bittasi muvaffaq bo'ladi.

D — Durability  (Chidamlilik)
    Tranzaksiya tugagandan keyin ma'lumot saqlanadi,
    hatto server o'chib qolsa ham.

    Misol: "Buyurtma qabul qilindi" degan javob kelsa,
    tok o'chsa ham buyurtma bazada saqlanib qoladi.
```

### PostgreSQL vs MySQL — qisqacha solishtirish

```
┌─────────────────┬──────────────────┬──────────────────┐
│    Xususiyat     │   PostgreSQL     │     MySQL        │
├─────────────────┼──────────────────┼──────────────────┤
│ ACID to'liq     │       ✅          │     Qisman       │
│ JSON saqlash    │    JSONB (tez)   │   JSON (sekinroq)│
│ Murakkab so'rov │      Kuchli      │    O'rtacha      │
│ Kengaytirish    │   Juda yaxshi    │    Cheklangan    │
│ O'rganish       │    O'rtacha      │      Oson        │
│ Tezlik (o'qish) │      Yaxshi      │   Biroz tezroq  │
│ Tezlik (yozish) │    Juda yaxshi   │      Yaxshi      │
│ Litsenziya      │     PostgreSQL   │       GPL        │
│ Ishlatuvchilar  │ Apple, Instagram │ Facebook, Uber   │
└─────────────────┴──────────────────┴──────────────────┘
```

---

## 5. PostgreSQL o'rnatish va sozlash

### Linux (Ubuntu/Debian)

```bash
# O'rnatish
sudo apt update
sudo apt install postgresql postgresql-contrib

# Holatni tekshirish
sudo systemctl status postgresql

# PostgreSQL foydalanuvchisiga o'tish
sudo -u postgres psql

# Yangi baza yaratish
CREATE DATABASE mening_bazam;

# Yangi foydalanuvchi yaratish
CREATE USER sardor WITH PASSWORD 'xavfsiz_parol_123';

# Ruxsat berish
GRANT ALL PRIVILEGES ON DATABASE mening_bazam TO sardor;
```

### macOS

```bash
# Homebrew orqali
brew install postgresql@16
brew services start postgresql@16

# Bazaga kirish
psql postgres
```

### Windows

```
1. postgresql.org/download/windows saytiga kiring
2. Installer yuklab oling
3. O'rnatish jarayonida:
   — Parol belgilang (eslab qoling!)
   — Port: 5432 (standart)
   — Locale: Default
4. pgAdmin ham o'rnatiladi (grafik interfeys)
```

### psql asosiy buyruqlari

```
\l              — barcha bazalar ro'yxati
\c baza_nomi    — bazaga ulanish
\dt             — joriy bazadagi jadvallar
\d jadval_nomi  — jadval tuzilmasi
\q              — chiqish
\h              — SQL buyruqlar bo'yicha yordam
\?              — psql buyruqlari bo'yicha yordam
\timing         — so'rov bajarilish vaqtini ko'rsatish
```

---

## 6. Birinchi qadam: "Hello World"

Dasturlashning an'anaviy birinchi qadami — "Hello World". PostgreSQL'da bu juda oddiy:

```sql
SELECT 'Hello World!';
```

Natija:

```
  ?column?
─────────────
 Hello World!
(1 row)
```

### Biroz murakkabroq misollar

```sql
-- Hozirgi sana va vaqtni ko'rish
SELECT NOW();

-- Natija: 2025-06-15 14:30:45.123456+05

-- Oddiy hisob-kitob
SELECT 2 + 2 AS natija;

-- Natija:
--  natija
-- ────────
--       4

-- Matn birlashtirish
SELECT 'Salom, ' || 'PostgreSQL!' AS xabar;

-- Natija:
--        xabar
-- ────────────────────
--  Salom, PostgreSQL!

-- PostgreSQL versiyasini bilish
SELECT version();
```

---

## 7. Jadval anatomiyasi

### Jadval nima?

Jadval (table) — bu ma'lumotlar bazasining **yuragi**. Uni elektron jadval (Excel) ga o'xshash deb tasavvur qilish mumkin, lekin ancha kuchli va tartibli.

```
┌─────────────────────────────────────────────────────────────────┐
│                     JADVAL: mijozlar                             │
│                                                                 │
│    Jadval nomi ──► "mijozlar" (bitta mavzuga tegishli)          │
│                                                                 │
│  ┌──────┬──────────┬────────────┬──────────────────┐            │
│  │  id  │   ism    │   shahar   │     telefon      │ ◄── Ustun │
│  │      │          │            │                  │    (Column)│
│  ├──────┼──────────┼────────────┼──────────────────┤            │
│  │  1   │ Azizbek  │ Toshkent   │ +998 90 123 4567│ ◄── Satr  │
│  │  2   │ Madina   │ Samarqand  │ +998 91 765 4321│    (Row)   │
│  │  3   │ Bobur    │ Buxoro     │ +998 93 111 2233│            │
│  └──────┴──────────┴────────────┴──────────────────┘            │
│     ▲        ▲           ▲              ▲                       │
│     │        │           │              │                       │
│  Yacheyka (Cell) — ustun va satrning kesishgan joyi             │
└─────────────────────────────────────────────────────────────────┘
```

### Ustun (Column) — "Nima haqida?"

Har bir ustun **bir turdagi** ma'lumotni saqlaydi:

```
ism ustuni     ──► Barcha mijozlarning ismlari
shahar ustuni  ──► Ular yashaydigan joylar  
telefon ustuni ──► Aloqa raqamlari
```

Ustunlar jadvalning **tuzilmasini** belgilaydi. Ular "qanday turdagi ma'lumot saqlanadi?" savoliga javob beradi. To'g'ri ustunlar tanlash — yaxshi bazaning asosi.

### Satr (Row) — "Kim yoki nima haqida?"

Har bir satr **bitta aniq obyekt** haqida to'liq ma'lumot:

```
1-satr: Azizbek haqida hamma narsa (ismi, shahri, telefoni)
2-satr: Madina haqida hamma narsa
3-satr: Bobur haqida hamma narsa
```

### Yacheyka (Cell) — bitta qiymat

Ustun va satrning kesishgan joyi — bu yacheyka. Masalan, 2-satr va "shahar" ustunining kesishmasida "Samarqand" yozilgan.

### Nega jadval tuzilmasi kuchli?

```
1. TARTIB VA ANIQLIK
   Har bir ustun o'z ma'nosiga ega — chalkashlik yo'q.
   Siz "telefon" ustuniga "Toshkent" yoza olmaysiz (agar constraint qo'yilgan bo'lsa).

2. QIDIRISH OSON
   "Toshkentdagi barcha mijozlarni top":
   SELECT * FROM mijozlar WHERE shahar = 'Toshkent';
   — 0.001 soniyada natija!

3. YANGILASH QULAY
   Bitta mijozning telefonini o'zgartirish:
   UPDATE mijozlar SET telefon = '+998 94 555 6677' WHERE id = 2;
   — Faqat bitta satr o'zgaradi.

4. BOG'LASH MUMKIN
   "Buyurtmalar" jadvali bilan bog'lab, katta tizim yaratish mumkin:
   "Azizbek qancha buyurtma bergan?" — bitta so'rov bilan topish mumkin.
```

---

## 8. Ma'lumot turlari (Data Types)

Ma'lumot turi (Data Type) — bu ustunning "tili". Agar siz noto'g'ri til tanlasangiz, ma'lumot noto'g'ri saqlanadi yoki umuman saqlanmaydi.

**Hayotiy misol:**
```
Tasavvur qiling, siz anketani to'ldirmoqdasiz:

    Ismingiz: __________ (matn kutiladi)
    Yoshingiz: _________ (raqam kutiladi)
    Tug'ilgan sana: ____ (sana kutiladi)

Agar "Yoshingiz" maydoniga "yigirma besh" deb yozsangiz,
kompyuter buni tushunmaydi. U "25" degan raqam kutadi.

Ma'lumot turi — bu shu "kutish" ni aniqlaydi.
```

### 8.1. Matn turlari (Text Types)

```
┌──────────────┬────────────────────────────────────────────────────────────┐
│    Turi       │ Tavsif va misol                                           │
├──────────────┼────────────────────────────────────────────────────────────┤
│ CHAR(n)      │ Belgilar soni QAT'IY. Har doim n ta belgi saqlaydi.       │
│              │                                                            │
│              │ CHAR(5) ga "Ali" deb yozsangiz, u "Ali  " bo'ladi         │
│              │ (2 ta bo'sh joy qo'shiladi).                               │
│              │                                                            │
│              │ Ishlatiladi: davlat kodi ('UZ'), jinsi ('E'/'A'),         │
│              │              valuta kodi ('USD')                           │
├──────────────┼────────────────────────────────────────────────────────────┤
│ VARCHAR(n)   │ Belgilar soni CHEGARALANGAN, lekin o'zgaruvchan.          │
│              │                                                            │
│              │ VARCHAR(255) ga "Ali" deb yozsangiz, faqat 3 ta belgi     │
│              │ saqlaydi (ortiqcha joy ishlatmaydi).                       │
│              │                                                            │
│              │ Ishlatiladi: ism, email, telefon, manzil                   │
├──────────────┼────────────────────────────────────────────────────────────┤
│ TEXT         │ Har qanday uzunlikdagi matn. Cheksiz (deyarli).           │
│              │                                                            │
│              │ PostgreSQL'da TEXT deyarli cheklanmagan uzunlikka ega.     │
│              │                                                            │
│              │ Ishlatiladi: izoh, sharh, maqola matni, tavsif             │
└──────────────┴────────────────────────────────────────────────────────────┘
```

**Qachon qaysi birini tanlash kerak?**

```
CHAR(2)       ──► Davlat kodi: 'UZ', 'US', 'RU'
CHAR(3)       ──► Valuta kodi: 'UZS', 'USD', 'EUR'
VARCHAR(100)  ──► Ism: 'Abdullayev Sardor Kamoliddin o'g'li'
VARCHAR(255)  ──► Email: 'sardor.abdullayev@example.com'
TEXT          ──► Mahsulot tavsifi: "Bu laptop 16GB RAM, 512GB SSD..."
```

### 8.2. Raqam turlari (Numeric Types)

```
┌──────────────────────┬───────────────────────────────────────────────────┐
│      Turi             │ Tavsif, diapazoni va misol                       │
├──────────────────────┼───────────────────────────────────────────────────┤
│ SMALLINT             │ Kichik butun sonlar                               │
│                      │ Diapazoni: -32,768 dan 32,767 gacha               │
│                      │ Xotira: 2 bayt                                    │
│                      │ Misol: yosh, oy raqami, kurs bahosi               │
├──────────────────────┼───────────────────────────────────────────────────┤
│ INTEGER (INT)        │ Standart butun sonlar                             │
│                      │ Diapazoni: ≈ -2.1 mlrd dan 2.1 mlrd gacha        │
│                      │ Xotira: 4 bayt                                    │
│                      │ Misol: id, soni, miqdor                           │
├──────────────────────┼───────────────────────────────────────────────────┤
│ BIGINT               │ Juda katta butun sonlar                           │
│                      │ Diapazoni: ≈ -9.2 * 10^18 dan 9.2 * 10^18 gacha  │
│                      │ Xotira: 8 bayt                                    │
│                      │ Misol: katta ID, timestamp, statistika            │
├──────────────────────┼───────────────────────────────────────────────────┤
│ NUMERIC(p, s)        │ Aniqligi yuqori o'nlik sonlar                     │
│                      │ p = jami raqamlar soni, s = verguldan keyin       │
│                      │ Xotira: o'zgaruvchan                              │
│                      │                                                    │
│                      │ NUMERIC(10, 2):                                    │
│                      │   ✅  12345678.99  (10 raqam, 2 si kasr)          │
│                      │   ❌  123456789.99 (11 raqam — sig'maydi)         │
│                      │                                                    │
│                      │ Misol: narx, maosh, pul miqdori                   │
│                      │ ⚠️ Moliyaviy hisoblarda FAQAT shu turni ishlating!│
├──────────────────────┼───────────────────────────────────────────────────┤
│ REAL                 │ 4 baytli float (6 xonagacha aniqlik)              │
│ DOUBLE PRECISION     │ 8 baytli float (15 xonagacha aniqlik)             │
│                      │                                                    │
│                      │ ⚠️ DIQQAT: Float sonlar aniq emas!               │
│                      │   0.1 + 0.2 = 0.30000000000000004                  │
│                      │   Shu sababli PUL uchun HECH QACHON ishlatmang!   │
│                      │                                                    │
│                      │ Misol: harorat, koordinata, ilmiy hisob           │
├──────────────────────┼───────────────────────────────────────────────────┤
│ SERIAL               │ Avtomatik o'suvchi butun son (1, 2, 3, ...)       │
│ BIGSERIAL            │ Katta avtomatik o'suvchi son                      │
│                      │                                                    │
│                      │ id SERIAL PRIMARY KEY                             │
│                      │ — har yangi yozuvga avtomatik yangi raqam beriladi│
└──────────────────────┴───────────────────────────────────────────────────┘
```

**MUHIM OGOHLANTIRISH — Noto'g'ri raqam turi tanlash xavfli:**

```sql
-- ❌ XATO: Narxni REAL (float) da saqlash
CREATE TABLE mahsulotlar (
    narx REAL  -- XATO TANLOV!
);

INSERT INTO mahsulotlar (narx) VALUES (19.99);
SELECT narx * 1000000 FROM mahsulotlar;
-- Natija: 19989999.xxx  (NOTO'G'RI! 19990000 bo'lishi kerak edi)

-- ✅ TO'G'RI: Narxni NUMERIC da saqlash
CREATE TABLE mahsulotlar (
    narx NUMERIC(12, 2)  -- TO'G'RI TANLOV!
);
```

### 8.3. Sana va vaqt turlari (Date/Time Types)

```
┌──────────────┬─────────────────────────────────────────────────────────┐
│    Turi       │ Tavsif va misol                                        │
├──────────────┼─────────────────────────────────────────────────────────┤
│ DATE         │ Faqat sana                                              │
│              │ Format: YYYY-MM-DD                                      │
│              │ Misol: '2025-11-01'                                     │
│              │ Ishlatiladi: tug'ilgan sana, boshlanish sanasi           │
├──────────────┼─────────────────────────────────────────────────────────┤
│ TIME         │ Faqat vaqt                                              │
│              │ Format: HH:MM:SS                                        │
│              │ Misol: '14:30:00'                                       │
│              │ Ishlatiladi: ish boshlanish vaqti, qo'ng'iroq vaqti     │
├──────────────┼─────────────────────────────────────────────────────────┤
│ TIMESTAMP    │ Sana + vaqt birgalikda                                  │
│              │ Format: YYYY-MM-DD HH:MM:SS                             │
│              │ Misol: '2025-11-01 14:30:00'                            │
│              │ Ishlatiladi: yaratilgan vaqt, oxirgi kirish              │
├──────────────┼─────────────────────────────────────────────────────────┤
│ TIMESTAMPTZ  │ Sana + vaqt + VAQT ZONASI                              │
│              │ Format: YYYY-MM-DD HH:MM:SS+TZ                          │
│              │ Misol: '2025-11-01 14:30:00+05:00'                      │
│              │                                                          │
│              │ ⭐ ENG YAXSHI TANLOV!                                   │
│              │ Xalqaro loyihalarda har doim shu turni ishlating.       │
│              │ PostgreSQL avtomatik UTC ga o'girib saqlaydi.           │
├──────────────┼─────────────────────────────────────────────────────────┤
│ INTERVAL     │ Vaqt oralig'i (davomiylik)                              │
│              │ Misol: '3 days', '2 hours 30 minutes', '1 year'        │
│              │ Ishlatiladi: muddatlar, kechikishlar, hisoblashlar      │
└──────────────┴─────────────────────────────────────────────────────────┘
```

**Amaliy misollar:**

```sql
-- Bugundan 30 kun keyingi sanani hisoblash
SELECT CURRENT_DATE + INTERVAL '30 days' AS muddat;

-- Foydalanuvchi necha kun oldin ro'yxatdan o'tganini bilish
SELECT NOW() - yaratilgan_vaqt AS farq FROM foydalanuvchilar;

-- 18 yoshdan katta mijozlarni topish
SELECT * FROM mijozlar 
WHERE tugilgan_sana <= CURRENT_DATE - INTERVAL '18 years';
```

### 8.4. Boshqa muhim turlar

```
┌──────────────┬─────────────────────────────────────────────────────────────┐
│    Turi       │ Tavsif, misol va qachon ishlatiladi                        │
├──────────────┼─────────────────────────────────────────────────────────────┤
│ BOOLEAN      │ Faqat TRUE yoki FALSE                                       │
│              │                                                              │
│              │ faol BOOLEAN DEFAULT TRUE                                    │
│              │ tasdiqlangan BOOLEAN DEFAULT FALSE                           │
│              │                                                              │
│              │ Misol: foydalanuvchi faolmi? Email tasdiqlandimi?            │
├──────────────┼─────────────────────────────────────────────────────────────┤
│ UUID         │ Unikal identifikator (128 bit)                              │
│              │ Misol: 'a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11'              │
│              │                                                              │
│              │ SERIAL dan farqi: UUID ni oldindan aytib bo'lmaydi,         │
│              │ shu sababli xavfsizroq (URL da id=1, id=2 ko'rinmaydi).    │
│              │                                                              │
│              │ Misol: foydalanuvchi ID, API kalitlari                       │
├──────────────┼─────────────────────────────────────────────────────────────┤
│ JSON / JSONB │ JSON formatdagi ma'lumotlar                                  │
│              │                                                              │
│              │ JSON — matn sifatida saqlaydi (sekinroq)                    │
│              │ JSONB — ikkilik formatda saqlaydi (tezroq, indeks qo'yiladi)│
│              │                                                              │
│              │ sozlamalar JSONB DEFAULT '{}'                                │
│              │ Misol: {"til": "uz", "tema": "qorong'u", "bildirishnoma": true}│
│              │                                                              │
│              │ ⭐ JSONB ni ishlating, JSON ni emas!                        │
├──────────────┼─────────────────────────────────────────────────────────────┤
│ ARRAY        │ Bir ustunda bir nechta qiymat                               │
│              │                                                              │
│              │ teglar TEXT[] DEFAULT '{}'                                   │
│              │ Misol: ARRAY['postgresql', 'sql', 'database']               │
│              │                                                              │
│              │ Ishlatiladi: teglar, kategoriyalar, ruxsatlar               │
├──────────────┼─────────────────────────────────────────────────────────────┤
│ BYTEA        │ Ikkilik (binary) ma'lumotlar                                │
│              │                                                              │
│              │ Rasm, fayl, shifrlangan ma'lumot saqlash uchun              │
│              │ ⚠️ Katta fayllar uchun fayl tizimidan foydalaning          │
├──────────────┼─────────────────────────────────────────────────────────────┤
│ ENUM         │ Cheklangan qiymatlar to'plami                               │
│              │                                                              │
│              │ CREATE TYPE jinsi AS ENUM ('erkak', 'ayol');                │
│              │                                                              │
│              │ Misol: buyurtma holati ('kutilmoqda', 'yuborildi',          │
│              │        'yetkazildi', 'bekor qilindi')                       │
└──────────────┴─────────────────────────────────────────────────────────────┘
```

### Data Type tanlash cheat-sheet

```
Nimani saqlayapsiz?              ──►  Qaysi turni ishlating?

Ism, email, manzil               ──►  VARCHAR(n) yoki TEXT
Tavsif, sharh, maqola            ──►  TEXT
Davlat kodi, valuta kodi         ──►  CHAR(2) yoki CHAR(3)
Yosh, soni, miqdor               ──►  INTEGER
Katta raqam (ID, statistika)     ──►  BIGINT
Narx, maosh, pul                 ──►  NUMERIC(p, s)   ⚠️ FLOAT emas!
Harorat, koordinata              ──►  DOUBLE PRECISION
Avtomatik ID                     ──►  SERIAL yoki BIGSERIAL
Tug'ilgan sana                   ──►  DATE
Yaratilgan vaqt                  ──►  TIMESTAMPTZ      ⭐ eng yaxshi
Ha/Yo'q (flag)                   ──►  BOOLEAN
Xavfsiz unikal ID                ──►  UUID
Sozlamalar, metadata             ──►  JSONB
Teglar, kategoriyalar            ──►  TEXT[]
Fayl, rasm (kichik)              ──►  BYTEA
Cheklangan ro'yxat               ──►  ENUM yoki CHECK
```

---

## 9. Cheklovlar (Constraints)

Cheklovlar — bu bazadagi **"qonunlar"**. Ular noto'g'ri yoki mantiqsiz ma'lumot kiritilishiga yo'l qo'ymaydi.

**Hayotiy misol:**
```
Tasavvur qiling, siz bank tizimi yaratyapsiz.

Cheklovlarsiz:
  — Mijozning ismi bo'sh bo'lishi mumkin (kim u?)
  — Balans -1,000,000 so'm bo'lishi mumkin (bank qarzga botadi!)
  — Ikki kishi bir xil pasport raqamiga ega bo'lishi mumkin (firibgarlik!)
  — Tug'ilgan sana 2030-yil bo'lishi mumkin (kelajakdan kelgan mijoz?)

Cheklovlar bilan:
  — Ism bo'sh bo'lishi MUMKIN EMAS (NOT NULL)
  — Balans 0 dan kam bo'lishi MUMKIN EMAS (CHECK)
  — Pasport raqami takrorlanishi MUMKIN EMAS (UNIQUE)
  — Tug'ilgan sana kelajakda bo'lishi MUMKIN EMAS (CHECK)
```

### 9.1. PRIMARY KEY — Noyob identifikator

Har bir jadvaldagi har bir satrni **yagona aniqlash** uchun ishlatiladi. Bu xuddi har bir fuqaroning pasport raqami kabi — ikki kishi bir xil raqamga ega bo'lishi mumkin emas.

```sql
CREATE TABLE mijozlar (
    id SERIAL PRIMARY KEY  -- Har bir mijozning noyob raqami
);
```

```
PRIMARY KEY = UNIQUE + NOT NULL

Ya'ni:
  — Qiymat takrorlanishi mumkin emas (UNIQUE)
  — Qiymat bo'sh bo'lishi mumkin emas (NOT NULL)
  — Har bir jadvalda faqat BITTA PRIMARY KEY bo'lishi mumkin
```

**Qo'shma PRIMARY KEY** (ikki ustundan iborat):

```sql
-- Bir talaba bitta fanni faqat bir marta o'qishi mumkin
CREATE TABLE talaba_fan (
    talaba_id INTEGER,
    fan_id INTEGER,
    baho SMALLINT,
    PRIMARY KEY (talaba_id, fan_id)  -- Qo'shma kalit
);

-- Bu kombinatsiya noyob bo'lishi kerak:
-- ✅ (1, 101) — Talaba 1, Fan 101
-- ✅ (1, 102) — Talaba 1, Fan 102  (boshqa fan)
-- ✅ (2, 101) — Talaba 2, Fan 101  (boshqa talaba)
-- ❌ (1, 101) — XATO! Bu kombinatsiya allaqachon bor!
```

### 9.2. UNIQUE — Takrorlanmaslik

Ustundagi qiymatlar **takrorlanmasligini** ta'minlaydi. PRIMARY KEY dan farqi — jadvalda bir nechta UNIQUE ustun bo'lishi mumkin va NULL qiymatga ruxsat beradi.

```sql
CREATE TABLE foydalanuvchilar (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE,          -- Har bir email yagona
    telefon VARCHAR(20) UNIQUE,         -- Har bir telefon yagona
    passport_raqam CHAR(9) UNIQUE       -- Har bir passport yagona
);

-- ✅ Muvaffaqiyatli
INSERT INTO foydalanuvchilar (email, telefon) 
VALUES ('ali@mail.com', '+998901234567');

-- ❌ XATO! Bu email allaqachon bor
INSERT INTO foydalanuvchilar (email, telefon) 
VALUES ('ali@mail.com', '+998907654321');
-- ERROR: duplicate key value violates unique constraint
```

### 9.3. NOT NULL — Bo'sh bo'lmasin

Ustun **bo'sh qoldirilmasligini** ta'minlaydi. NULL — bu "noma'lum" yoki "kiritilmagan" ma'noni bildiradi.

```sql
CREATE TABLE buyurtmalar (
    id SERIAL PRIMARY KEY,
    mijoz_id INTEGER NOT NULL,       -- Buyurtma kimsiz bo'lishi mumkin emas
    summa NUMERIC(12,2) NOT NULL,    -- Summa bo'sh bo'lishi mumkin emas
    izoh TEXT                         -- Izoh bo'sh bo'lishi MUMKIN (ixtiyoriy)
);

-- ✅ To'g'ri
INSERT INTO buyurtmalar (mijoz_id, summa) VALUES (1, 150000);

-- ❌ XATO!
INSERT INTO buyurtmalar (mijoz_id, summa) VALUES (NULL, 150000);
-- ERROR: null value in column "mijoz_id" violates not-null constraint
```

**Qachon NOT NULL ishlatish kerak?**

```
HAR DOIM NOT NULL qo'ying, AGAR:
  ✅ Ma'lumot mantiqan majburiy bo'lsa (ism, email, narx)
  ✅ Boshqa jadvallar shu ustunni ishlatsa (foreign key)
  ✅ Hisobotlar shu ustun bo'yicha tuzilsa

NOT NULL QOYMANG, AGAR:
  ⬜ Ma'lumot ixtiyoriy bo'lsa (izoh, ikkinchi telefon)
  ⬜ Ma'lumot keyinroq to'ldirilishi mumkin bo'lsa (profil rasmi)
```

### 9.4. CHECK — Mantiqiy shart

Ustundagi qiymatga **mantiqiy qoida** qo'yadi. Agar qiymat shartga mos kelmasa, PostgreSQL xatolik beradi.

```sql
CREATE TABLE xodimlar (
    id SERIAL PRIMARY KEY,
    ism TEXT NOT NULL,
    yosh INTEGER CHECK (yosh >= 18 AND yosh <= 120),
    maosh NUMERIC(12,2) CHECK (maosh >= 0),
    jinsi CHAR(1) CHECK (jinsi IN ('E', 'A')),
    tugilgan_sana DATE CHECK (tugilgan_sana < CURRENT_DATE),
    ishga_kirgan DATE CHECK (ishga_kirgan <= CURRENT_DATE),
    
    -- Jadval darajasidagi CHECK (ikki ustunni solishtirish)
    CHECK (ishga_kirgan > tugilgan_sana + INTERVAL '18 years')
    -- Xodim 18 yoshdan katta bo'lganda ishga kirgan bo'lishi kerak
);
```

**Hayotiy misollar:**

```sql
-- Narx manfiy bo'lmasligi kerak
narx NUMERIC(12,2) CHECK (narx >= 0)

-- Chegirma 0% dan 100% gacha bo'lishi kerak
chegirma INTEGER CHECK (chegirma >= 0 AND chegirma <= 100)

-- Email format tekshiruvi (oddiy)
email VARCHAR(255) CHECK (email LIKE '%@%.%')

-- Buyurtma holati faqat ma'lum qiymatlardan bo'lishi kerak
status TEXT CHECK (status IN ('kutilmoqda', 'jarayonda', 'yuborildi', 'yetkazildi', 'bekor'))
```

### 9.5. DEFAULT — Sukut qiymati

Agar foydalanuvchi qiymat kiritmasa, tizim **avtomatik** belgilangan qiymatni qo'yadi.

```sql
CREATE TABLE postlar (
    id SERIAL PRIMARY KEY,
    sarlavha TEXT NOT NULL,
    matn TEXT NOT NULL,
    yaratilgan_vaqt TIMESTAMPTZ DEFAULT NOW(),          -- Hozirgi vaqt
    yangilangan_vaqt TIMESTAMPTZ DEFAULT NOW(),
    status TEXT DEFAULT 'qoralama',                       -- Sukut: qoralama
    ko'rishlar_soni INTEGER DEFAULT 0,                    -- Sukut: 0
    yoqtirishlar INTEGER DEFAULT 0,
    chop_etilganmi BOOLEAN DEFAULT FALSE                  -- Sukut: yo'q
);

-- Faqat sarlavha va matn kiritsangiz bas:
INSERT INTO postlar (sarlavha, matn) 
VALUES ('Birinchi post', 'Salom dunyo!');

-- Natija:
-- id=1, sarlavha='Birinchi post', matn='Salom dunyo!',
-- yaratilgan_vaqt=2025-06-15 14:30:00+05, status='qoralama',
-- ko'rishlar_soni=0, yoqtirishlar=0, chop_etilganmi=false
```

### 9.6. FOREIGN KEY — Jadvallar orasidagi bog'lanish

Foreign Key (tashqi kalit) — ikki jadvalni bir-biriga bog'laydi. Bu bog'lanish orqali ma'lumotlar **izchil** saqlanadi.

```sql
-- Avval "ota" jadval
CREATE TABLE kategoriyalar (
    id SERIAL PRIMARY KEY,
    nomi TEXT NOT NULL
);

-- Keyin "bola" jadval
CREATE TABLE mahsulotlar (
    id SERIAL PRIMARY KEY,
    nomi TEXT NOT NULL,
    narx NUMERIC(12,2) NOT NULL,
    kategoriya_id INTEGER REFERENCES kategoriyalar(id)
    -- Bu ustun faqat kategoriyalar jadvalidagi mavjud id larni qabul qiladi
);

-- ✅ To'g'ri (agar kategoriyalar jadvalida id=1 bo'lsa)
INSERT INTO mahsulotlar (nomi, narx, kategoriya_id) 
VALUES ('Laptop', 5200000, 1);

-- ❌ XATO (kategoriyalar jadvalida id=999 yo'q)
INSERT INTO mahsulotlar (nomi, narx, kategoriya_id) 
VALUES ('Telefon', 3000000, 999);
-- ERROR: insert or update on table "mahsulotlar" violates foreign key constraint
```

**Vizual tushuntirish:**

```
kategoriyalar jadvali           mahsulotlar jadvali
┌────┬──────────────┐           ┌────┬──────────┬───────────┬──────────────┐
│ id │ nomi         │           │ id │ nomi     │ narx      │kategoriya_id │
├────┼──────────────┤           ├────┼──────────┼───────────┼──────────────┤
│  1 │ Elektronika  │◄──────────│  1 │ Laptop   │ 5,200,000 │      1       │
│  2 │ Kiyim-kechak │◄──────────│  2 │ Ko'ylak  │   180,000 │      2       │
│  3 │ Oziq-ovqat   │◄──┐       │  3 │ Telefon  │ 3,000,000 │      1       │
└────┴──────────────┘   └───────│  4 │ Non      │     5,000 │      3       │
                                └────┴──────────┴───────────┴──────────────┘

        kategoriya_id ──► kategoriyalar.id ga ishora qiladi
```

### Barcha cheklovlar xulosa jadvali

```
┌──────────────────┬───────────────────────────────────────────────────────┐
│   Cheklov         │ Maqsad                                               │
├──────────────────┼───────────────────────────────────────────────────────┤
│ PRIMARY KEY      │ Har bir satrni noyob aniqlash (ID)                    │
│ UNIQUE           │ Qiymatlar takrorlanmasligi (email, telefon)           │
│ NOT NULL         │ Maydon bo'sh bo'lmasligi (ism, narx)                  │
│ CHECK            │ Mantiqiy shartni tekshirish (yosh >= 18)              │
│ DEFAULT          │ Sukut qiymatni belgilash (status = 'active')          │
│ FOREIGN KEY      │ Jadvallar orasidagi bog'lanish (mijoz_id → mijozlar) │
│ EXCLUDE          │ Murakkab mos kelmaslik (masalan, vaqt oraliqlarida)   │
└──────────────────┴───────────────────────────────────────────────────────┘
```

---

## 10. Jadval yaratishning 3 bosqichi

Bu 3 bosqich — har bir dasturchi va muhandisning o'sish yo'lini aks ettiradi.

### Bosqich 1: Oddiy jadval (Junior yondashuv)

```sql
-- ❌ Hech qanday tur va cheklov yo'q
CREATE TABLE mijozlar (
    id,
    ism,
    telefon,
    shahar
);

-- Noto'g'ri ma'lumot kiritish mumkin:
INSERT INTO mijozlar VALUES ('bir', 123, NULL, 9999);
-- Baza hech narsa demaydi, lekin ma'lumot mantiqan noto'g'ri!
```

```
Muammolar:
  ❌ Id matn bo'lishi mumkin ("bir" — bu nima?)
  ❌ Ism raqam bo'lishi mumkin (123 — bu ism emas)
  ❌ Telefon bo'sh (NULL) — bog'lanib bo'lmaydi
  ❌ Shahar raqam (9999) — bu qaysi shahar?

Xulosa: "Yozadi, lekin NIMA yozayotganini bilmaydi"
```

### Bosqich 2: Yaxshi jadval (Middle yondashuv)

```sql
-- ✅ Data type'lar aniqlangan
CREATE TABLE mijozlar (
    id SERIAL,
    ism TEXT,
    telefon VARCHAR(20),
    shahar TEXT,
    tugilgan_sana DATE
);

-- Endi noto'g'ri turdagi ma'lumot kiritish mumkin emas:
INSERT INTO mijozlar (ism, telefon, tugilgan_sana)
VALUES ('Otabek', '+998901234567', '1998-05-12');  -- ✅

INSERT INTO mijozlar (ism, telefon, tugilgan_sana)
VALUES ('Otabek', '+998901234567', 'salom');  -- ❌ Sana emas!
```

```
Yaxshi tomonlari:
  ✅ id avtomatik raqamlanadi
  ✅ tugilgan_sana faqat sana bo'lishi mumkin

Hali ham muammolar:
  ⚠️ Ism bo'sh bo'lishi mumkin
  ⚠️ Bir xil telefon bir necha marta kiritilishi mumkin
  ⚠️ Tug'ilgan sana kelajakda bo'lishi mumkin

Xulosa: "Nima yozayotganini biladi, lekin NAZORAT qilmaydi"
```

### Bosqich 3: Cheklovli jadval (Senior yondashuv)

```sql
-- ✅ Barcha muhim cheklovlar bilan
CREATE TABLE mijozlar (
    id SERIAL PRIMARY KEY,
    ism TEXT NOT NULL,
    telefon VARCHAR(20) UNIQUE,
    shahar TEXT DEFAULT 'Toshkent',
    tugilgan_sana DATE CHECK (tugilgan_sana < CURRENT_DATE),
    status TEXT DEFAULT 'active' CHECK (status IN ('active', 'inactive')),
    yaratilgan_vaqt TIMESTAMPTZ DEFAULT NOW()
);
```

```
Himoya qatlamlari:
  ✅ id — har bir satr noyob (PRIMARY KEY)
  ✅ ism — bo'sh bo'lishi MUMKIN EMAS (NOT NULL)
  ✅ telefon — takrorlanishi MUMKIN EMAS (UNIQUE)
  ✅ shahar — kiritilmasa avtomatik 'Toshkent' (DEFAULT)
  ✅ tugilgan_sana — kelajakda bo'lishi MUMKIN EMAS (CHECK)
  ✅ status — faqat 'active' yoki 'inactive' (CHECK)
  ✅ yaratilgan_vaqt — avtomatik hozirgi vaqt (DEFAULT)

Xulosa: "Baza o'zini himoya qiladi. Noto'g'ri ma'lumotga ruxsat bermaydi."
```

**Tekshiramiz:**

```sql
-- ✅ Muvaffaqiyatli
INSERT INTO mijozlar (ism, telefon, tugilgan_sana)
VALUES ('Otabek', '+998909991122', '1995-03-21');

-- ❌ XATO: ism bo'sh
INSERT INTO mijozlar (ism, telefon, tugilgan_sana)
VALUES (NULL, '+998909991122', '1995-03-21');
-- ERROR: null value in column "ism" violates not-null constraint

-- ❌ XATO: telefon takrorlanadi
INSERT INTO mijozlar (ism, telefon, tugilgan_sana)
VALUES ('Sardor', '+998909991122', '1998-07-10');
-- ERROR: duplicate key value violates unique constraint

-- ❌ XATO: kelajak sana
INSERT INTO mijozlar (ism, telefon, tugilgan_sana)
VALUES ('Nodir', '+998901112233', '2030-01-01');
-- ERROR: new row for relation "mijozlar" violates check constraint
```

---

## 11. CRUD operatsiyalari

CRUD — bu ma'lumotlar bazasi bilan ishlashning 4 ta asosiy operatsiyasi:

```
┌─────────────────────────────────────────────────────────────────┐
│                        CRUD                                      │
├────────┬──────────────────────┬──────────────────────────────────┤
│ Harf   │ Ma'nosi              │ SQL buyrug'i                     │
├────────┼──────────────────────┼──────────────────────────────────┤
│   C    │ Create (Yaratish)    │ INSERT INTO                      │
│   R    │ Read   (O'qish)      │ SELECT                           │
│   U    │ Update (Yangilash)   │ UPDATE ... SET ... WHERE         │
│   D    │ Delete (O'chirish)   │ DELETE FROM ... WHERE            │
└────────┴──────────────────────┴──────────────────────────────────┘
```

Keling, avval jadval yaratamiz:

```sql
CREATE TABLE mijozlar (
    id SERIAL PRIMARY KEY,
    ism TEXT NOT NULL,
    telefon VARCHAR(20) UNIQUE,
    shahar TEXT DEFAULT 'Toshkent',
    tugilgan_sana DATE,
    status TEXT DEFAULT 'active'
);
```

### 11.1. CREATE — INSERT (Ma'lumot qo'shish)

```sql
-- Bitta yozuv qo'shish
INSERT INTO mijozlar (ism, telefon, tugilgan_sana)
VALUES ('Otabek', '+998901234567', '1995-05-12');

-- Bir nechta yozuv qo'shish
INSERT INTO mijozlar (ism, telefon, tugilgan_sana, shahar) VALUES
    ('Madina', '+998911112233', '1998-08-25', 'Samarqand'),
    ('Bobur',  '+998934445566', '2000-01-15', 'Buxoro'),
    ('Nilufar','+998977778899', '1997-12-03', 'Toshkent'),
    ('Jasur',  '+998950001122', '1993-06-30', 'Namangan');

-- Natija:
-- ┌────┬─────────┬────────────────┬───────────┬───────────────┬────────┐
-- │ id │  ism    │   telefon      │  shahar   │ tugilgan_sana │ status │
-- ├────┼─────────┼────────────────┼───────────┼───────────────┼────────┤
-- │  1 │ Otabek  │ +998901234567  │ Toshkent  │ 1995-05-12    │ active │
-- │  2 │ Madina  │ +998911112233  │ Samarqand │ 1998-08-25    │ active │
-- │  3 │ Bobur   │ +998934445566  │ Buxoro    │ 2000-01-15    │ active │
-- │  4 │ Nilufar │ +998977778899  │ Toshkent  │ 1997-12-03    │ active │
-- │  5 │ Jasur   │ +998950001122  │ Namangan  │ 1993-06-30    │ active │
-- └────┴─────────┴────────────────┴───────────┴───────────────┴────────┘
```

**Muhim eslatmalar:**
```
1. id va status ni kiritmadik — ular avtomatik to'ldirildi
   (id = SERIAL, status = DEFAULT 'active')

2. Otabek uchun shahar ni kiritmadik — avtomatik 'Toshkent' bo'ldi
   (DEFAULT 'Toshkent')

3. Ustunlar tartibini o'zgartirish mumkin:
   INSERT INTO mijozlar (shahar, ism, telefon) VALUES (...)
   — muhimi ustun nomi va qiymat mos kelishi
```

### 11.2. READ — SELECT (Ma'lumot o'qish)

SELECT — eng ko'p ishlatiladigan SQL buyrug'i. U ma'lumotni o'qish, filtrlash, tartiblash va guruhlash imkonini beradi.

```sql
-- Barcha ma'lumotlarni ko'rish
SELECT * FROM mijozlar;

-- Faqat kerakli ustunlarni ko'rish
SELECT ism, shahar FROM mijozlar;

-- Filtrlash (WHERE)
SELECT * FROM mijozlar WHERE shahar = 'Toshkent';

-- Natija:
-- ┌────┬─────────┬────────────────┬──────────┐
-- │ id │  ism    │   telefon      │  shahar  │
-- ├────┼─────────┼────────────────┼──────────┤
-- │  1 │ Otabek  │ +998901234567  │ Toshkent │
-- │  4 │ Nilufar │ +998977778899  │ Toshkent │
-- └────┴─────────┴────────────────┴──────────┘

-- Bir nechta shart (AND / OR)
SELECT * FROM mijozlar 
WHERE shahar = 'Toshkent' AND status = 'active';

SELECT * FROM mijozlar 
WHERE shahar = 'Toshkent' OR shahar = 'Samarqand';

-- Tartiblash (ORDER BY)
SELECT * FROM mijozlar ORDER BY ism;           -- A dan Z gacha
SELECT * FROM mijozlar ORDER BY ism DESC;      -- Z dan A gacha
SELECT * FROM mijozlar ORDER BY tugilgan_sana;  -- Kattadan kichikka

-- Cheklash (LIMIT)
SELECT * FROM mijozlar ORDER BY id LIMIT 3;     -- Faqat dastlabki 3 ta
SELECT * FROM mijozlar ORDER BY id LIMIT 3 OFFSET 2; -- 3-chi dan boshlab 3 ta

-- Hisoblash
SELECT COUNT(*) AS jami FROM mijozlar;                         -- Jami nechta
SELECT COUNT(*) AS toshkentliklar FROM mijozlar 
WHERE shahar = 'Toshkent';                                     -- Toshkentliklar

-- Guruhlash (GROUP BY)
SELECT shahar, COUNT(*) AS soni 
FROM mijozlar 
GROUP BY shahar 
ORDER BY soni DESC;

-- Natija:
-- ┌───────────┬──────┐
-- │  shahar   │ soni │
-- ├───────────┼──────┤
-- │ Toshkent  │    2 │
-- │ Samarqand │    1 │
-- │ Buxoro    │    1 │
-- │ Namangan  │    1 │
-- └───────────┴──────┘

-- O'xshash qidiruv (LIKE)
SELECT * FROM mijozlar WHERE ism LIKE 'O%';    -- O bilan boshlanadi
SELECT * FROM mijozlar WHERE ism LIKE '%r';    -- r bilan tugaydi
SELECT * FROM mijozlar WHERE ism LIKE '%adi%'; -- ichida 'adi' bor
SELECT * FROM mijozlar WHERE ism ILIKE '%OTABEK%'; -- Katta-kichik harf farq qilmaydi

-- Oraliq qidiruv (BETWEEN)
SELECT * FROM mijozlar 
WHERE tugilgan_sana BETWEEN '1995-01-01' AND '2000-12-31';

-- Ro'yxatda tekshirish (IN)
SELECT * FROM mijozlar 
WHERE shahar IN ('Toshkent', 'Samarqand', 'Buxoro');

-- NULL tekshirish
SELECT * FROM mijozlar WHERE telefon IS NULL;      -- Telefoni yo'q
SELECT * FROM mijozlar WHERE telefon IS NOT NULL;   -- Telefoni bor
```

### 11.3. UPDATE — Ma'lumot yangilash

```sql
-- Bitta satrni yangilash
UPDATE mijozlar
SET shahar = 'Samarqand'
WHERE id = 1;
-- Otabekning shahri endi Samarqand

-- Bir nechta ustunni yangilash
UPDATE mijozlar
SET shahar = 'Xorazm',
    status = 'inactive',
    telefon = '+998901111111'
WHERE id = 3;

-- Shartli yangilash
UPDATE mijozlar
SET status = 'inactive'
WHERE tugilgan_sana < '1995-01-01';
-- 1995-dan oldin tug'ilgan barcha mijozlar inactive bo'ldi

-- Hisoblash bilan yangilash
-- Masalan: mahsulotlar jadvalida barcha narxlarga 10% qo'shish
-- UPDATE mahsulotlar SET narx = narx * 1.10;
```

**⚠️ DIQQAT — Eng muhim qoida:**

```
┌──────────────────────────────────────────────────────────────┐
│                    ⚠️  OGOHLANTIRISH  ⚠️                     │
│                                                              │
│   WHERE ni HECH QACHON unutmang!                             │
│                                                              │
│   ❌ UPDATE mijozlar SET status = 'inactive';                │
│      — BARCHA mijozlar inactive bo'ladi!                     │
│                                                              │
│   ✅ UPDATE mijozlar SET status = 'inactive' WHERE id = 1;   │
│      — Faqat id=1 bo'lgan mijoz inactive bo'ladi             │
│                                                              │
│   Har doim avval SELECT bilan tekshiring:                    │
│   SELECT * FROM mijozlar WHERE id = 1;                       │
│   — keyin UPDATE yozing                                      │
└──────────────────────────────────────────────────────────────┘
```

### 11.4. DELETE — Ma'lumot o'chirish

```sql
-- Bitta yozuvni o'chirish
DELETE FROM mijozlar WHERE id = 1;

-- Shartli o'chirish
DELETE FROM mijozlar WHERE status = 'inactive';

-- Sana bo'yicha o'chirish
DELETE FROM mijozlar WHERE tugilgan_sana < '1990-01-01';
```

**⚠️ DIQQAT — Eng xavfli buyruq:**

```
┌──────────────────────────────────────────────────────────────┐
│                   🔴 XAVF ZONASI  🔴                         │
│                                                              │
│   ❌ DELETE FROM mijozlar;                                   │
│      — BARCHA ma'lumotlar O'CHADI!                           │
│      — Bu buyruqni ortga qaytarib bo'lmaydi!                 │
│                                                              │
│   Oltin qoida:                                               │
│   1. Avval SELECT bilan tekshiring nima o'chishini           │
│   2. Keyin DELETE yozing                                     │
│                                                              │
│   SELECT * FROM mijozlar WHERE status = 'inactive';          │
│   -- 3 ta yozuv chiqdi, hammasi to'g'ri                     │
│                                                              │
│   DELETE FROM mijozlar WHERE status = 'inactive';            │
│   -- Endi xotirjam o'chirishingiz mumkin                     │
└──────────────────────────────────────────────────────────────┘
```

### 11.5. DROP TABLE — Jadvalni butunlay o'chirish

```sql
-- Jadvalni o'chirish
DROP TABLE mijozlar;
-- DIQQAT: Jadval va undagi BARCHA ma'lumotlar yo'qoladi!
-- Bu buyruqni ORTGA QAYTARIB BO'LMAYDI!

-- Xavfsizroq variant:
DROP TABLE IF EXISTS mijozlar;
-- Agar jadval mavjud bo'lmasa, xatolik bermaydi
```

### DELETE va DROP farqi

```
┌──────────────────────────────────────────────────────────────┐
│            DELETE                 vs          DROP            │
├──────────────────────────────────────────────────────────────┤
│                                                              │
│  DELETE FROM mijozlar;          DROP TABLE mijozlar;          │
│                                                              │
│  Ma'lumotlar o'chadi           Jadval O'ZI o'chadi           │
│  Jadval qoladi                 Ma'lumotlar ham o'chadi       │
│  Yangi ma'lumot kiritish       Jadval qaytadan yaratish      │
│  mumkin                        kerak                         │
│                                                              │
│  Misol: Daftarni tozalash      Misol: Daftarni yoqish       │
└──────────────────────────────────────────────────────────────┘
```

---

## 12. Amaliy loyiha: "Kutubxona tizimi"

Keling, hamma o'rgangan bilimlarimizni bitta amaliy loyihada qo'llaymiz — **kutubxona boshqaruv tizimi** yaratamiz.

### Tizim tuzilmasi

```
┌─────────────────────────────────────────────────────────────┐
│                  KUTUBXONA TIZIMI                             │
│                                                              │
│  ┌─────────────┐    ┌─────────────┐    ┌─────────────┐      │
│  │  mualliflar  │    │   kitoblar   │    │  a'zolar     │     │
│  │             │◄───│             │    │             │      │
│  │ id          │    │ id          │    │ id          │      │
│  │ ism         │    │ nomi        │    │ ism         │      │
│  │ mamlakat    │    │ muallif_id──┘    │ telefon     │      │
│  └─────────────┘    │ yili        │    │ manzil      │      │
│                     │ janr        │    └──────┬──────┘      │
│                     │ mavjud_soni │           │             │
│                     └──────┬──────┘           │             │
│                            │                  │             │
│                     ┌──────▼──────────────────▼──────┐      │
│                     │         ijaralar                │      │
│                     │                                │      │
│                     │ id                             │      │
│                     │ kitob_id ──► kitoblar.id       │      │
│                     │ azo_id   ──► azolar.id         │      │
│                     │ olingan_sana                   │      │
│                     │ qaytarish_sana                 │      │
│                     │ qaytarildi (boolean)           │      │
│                     └────────────────────────────────┘      │
└─────────────────────────────────────────────────────────────┘
```

### Jadvallarni yaratish

```sql
-- 1. Mualliflar jadvali
CREATE TABLE mualliflar (
    id SERIAL PRIMARY KEY,
    ism TEXT NOT NULL,
    familiya TEXT NOT NULL,
    mamlakat VARCHAR(100) DEFAULT 'Nomalum',
    tugilgan_yili INTEGER CHECK (tugilgan_yili > 0 AND tugilgan_yili <= EXTRACT(YEAR FROM NOW())),
    yaratilgan_vaqt TIMESTAMPTZ DEFAULT NOW()
);

-- 2. Kitoblar jadvali
CREATE TABLE kitoblar (
    id SERIAL PRIMARY KEY,
    nomi TEXT NOT NULL,
    muallif_id INTEGER NOT NULL REFERENCES mualliflar(id),
    nashr_yili INTEGER CHECK (nashr_yili > 0),
    janr VARCHAR(50) NOT NULL,
    sahifalar_soni INTEGER CHECK (sahifalar_soni > 0),
    mavjud_soni INTEGER DEFAULT 1 CHECK (mavjud_soni >= 0),
    til VARCHAR(50) DEFAULT 'O''zbek',
    isbn VARCHAR(20) UNIQUE,
    yaratilgan_vaqt TIMESTAMPTZ DEFAULT NOW()
);

-- 3. A'zolar (kutubxona foydalanuvchilari)
CREATE TABLE azolar (
    id SERIAL PRIMARY KEY,
    ism TEXT NOT NULL,
    familiya TEXT NOT NULL,
    telefon VARCHAR(20) UNIQUE NOT NULL,
    email VARCHAR(255) UNIQUE,
    manzil TEXT,
    tugilgan_sana DATE CHECK (tugilgan_sana < CURRENT_DATE),
    azolik_sanasi DATE DEFAULT CURRENT_DATE,
    faol BOOLEAN DEFAULT TRUE,
    yaratilgan_vaqt TIMESTAMPTZ DEFAULT NOW()
);

-- 4. Ijaralar jadvali
CREATE TABLE ijaralar (
    id SERIAL PRIMARY KEY,
    kitob_id INTEGER NOT NULL REFERENCES kitoblar(id),
    azo_id INTEGER NOT NULL REFERENCES azolar(id),
    olingan_sana DATE DEFAULT CURRENT_DATE NOT NULL,
    qaytarish_sana DATE NOT NULL,
    qaytarildi BOOLEAN DEFAULT FALSE,
    haqiqiy_qaytarish DATE,
    jarima NUMERIC(10,2) DEFAULT 0 CHECK (jarima >= 0),
    yaratilgan_vaqt TIMESTAMPTZ DEFAULT NOW(),

    -- Qaytarish sanasi olish sanasidan keyin bo'lishi kerak
    CHECK (qaytarish_sana > olingan_sana),
    -- Haqiqiy qaytarish sanasi olish sanasidan keyin bo'lishi kerak
    CHECK (haqiqiy_qaytarish IS NULL OR haqiqiy_qaytarish >= olingan_sana)
);
```

### Ma'lumotlar kiritish

```sql
-- Mualliflar
INSERT INTO mualliflar (ism, familiya, mamlakat, tugilgan_yili) VALUES
    ('Alisher', 'Navoiy', 'O''zbekiston', 1441),
    ('Abdulla', 'Qodiriy', 'O''zbekiston', 1894),
    ('Cho''lpon', 'Abdulhamid Sulaymon', 'O''zbekiston', 1897),
    ('Lev', 'Tolstoy', 'Rossiya', 1828),
    ('Fyodor', 'Dostoyevskiy', 'Rossiya', 1821);

-- Kitoblar
INSERT INTO kitoblar (nomi, muallif_id, nashr_yili, janr, sahifalar_soni, mavjud_soni, isbn) VALUES
    ('Xamsa', 1, 1483, 'She''riyat', 650, 3, '978-9943-00-001'),
    ('O''tkan kunlar', 2, 1925, 'Roman', 320, 5, '978-9943-00-002'),
    ('Kecha va kunduz', 3, 1936, 'Roman', 280, 2, '978-9943-00-003'),
    ('Urush va tinchlik', 4, 1869, 'Roman', 1225, 4, '978-9943-00-004'),
    ('Jinoyat va jazo', 5, 1866, 'Roman', 671, 3, '978-9943-00-005'),
    ('Mehrobdan chayon', 2, 1929, 'Roman', 200, 2, '978-9943-00-006');

-- A'zolar
INSERT INTO azolar (ism, familiya, telefon, email, manzil, tugilgan_sana) VALUES
    ('Sardor', 'Karimov', '+998901234567', 'sardor@mail.com', 'Toshkent, Chilonzor', '2000-03-15'),
    ('Dilnoza', 'Rahimova', '+998911112233', 'dilnoza@mail.com', 'Samarqand, Registon', '1999-07-22'),
    ('Bekzod', 'Tursunov', '+998934445566', 'bekzod@mail.com', 'Buxoro, Markaz', '2001-11-08'),
    ('Malika', 'Umarova', '+998977778899', 'malika@mail.com', 'Toshkent, Yunusobod', '1998-05-30');

-- Ijaralar
INSERT INTO ijaralar (kitob_id, azo_id, olingan_sana, qaytarish_sana) VALUES
    (2, 1, '2025-06-01', '2025-06-15'),   -- Sardor "O'tkan kunlar" oldi
    (4, 2, '2025-06-03', '2025-06-17'),   -- Dilnoza "Urush va tinchlik" oldi
    (5, 1, '2025-06-05', '2025-06-19'),   -- Sardor "Jinoyat va jazo" oldi
    (1, 3, '2025-06-07', '2025-06-21'),   -- Bekzod "Xamsa" oldi
    (3, 4, '2025-06-10', '2025-06-24');   -- Malika "Kecha va kunduz" oldi
```

### So'rovlar — hayotiy savollar

```sql
-- 1. Barcha kitoblar va ularning muallifi
SELECT 
    k.nomi AS kitob, 
    m.ism || ' ' || m.familiya AS muallif,
    k.nashr_yili,
    k.janr
FROM kitoblar k
JOIN mualliflar m ON k.muallif_id = m.id
ORDER BY k.nashr_yili;

-- Natija:
-- ┌────────────────────┬──────────────────────────┬───────────┬──────────┐
-- │ kitob              │ muallif                  │ nashr_yili│ janr     │
-- ├────────────────────┼──────────────────────────┼───────────┼──────────┤
-- │ Xamsa              │ Alisher Navoiy           │ 1483      │ She'riyat│
-- │ Jinoyat va jazo    │ Fyodor Dostoyevskiy      │ 1866      │ Roman    │
-- │ Urush va tinchlik  │ Lev Tolstoy              │ 1869      │ Roman    │
-- │ O'tkan kunlar      │ Abdulla Qodiriy          │ 1925      │ Roman    │
-- │ Mehrobdan chayon   │ Abdulla Qodiriy          │ 1929      │ Roman    │
-- │ Kecha va kunduz    │ Cho'lpon A. S.           │ 1936      │ Roman    │
-- └────────────────────┴──────────────────────────┴───────────┴──────────┘

-- 2. Hozir kimda qaysi kitob bor? (qaytarilmaganlar)
SELECT 
    a.ism || ' ' || a.familiya AS azo,
    k.nomi AS kitob,
    i.olingan_sana,
    i.qaytarish_sana,
    CASE 
        WHEN i.qaytarish_sana < CURRENT_DATE THEN 'MUDDATI O''TDI!'
        ELSE 'Muddatida'
    END AS holat
FROM ijaralar i
JOIN azolar a ON i.azo_id = a.id
JOIN kitoblar k ON i.kitob_id = k.id
WHERE i.qaytarildi = FALSE
ORDER BY i.qaytarish_sana;

-- 3. Har bir muallif nechta kitob yozgan?
SELECT 
    m.ism || ' ' || m.familiya AS muallif,
    COUNT(k.id) AS kitoblar_soni
FROM mualliflar m
LEFT JOIN kitoblar k ON m.id = k.muallif_id
GROUP BY m.id, m.ism, m.familiya
ORDER BY kitoblar_soni DESC;

-- 4. Eng faol o'quvchi kim? (eng ko'p kitob olgan)
SELECT 
    a.ism || ' ' || a.familiya AS azo,
    COUNT(i.id) AS olingan_kitoblar
FROM azolar a
JOIN ijaralar i ON a.id = i.azo_id
GROUP BY a.id, a.ism, a.familiya
ORDER BY olingan_kitoblar DESC
LIMIT 1;

-- 5. Kitob qaytarish
UPDATE ijaralar
SET qaytarildi = TRUE,
    haqiqiy_qaytarish = CURRENT_DATE
WHERE id = 1;
-- Sardor "O'tkan kunlar"ni qaytardi

-- 6. Muddati o'tgan ijaralar uchun jarima hisoblash (har kun uchun 5000 so'm)
UPDATE ijaralar
SET jarima = (CURRENT_DATE - qaytarish_sana) * 5000
WHERE qaytarildi = FALSE 
  AND qaytarish_sana < CURRENT_DATE;
```

---

## 13. Xatolar va ularning yechimlari

### Eng ko'p uchraydigan xatolar

```
┌──────────────────────────────────────────────────────────────────────┐
│ XATO                              │ SABAB VA YECHIM                 │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│ ERROR: relation "jadval" does     │ Jadval mavjud emas.              │
│ not exist                         │ Jadval nomini tekshiring.        │
│                                   │ \dt buyrug'i bilan ro'yxatni    │
│                                   │ ko'ring.                        │
│                                                                      │
│ ERROR: null value in column       │ NOT NULL ustun bo'sh             │
│ "ism" violates not-null           │ qoldirildi. Qiymat kiriting.    │
│ constraint                        │                                  │
│                                                                      │
│ ERROR: duplicate key value        │ UNIQUE ustunda takroriy qiymat.  │
│ violates unique constraint        │ Boshqa qiymat ishlating yoki    │
│                                   │ mavjudini yangilang.            │
│                                                                      │
│ ERROR: insert or update           │ FOREIGN KEY xatosi.              │
│ violates foreign key constraint   │ Havola qilingan jadvalda bunday │
│                                   │ id mavjud emas.                 │
│                                                                      │
│ ERROR: new row violates           │ CHECK chekloviga zid qiymat     │
│ check constraint                  │ kiritildi. Qiymatni tekshiring. │
│                                                                      │
│ ERROR: invalid input syntax       │ Noto'g'ri ma'lumot turi.        │
│ for type integer: "salom"         │ INTEGER ustuniga matn            │
│                                   │ kiritmoqchi bo'ldingiz.         │
│                                                                      │
│ ERROR: column "nom" does not      │ Ustun nomi noto'g'ri.           │
│ exist                             │ \d jadval_nomi bilan            │
│                                   │ ustunlarni tekshiring.          │
│                                                                      │
│ ERROR: syntax error at or near    │ SQL sintaksisi xato.            │
│ "FORM"                            │ To'g'risi: FROM (F-R-O-M)      │
└──────────────────────────────────────────────────────────────────────┘
```

### Xatolarni oldini olish bo'yicha maslahatlar

```
1. HAR DOIM avval SELECT, keyin UPDATE/DELETE
   — O'zgartirmoqchi bo'lgan ma'lumotni avval ko'ring

2. WHERE ni HECH QACHON unutmang
   — UPDATE va DELETE da WHERE yo'q bo'lsa, BARCHA satrlar ta'sirlanadi

3. Tranzaksiyalardan foydalaning
   — BEGIN ... COMMIT / ROLLBACK

4. Zaxira nusxa (backup) oling
   — pg_dump baza_nomi > zaxira.sql

5. Muhit (environment) ajrating
   — Development, Staging, Production alohida bazalar bo'lsin
```

### Tranzaksiyalar — xavfsiz o'zgartirish

```sql
-- Tranzaksiya boshlash
BEGIN;

-- O'zgartirishlar qilish
UPDATE mijozlar SET shahar = 'Andijon' WHERE id = 1;
DELETE FROM mijozlar WHERE id = 5;

-- Natijani tekshirish
SELECT * FROM mijozlar;

-- Agar hamma narsa to'g'ri bo'lsa:
COMMIT;   -- O'zgartirishlarni saqlash

-- Agar xato bo'lsa:
ROLLBACK; -- Barcha o'zgartirishlarni BEKOR qilish
          -- Xuddi hech narsa bo'lmagandek!
```

**Hayotiy misol — bank o'tkazmasi:**

```sql
BEGIN;

-- Ali hisobidan 500,000 so'm yechish
UPDATE hisoblar SET balans = balans - 500000 WHERE id = 1;

-- Tekshirish: balans manfiy bo'lib qolmadimi?
-- (CHECK constraint ham tekshiradi, lekin qo'shimcha nazorat)

-- Vali hisobiga 500,000 so'm qo'shish
UPDATE hisoblar SET balans = balans + 500000 WHERE id = 2;

-- Agar ikki qadam ham muvaffaq bo'lsa:
COMMIT;

-- Agar biror xato bo'lsa (masalan, Vali hisobi mavjud emas):
-- PostgreSQL avtomatik ROLLBACK qiladi
-- Ali hisobidan pul yechilmaydi!
```

---

## 14. Eng muhim qoidalar va maslahatlar

### Nomlash qoidalari

```
✅ YAXSHI AMALIYOT:

  Jadval nomi:    ko'plik, kichik harf, pastki chiziq
                  mijozlar, buyurtmalar, kitob_kategoriyalari

  Ustun nomi:     birlik, kichik harf, pastki chiziq
                  ism, yaratilgan_vaqt, telefon_raqam

  Primary key:    id (yoki jadval_nomi_id)
  Foreign key:    bog'langan_jadval_id
                  muallif_id, kategoriya_id

❌ YOMON AMALIYOT:

  Jadval nomi:    Mijozlar, MIJOZLAR, tbl_customers
  Ustun nomi:     FirstName, TELEFON, fld_name
  Katta harf:     MYCUSTOMERS (PostgreSQL kichik harfga o'giradi)
```

### Xavfsizlik bo'yicha eng muhim 10 qoida

```
 1. WHERE ni hech qachon unutmang (UPDATE/DELETE da)
 2. Har doim avval SELECT, keyin UPDATE/DELETE
 3. Tranzaksiyalar (BEGIN/COMMIT/ROLLBACK) ishlating
 4. Zaxira nusxa (backup) muntazam oling
 5. Foydalanuvchilarga minimal ruxsat bering (GRANT)
 6. Parollarni bazada HECH QACHON ochiq saqlaman (hash ishlating)
 7. SQL injection dan himoyalaning (parametrli so'rovlar)
 8. Production bazada to'g'ridan-to'g'ri ishlang
 9. Indekslar (INDEX) qo'ying — tezlik uchun
10. Loglar (pg_log) ni kuzatib boring
```

### Indekslar — tezlik sirri

```sql
-- Indeks nima?
-- Xuddi kitob oxiridagi "Mundarija" kabi.
-- U qidiruvni tezlashtiradi.

-- Indeks yaratish
CREATE INDEX idx_mijozlar_shahar ON mijozlar(shahar);
CREATE INDEX idx_mijozlar_ism ON mijozlar(ism);

-- Endi bu so'rov ANCHA tezroq ishlaydi:
SELECT * FROM mijozlar WHERE shahar = 'Toshkent';
-- Indekssiz: barcha 1,000,000 satrni ko'radi
-- Indeks bilan: to'g'ridan-to'g'ri kerakli satrlarga boradi

-- Qachon indeks kerak?
--   ✅ WHERE da tez-tez ishlatiladigan ustunlar
--   ✅ JOIN da ishlatiladigan ustunlar
--   ✅ ORDER BY da ishlatiladigan ustunlar
--   ❌ Juda kichik jadvallarda (100 dan kam satr)
--   ❌ Tez-tez yangilanadigan ustunlarda (indeks ham yangilanadi)
```

---

## 15. Xulosa

```
┌──────────────────────────────────────────────────────────────────┐
│                     NIMALARNI O'RGANDIK?                         │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. TARIX                                                        │
│     Qo'lyozmadan SQL, NoSQL va Vector DB gacha bo'lgan yo'l     │
│                                                                  │
│  2. DB TURLARI                                                   │
│     SQL (qat'iy, ishonchli) vs NoSQL (tez, moslashuvchan)       │
│     vs Vector DB (AI, semantik qidiruv)                         │
│                                                                  │
│  3. PostgreSQL                                                   │
│     Dunyodagi eng kuchli ochiq kodli relyatsion baza            │
│                                                                  │
│  4. JADVAL TUZILMASI                                             │
│     Ustunlar (nima?), satrlar (kim?), yacheykalar (qiymat)      │
│                                                                  │
│  5. DATA TYPES                                                   │
│     TEXT, INTEGER, NUMERIC, DATE, BOOLEAN, JSON, ARRAY           │
│     Narx uchun NUMERIC, sana uchun TIMESTAMPTZ                  │
│                                                                  │
│  6. CONSTRAINTS                                                  │
│     PRIMARY KEY, UNIQUE, NOT NULL, CHECK, DEFAULT, FOREIGN KEY  │
│     Baza o'zini himoya qilishi uchun                            │
│                                                                  │
│  7. CRUD                                                         │
│     INSERT (qo'shish), SELECT (o'qish),                         │
│     UPDATE (yangilash), DELETE (o'chirish)                       │
│                                                                  │
│  8. XAVFSIZLIK                                                   │
│     WHERE ni unutmang, tranzaksiyalar ishlating,                │
│     zaxira nusxa oling                                          │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

### Keyingi qadamlar

Bu qo'llanmani o'zlashtirganingizdan so'ng, quyidagi mavzularni o'rganishni davom eting:

```
Bosqich 2 (O'rta daraja):
  — JOIN turlari (INNER, LEFT, RIGHT, FULL, CROSS)
  — Subqueries (ichki so'rovlar)
  — Views (ko'rinishlar)
  — Stored Functions va Procedures
  — Triggers (avtomatik harakatlar)

Bosqich 3 (Ilg'or daraja):
  — Indekslar chuqurroq (B-tree, Hash, GIN, GiST)
  — Query optimization (so'rovlarni optimallashtirish)
  — Partitioning (jadvalni bo'lish)
  — Replication (nusxalash)
  — Connection pooling (PgBouncer)
  — Monitoring va performance tuning

Bosqich 4 (Mutaxassis daraja):
  — PostgreSQL extensions (PostGIS, pgvector, TimescaleDB)
  — Logical va physical replication
  — High Availability (yuqori mavjudlik)
  — Disaster Recovery (falokatdan tiklash)
  — Cloud PostgreSQL (AWS RDS, Azure, GCP)
```

---

> **"Yaxshi baza — o'zini himoya qila oladigan baza.**
> **Yaxshi dasturchi — bazani himoya qila oladigan dasturchi."**

---

*Bu qo'llanma PostgreSQL 16+ versiyasiga asoslangan. Rasmiy hujjatlar: [postgresql.org/docs](https://www.postgresql.org/docs/)*
