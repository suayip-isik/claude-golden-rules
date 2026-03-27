# Claude Code — Altın Kurallar ve Maliyet Optimizasyonu

Her dosya türü için dikkat edilmesi gereken kurallar, yaygın hatalar ve maliyet optimizasyonu.

---

## 1. CLAUDE.md — Projenin Anayasası

### Altın Kurallar

**Yalnızca evrensel bilgileri yaz.** CLAUDE.md her oturum başında, her mesajda context'e yüklenir. İçindeki her satır çarpan etkisi yaratır — 50 mesajlık bir oturumda her satır 50 kez okunur.

**Kısa tut.** Anthropic resmi olarak ~500 satırı üst sınır önerir. Pratikte 100-200 satır idealdir. Araştırmalar, LLM'lerin ~150-200 talimatı tutarlı takip edebildiğini gösterir; Claude Code'un kendi sistem prompt'u zaten ~50 talimat içerir.

**Linter'ın işini yaptırma.** "2 space indent kullan", "noktalı virgül koy" gibi kurallar CLAUDE.md'ye yazılmamalı. ESLint/Prettier/Stylelint bu işi daha güvenilir ve bedavaya yapar. Claude bu kuralları %100 takip edemez, linter eder.

**Açıklamayı atla.** "TypeScript strict mode kullan" yeterli. Neden strict mode kullanılması gerektiğini açıklama — Claude zaten biliyor.

**Negatif kurallar pozitiflerden etkili.** "YAPMA" ve "ASLA" talimatları daha iyi takip edilir. Yasaklar bölümünü ayrı tut ve kısa yaz.

**Spesifik talimatları taşı.** Yalnızca belirli dosya türleriyle çalışırken geçerli olan kuralları `rules/` altına taşı. CLAUDE.md'de tutma.

### Yapısı

```markdown
# Proje Adı
1-2 cümle açıklama

## Tech Stack
5-8 satır

## Dizin Yapısı
10-15 satır — ana klasörler

## Komutlar
8-12 satır — sık kullanılanlar

## Temel Kurallar
10-15 madde — yalnızca HER görevde geçerli olanlar

## Yasaklar
5-10 madde — ASLA yapılmaması gerekenler
```

### Yaygın Hatalar

| Hata | Neden Sorun | Çözüm |
|---|---|---|
| 500+ satır, her şey bir arada | Talimat takip kalitesi düşer, token israfı | Rules/skills'e taşı |
| Kod örnekleri yazma | Context şişer, Claude zaten biliyor | Sadece kuralı yaz |
| Linter kuralları yazma | %100 takip edilmez, israf | `.eslintrc` kullan |
| İçindekiler tablosu, emoji başlıklar | Gereksiz token tüketimi | Düz markdown |
| Aynı kuralı farklı yerlerde tekrarlama | Context kirliliği + tutarsızlık riski | Tek kaynak prensibi |

---

## 2. Rules — Koşullu Yüklenen Kurallar

### Altın Kurallar

**Path-scoping kullan.** Frontmatter'da `paths:` belirt — kural yalnızca ilgili dosyalarla çalışırken yüklenir. Bu, CSS kurallarının JS dosyası düzenlerken context'ten yer yememesini sağlar.

```yaml
---
paths:
  - "assets/css/**/*.css"
---
```

**Her rule dosyası tek konuya odaklansın.** `security.md`, `performance.md`, `code-style.md` gibi ayrı dosyalar. Tek devasa `all-rules.md` YAPMA.

**30-60 satır idealdir, 80'i aşmasın.** Aşarsa bölünmesi gereken bir işaret.

**Talimatı kural olarak yaz, açıklama olarak değil.** "X yap" veya "X yapma" formatı. Paragrafla anlatım yapma.

**Çakışma olmasın.** Aynı kural birden fazla dosyada bulunmamalı. CLAUDE.md'de kısa versiyon, rule dosyasında detaylı versiyon gibi çifte kayıt YAPMA.

### Path-Scoped vs Global

| Durum | Path-Scoped | Global (path yok) |
|---|---|---|
| CSS kuralları | ✅ `assets/css/**` | ❌ |
| JS kuralları | ✅ `assets/js/**` | ❌ |
| Git workflow | ❌ | ✅ Her zaman geçerli |
| Workflow (plan-first) | ❌ | ✅ Her zaman geçerli |
| Güvenlik | ✅ `*.html`, `assets/js/**` | ❌ |

### Yaygın Hatalar

| Hata | Çözüm |
|---|---|
| Path-scoping kullanmamak | Her rule dosyasına ilgili path'leri ekle |
| CLAUDE.md'deki kuralları rule'da tekrarlamak | Birinden sil, tek yerde tut |
| 100+ satır rule dosyası | Bölmeyi düşün veya detayları kısalt |
| Kod snippet'leri uzun uzun yazmak | Tek satır kural yeterli, Claude pattern'i codebase'den öğrenir |

