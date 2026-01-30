# Reactive Resume - Nasadenie na Vercel a Supabase

Tento návod vám ukáže, ako nasadiť Reactive Resume na Vercel s Supabase ako databázou a úložiskom.

## 🚀 Rýchly štart

### Predpoklady
- Účet na [Vercel](https://vercel.com/signup) (zadarmo)
- Účet na [Supabase](https://supabase.com) (zadarmo)
- Váš GitHub repozitár s Reactive Resume

### Krok 1: Nastavenie Supabase

1. **Vytvorte nový projekt na Supabase:**
   - Prihláste sa na [supabase.com](https://supabase.com)
   - Kliknite na "New project"
   - Vyplňte údaje projektu a poznačte si heslo databázy
   - Počkajte, kým sa projekt vytvorí (1-2 minúty)

2. **Získajte connection string databázy:**
   - Prejdite na Settings → Database
   - Skopírujte URI connection string
   - Vyzerá takto: `postgresql://postgres:[HESLO]@db.xxxxx.supabase.co:5432/postgres`
   - Nahraďte `[HESLO]` vaším heslom databázy

3. **Vytvorte Storage bucket:**
   - Prejdite na Storage v dashboarde
   - Kliknite "Create a new bucket"
   - Názov: `reactive-resume`
   - Nechajte bucket privátny (Public bucket: vypnuté)

4. **Získajte S3 prihlasovacie údaje:**
   - Settings → API
   - Poznačte si:
     - Project URL
     - Project reference ID (v URL alebo Settings → General)
     - Service role key (pod Project API keys → service_role)

### Krok 2: Nastavenie Printer service

Pre generovanie PDF potrebujete headless Chrome prehliadač. Odporúčame:

**Browserless Cloud (najjednoduchšie):**
1. Zaregistrujte sa na [browserless.io](https://www.browserless.io/)
2. Vytvorte API kľúč
3. Váš endpoint bude: `wss://chrome.browserless.io?token=VÁŠ_API_KĽÚČ`
4. Free verzia: 6 hodín mesačne zadarmo

### Krok 3: Nasadenie na Vercel

1. **Importujte repozitár:**
   - Prihláste sa na [vercel.com](https://vercel.com)
   - Kliknite "Add New..." → "Project"
   - Importujte váš Reactive Resume repozitár z GitHubu

2. **Nastavte build settings:**
   - Framework Preset: Vite alebo Other
   - Build Command: `pnpm install && pnpm run build`
   - Install Command: `pnpm install`

3. **Pridajte environment variables:**
   
   V Settings → Environment Variables pridajte:

   **Povinné premenné:**
   ```bash
   # URL aplikácie (aktualizujte po prvom nasadení)
   APP_URL=https://vasa-aplikacia.vercel.app
   
   # Databáza (zo Supabase)
   DATABASE_URL=postgresql://postgres:HESLO@db.xxxxx.supabase.co:5432/postgres
   
   # Printer service
   PRINTER_ENDPOINT=wss://chrome.browserless.io?token=VÁŠ_TOKEN
   PRINTER_APP_URL=https://vasa-aplikacia.vercel.app
   
   # Autentifikácia (vygenerujte: openssl rand -hex 32)
   AUTH_SECRET=váš-vygenerovaný-tajný-kľúč
   ```

   **Supabase Storage (S3):**
   ```bash
   S3_ACCESS_KEY_ID=vase-supabase-project-id
   S3_SECRET_ACCESS_KEY=vase-supabase-service-role-key
   S3_REGION=auto
   S3_ENDPOINT=https://xxxxx.supabase.co/storage/v1/s3
   S3_BUCKET=reactive-resume
   S3_FORCE_PATH_STYLE=false
   ```

   **Voliteľné - Email (SMTP):**
   ```bash
   SMTP_HOST=smtp.gmail.com
   SMTP_PORT=587
   SMTP_USER=vas-email@gmail.com
   SMTP_PASS=vaše-app-heslo
   SMTP_FROM=Reactive Resume <noreply@vasadomena.com>
   SMTP_SECURE=false
   ```

   **Voliteľné - Social auth:**
   ```bash
   GOOGLE_CLIENT_ID=
   GOOGLE_CLIENT_SECRET=
   GITHUB_CLIENT_ID=
   GITHUB_CLIENT_SECRET=
   ```

4. **Nasaďte:**
   - Kliknite "Deploy"
   - Počkajte na dokončenie buildu
   - Získate URL ako `https://vasa-aplikacia.vercel.app`
   - Aktualizujte `APP_URL` s reálnou URL
   - Znovu nasaďte

### Krok 4: Overenie

Po nasadení skontrolujte:
1. Navštívte `https://vasa-aplikacia.vercel.app/api/health`
2. Vytvorte účet
3. Vytvorte životopis
4. Skúste export do PDF

## 📝 Kompletná dokumentácia

Detailnú dokumentáciu nájdete tu:
- [Vercel + Supabase Deployment Guide](./vercel-supabase.mdx)
- [Príklad .env súboru](./.env.vercel.example)

## 💰 Náklady

Oba služby majú štedrý free tier:

**Vercel Free:**
- 100 GB bandwidth/mesiac
- 6,000 build minút/mesiac
- Neobmedzené nasadenia
- 10-sekundový timeout (Pro: 60s)

**Supabase Free:**
- 500 MB databáza
- 1 GB file storage
- 50,000 aktívnych užívateľov/mesiac
- Pozastaví sa po 7 dňoch nečinnosti

**Browserless Free:**
- 6 hodín/mesiac zadarmo

## 🔧 Riešenie problémov

### Databáza sa nepripojí
- Skontrolujte `DATABASE_URL` - obsahuje správne heslo?
- Supabase databáza beží? (zelený status v dashboarde)

### PDF export nefunguje
- Je `PRINTER_ENDPOINT` správne nastavený?
- Browserless service beží?
- Máte ešte kredit? (free tier: 6h/mesiac)

### Upload súborov nefunguje
- Bucket existuje v Supabase Storage?
- Všetky `S3_*` premenné sú nastavené?
- Používate `service_role` key, nie `anon` key?

### Prihlásenie nefunguje
- `APP_URL` presne zodpovedá vašej URL?
- Vygenerovali ste nový `AUTH_SECRET`?
- Vyčistite cookies a skúste znova

## 🆘 Podpora

Ak potrebujete pomoc:
- [GitHub Issues](https://github.com/amruthpillai/reactive-resume/issues)
- [Discord komunita](https://discord.gg/hzwkZbyvUW)
- [Vercel dokumentácia](https://vercel.com/docs)
- [Supabase dokumentácia](https://supabase.com/docs)

## 🔐 Bezpečnostné odporúčania

✅ Použite silný `AUTH_SECRET` (32+ znakov)
✅ Nikdy nezdieľajte `service_role` key
✅ Používajte HTTPS (Vercel to robí automaticky)
✅ Uchovávajte všetky tajomstvá v environment variables
✅ Pravidelne aktualizujte závislosti
✅ Monitorujte logy v Vercel a Supabase

---

**Poznámka:** Pre produkčné nasadenie zvážte Pro plány pre vyššie limity a lepšiu podporu.