---

## 3. Commands — Manuel Tetiklenen İş Akışları

### Altın Kurallar

**`$ARGUMENTS` kullan.** Komutun parametrik olmasını sağlar. `/review index.html` gibi dosya belirtme imkanı verir.

**`allowed-tools` ile yetki sınırla.** Read-only review komutu için `Write`, `Edit` verme. Düzeltme yapacaksa ver.

**Çok fazla command oluşturma.** Bu bir anti-pattern. Shrivu Shankar (Anthropic mühendisi): "Eğer uzun bir custom slash command listeniz varsa, bir anti-pattern yaratmışsınız. Asıl amaç, neredeyse ne isterseniz yazıp yararlı sonuç almaktır."

**Side-effect'li command'larda `disable-model-invocation: true` kullan.** Deploy, release gibi tehlikeli işlemlerde Claude'un kendi kendine tetiklememesini sağla.

```yaml
---
description: Production'a deploy et
disable-model-invocation: true
allowed-tools: Bash
---
```

**Kısa ve odaklı tut.** 20-50 satır ideal. Command, Claude'a "ne yapacağını" söyler — "neden"i açıklamana gerek yok.

### Ne Zaman Command, Ne Zaman Başka Bir Şey

| İhtiyaç | Araç |
|---|---|
| "Her seferinde aynı adımları çalıştır" | Command |
| "Claude otomatik karar versin" | Agent veya Skill |
| "Template + script gerekli" | Skill |
| "Her dosya düzenlemesinde geçerli" | Rule |
| "Her oturumda geçerli" | CLAUDE.md |

### Yaygın Hatalar

| Hata | Çözüm |
|---|---|
| 10+ command oluşturmak | 3-5 temel command yeterli |
| Her command'a tüm tool'ları vermek | Minimum yetki prensibi |
| Command içinde uzun açıklamalar | Kısa talimat, Claude gerisini bilir |

---

## 4. Skills — On-Demand Yüklenen Uzmanlıklar

### Altın Kurallar

**SKILL.md kısa olsun, destek dosyaları ayrı olsun.** SKILL.md talimatları içerir, büyük referans dökümanları ayrı dosyalarda tutulur. Claude gerektiğinde ayrı dosyaları okur.

```
skills/new-article/
├── SKILL.md              # Talimatlar (50-150 satır)
├── templates/
│   └── article.html      # Template (Claude gerektiğinde okur)
└── references/
    └── schema-examples.json
```

**`description` alanı kesin ve spesifik olsun.** Claude, skill'i tetiklemek için bu açıklamayı kullanır. Belirsiz açıklama = yanlış tetikleme veya hiç tetiklememe.

```yaml
# ❌ Kötü
description: Kod ile ilgili şeyler yapar

# ✅ İyi  
description: Yeni HTML makale sayfası oluşturur. Schema.org JSON-LD, SEO meta tag'ler ve responsive görseller dahil.
```

**Skill name küçük harf, kısa çizgi ile.** `new-article`, `code-review` formatında.

**Her skill tek bir iş yapsın.** "Her şeyi yapan" skill yerine birden fazla odaklı skill oluştur.

**Script'leri skill klasörüne koy.** Claude, SKILL.md içindeki talimatla bu script'leri çalıştırır.

### Ne Zaman Skill, Ne Zaman Command

| Skill | Command |
|---|---|
| Claude otomatik tetikleyebilir | Yalnızca sen tetiklersin |
| Template/script/referans dosyaları var | Tek markdown dosyası yeterli |
| Birden fazla adımlı, zengin iş akışı | Basit checklist veya tek adım |
| Context'e on-demand yüklenir | Çağrıldığında yüklenir |

### Yaygın Hatalar

| Hata | Çözüm |
|---|---|
| SKILL.md içine dev referans yazmak | Ayrı dosyaya koy, SKILL.md'den referans ver |
| Belirsiz description | Spesifik yaz, tetikleme keyword'lerini dahil et |
| Çok fazla skill tanımlamak | Her skill metadata'sı context tüketir (~100 token) |

---

## 5. Agents — Ayrı Context'te Çalışan Uzmanlar

### Altın Kurallar

**Agent'ı okuma/analiz görevi için kullan.** Agent'lar ayrı context window'da çalışır ve sonucu ana oturuma döndürür. Bu, ana context'i temiz tutar. Dosya araştırma, codebase keşfi, review gibi görevlerde idealdir.

**Tool'ları minimumda tut.** Agent'a ihtiyaç duyduğu minimum tool setini ver.

```yaml
# ❌ Kötü — gereksiz yetki
tools: Read, Write, Edit, Bash, Grep, Glob, WebSearch, WebFetch

# ✅ İyi — sadece gerekli
tools: Read, Grep, Glob
```

**Agent isimlerinde dikkatli ol.** Claude Code bazı isimlere varsayılan davranışlar yükler. `code-reviewer` gibi yaygın isimler, senin talimatlarını gölgeleyebilir.

```yaml
# ⚠️ Dikkat — Claude varsayılan "code-reviewer" davranışı uygulayabilir
name: code-reviewer

# ✅ Daha güvenli — özel isim
name: cr-medical
name: tech-reviewer-v2
```

**Model seçimini belirt.** Review/analiz için `sonnet` yeterli ve ucuz. Mimari kararlar için `opus`.

```yaml
model: sonnet   # Review, analiz, arama — hızlı ve ucuz
model: opus     # Planlama, mimari, karmaşık karar
model: haiku    # Basit kontrol, format doğrulama — en ucuz
```

**Description'ı spesifik yaz.** Claude, agent'a delege etme kararını description'a göre verir.

### Ne Zaman Agent, Ne Zaman Command

| Agent | Command |
|---|---|
| Çok dosya okuması gerekli (context koruma) | Tek dosya veya bilinen dosya seti |
| Claude otomatik delege edebilmeli | Sen manuel tetiklemelisin |
| Sonuç özeti yeterli (detay ana context'e girmemeli) | Tüm detayı görmek istiyorsun |
| Paralel çalıştırılabilir | Sıralı adımlar |

### Yaygın Hatalar

| Hata | Çözüm |
|---|---|
| Uygulama yapan agent oluşturmak | Agent okusun/analiz etsin, uygulamayı ana oturum yapsın |
| Tüm tool'ları vermek | Minimum yetki: Read, Grep, Glob yeterli |
| Yaygın isim kullanmak | Özel, projeye özgü isim ver |
| Agent'a CLAUDE.md kurallarını tekrar yazmak | Agent kendi system prompt'unu alır, CLAUDE.md almaz |

---

## 6. settings.json — İzinler ve Otomasyonlar

### Altın Kurallar

**allow listesini daralt.** Sık kullandığın komutları izin ver, geri kalanı Claude her seferinde sorar (güvenlik).

```json
{
  "permissions": {
    "allow": [
      "Bash(git status)",
      "Bash(git diff *)",
      "Bash(npx eslint *)",
      "Bash(npx stylelint *)"
    ],
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force*)"
    ]
  }
}
```

**Auto-compact eşiğini düşür.** Varsayılan %95 çok geç. %60 daha temiz context sağlar.

```json
{
  "env": {
    "CLAUDE_AUTOCOMPACT_PCT_OVERRIDE": "60"
  }
}
```

**Thinking token bütçesini ayarla.** Varsayılan 32K token. Basit görevler için 8-10K yeterli.

```json
{
  "env": {
    "MAX_THINKING_TOKENS": "10000"
  }
}
```

**Hooks ile deterministik kontrol koy.** Linter'ı her düzenlemeden sonra otomatik çalıştır, test çıktısını filtrele, main branch'e yazmayı engelle.

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npx eslint --fix $(echo $TOOL_INPUT | jq -r '.path') 2>/dev/null || true"
          }
        ]
      }
    ]
  }
}
```

---

## 7. Maliyet Optimizasyonu — Token = Para

### Temel Prensip

Context window'daki her token her mesajda tekrar gönderilir. 50 mesajlık oturumda:
- CLAUDE.md 200 satır = ~4K token × 50 = **200K token** sadece CLAUDE.md için
- CLAUDE.md 2000 satır = ~40K token × 50 = **2M token** — 10 kat fark

### En Yüksek Etkili Stratejiler

| Strateji | Tahmini Tasarruf | Açıklama |
|---|---|---|
| **Görev arası `/clear`** | %30-50 | Farklı göreve geçerken context temizle |
| **CLAUDE.md'yi 200 satıra indir** | %20-30 | Rules/skills'e taşı |
| **Spesifik prompt yaz** | %15-25 | `@dosya.js satır 45-60` referans ver |
| **Model seçimi** | %30-80 maliyet | Haiku basit iş, Sonnet normal, Opus kritik |
| **Plan Mode (Shift+Tab×2)** | %20-40 | Koddan önce planla, rework'ü önle |
| **Path-scoped rules** | %10-20 | İlgisiz kurallar yüklenmesin |
| **Skills'e taşı** | %10-15 | On-demand yükleme |
| **MCP server temizliği** | %5-30 | Kullanılmayanları kaldır |

### Oturum Yönetimi

```bash
# Her görev değişiminde
/clear

# 30-45 dakikada bir veya context %60-70'e ulaşınca
/compact

# Token durumunu izle
/cost

# Context kullanımını gör
/context
```

### Model Stratejisi

```bash
# Plan mode'da Opus, kodlamada Sonnet (en verimli)
# settings.json'da veya /model ile ayarla

claude --model haiku    # Basit: typo, format, tek satır değişiklik
claude --model sonnet   # Normal: implementasyon, refactoring
claude --model opus     # Kritik: mimari, planlama, karmaşık debug
```

Agent'larda da model belirt:
```yaml
model: haiku    # Format kontrol, basit analiz
model: sonnet   # Code review, codebase araştırma
model: opus     # Mimari karar, karmaşık planlama
```

### Prompt Kalitesi = Token Tasarrufu

```bash
# ❌ Pahalı — belirsiz, keşif gerektirir
> Koda bak ve iyileştir

# ✅ Ucuz — hedefli, doğrudan sonuç
> @assets/js/main.js satır 45-60 loadArticles fonksiyonunda
> gereksiz DOM sorgusunu değişkene al, try/catch ekle

# ❌ Pahalı — toplu dosya okuma
> src/ klasöründeki tüm dosyaları oku ve özetle

# ✅ Ucuz — hedefli okuma
> @components/header.js ve @components/footer.js nav yapısını karşılaştır
```

### Anti-Pattern'ler

| Anti-Pattern | Neden Sorun | Doğrusu |
|---|---|---|
| Her şeyi CLAUDE.md'ye yazmak | Her mesajda tüm tokenlar yüklenir | Rules + skills'e dağıt |
| 10+ MCP server bağlamak | Tool tanımları context'i doldurur | Sadece kullanılanları tut |
| 10+ custom command | Karmaşıklık, bakım zorluğu | 3-5 temel command |
| Agent'a tüm tool'ları vermek | Gereksiz context, güvenlik riski | Minimum yetki |
| Compact'ı geciktirmek | Context dolunca kalite düşer | %60'ta compact |
| Her görevde Opus | 5x maliyet, çoğu görev için gereksiz | Haiku/Sonnet/Opus hibrit |
| Belirsiz prompt yazmak | Keşif tokeni harcanır | `@dosya:satır` referansı |
| Aynı oturumda farklı görevler | Context kirliliği, karışan bağlam | `/clear` ile görev ayır |
| CLAUDE.md'de kod örnekleri | Gereksiz token | Kural yaz, Claude pattern'i codebase'den öğrenir |
| Compaction sonrası kaybolan tercihler | Aynı hata tekrar tekrar | CLAUDE.md'ye kritik uyarıları yaz |

---

## 8. Genel Mimari Kararlar

### Dosya Türü Seçim Matrisi

```
"Bu bilgi HER görevde mi geçerli?"
├── Evet → CLAUDE.md
└── Hayır → "Belirli dosya türleriyle mi ilgili?"
    ├── Evet → rules/ (path-scoped)
    └── Hayır → "Ben mi tetikleyeceğim?"
        ├── Evet → commands/
        └── Hayır → "Zengin template/script gerekli mi?"
            ├── Evet → skills/
            └── Hayır → "Ayrı context'te çalışmalı mı?"
                ├── Evet → agents/
                └── Hayır → rules/ (global)
```

### Başlangıç Stratejisi

**Gün 1 — Minimum:**
- `CLAUDE.md` (100-200 satır)
- `.claude/rules/workflow.md` (plan-first akış)

**Hafta 1 — Temel genişleme:**
- `.claude/rules/code-style.md` (path-scoped)
- `.claude/settings.json` (izinler + compact ayarı)
- 1-2 command (en sık yaptığın tekrarlı iş)

**Hafta 2+ — İhtiyaç duydukça:**
- Tekrar eden sorunlar → ilgili rule ekle
- Sık yapılan zengin iş → skill oluştur
- Context kirliliği yaratan görev → agent ekle

**Asla:** Tüm yapıyı ilk günden doldurmaya çalışma. Kullanılmayan dosyalar context tüketir.

### Her Şeyin Özeti

| Dosya | Yükleme Anı | İdeal Boyut | Maliyet Etkisi |
|---|---|---|---|
| CLAUDE.md | Her mesajda | 100-200 satır | EN YÜKSEK — her satır çarpan |
| rules/ (path-scoped) | İlgili dosya açıldığında | 30-60 satır | ORTA — koşullu |
| rules/ (global) | Her mesajda | 30-50 satır | YÜKSEK — CLAUDE.md gibi |
| commands/ | `/komut` ile | 20-50 satır | DÜŞÜK — tek seferlik |
| skills/ | İhtiyaç duyulduğunda | 50-150 satır | DÜŞÜK — on-demand |
| agents/ | Delege edildiğinde | 30-80 satır | DÜŞÜK — ayrı context |
| settings.json | Oturum başında | — | YOK — config |
