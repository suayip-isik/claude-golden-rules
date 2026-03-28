# Claude Code — Verimli Kullanım Rehberi

> Geliştiriciler için pratik bir başvuru kaynağı: dosya türü seçimi, maliyet optimizasyonu, güvenlik, hata yönetimi ve takım workflow'u.

---

## Bu Nedir?

Bu rehber, Claude Code ile günlük çalışırken en çok önem taşıyan pattern'leri, anti-pattern'leri ve mimari kararları bir araya getiriyor. Temel bilgilere zaten sahip olan ve daha fazla token harcamadan ya da sorun yaratmadan kurulumdan daha iyi verim almak isteyen geliştiriciler için yazılmıştır.

Rehber; altı temel dosya türünü (`CLAUDE.md`, `rules/`, `commands/`, `skills/`, `agents/`, `settings.json`) ve resmi dokümantasyonun büyük ölçüde deneme yanılmaya bıraktığı yedi operasyonel konuyu kapsıyor.

---

## İçindekiler

| #   | Konu                                           |
| --- | ---------------------------------------------- |
| 1   | [CLAUDE.md — Projenin Anayasası](#)            |
| 2   | [Rules — Koşullu Yüklenen Kurallar](#)         |
| 3   | [Commands — Manuel Tetiklenen İş Akışları](#)  |
| 4   | [Skills — On-Demand Yüklenen Uzmanlıklar](#)   |
| 5   | [Agents — Ayrı Context'te Çalışan Uzmanlar](#) |
| 6   | [settings.json — İzinler ve Otomasyonlar](#)   |
| 7   | [Güvenlik — Secrets ve İzin Yönetimi](#)       |
| 8   | [Hata Recovery ve Checkpoint Stratejisi](#)    |
| 9   | [Debugging ve Troubleshooting](#)              |
| 10  | [Test ve Doğrulama](#)                         |
| 11  | [Versiyonlama ve Takım Workflow'u](#)          |
| 12  | [Çakışma Çözüm Kuralları](#)                   |
| 13  | [Maliyet Optimizasyonu — Token = Para](#)      |
| 14  | [Genel Mimari Kararlar](#)                     |
| 15  | [Kaynaklar ve Referanslar](#)                  |

---

## Hızlı Başlangıç

Beş dakikan varsa şu üç şeyle başla:

**1. CLAUDE.md'yi 200 satırın altında tut.**
Her satır her mesajda yüklenir. 50 mesajlık bir oturumda 2.000 satırlık bir CLAUDE.md, 200 satırlıktan 10 kat daha fazla token harcar — üstelik ~150 kuraldan sonra talimat takip kalitesi de düşer.

**2. Path-scoped rule kullan.**
TypeScript dosyası düzenlerken CSS kuralları yüklenmemeli. Her rule dosyasına `paths:` frontmatter ekle:

```yaml
---
paths:
  - "src/styles/**/*.css"
---
```

**3. İlk günden temel bir deny listesi ekle.**
Claude tehlikeli bir şeye dokunmadan önce `settings.json`'da kilitle:

```json
{
  "permissions": {
    "deny": [
      "Bash(rm -rf *)",
      "Bash(git push --force*)",
      "Bash(git reset --hard *)",
      "Bash(curl * | bash)"
    ]
  }
}
```

---

## Dosya Türü Seçim Matrisi

Bir şeyin nereye ait olduğundan emin değil misin? Bu ağacı kullan:

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

---

## Maliyete Hızlı Bakış

| Strateji                                 | Tahmini Tasarruf |
| ---------------------------------------- | ---------------- |
| Görevler arası `/clear`                  | %30–50           |
| CLAUDE.md'yi 200 satıra indir            | %20–30           |
| Hedefli prompt yaz (`@dosya.ts:45-60`)   | %15–25           |
| Doğru modeli seç (Haiku / Sonnet / Opus) | %30–80           |
| Kodlamadan önce Plan Mode                | %20–40           |
| Path-scoped rules                        | %10–20           |
| Kullanılmayan MCP server'ları kaldır     | %5–30            |

---

## Bu Repodaki Dosyalar

```
├── CLAUDE.md                                  # Bu repo için Claude Code rehberi
├── claude-code-efficiency-guide.md            # Tam rehber (İngilizce)
├── claude-code-verimli-kullanim-rehberi.md    # Tam rehber (Türkçe)
├── README.md                                  # İngilizce README
└── README.tr.md                               # Bu dosya (Türkçe)
```

---

## Kimin İçin

- Claude Code'u zaten kullanan ve daha yalın, daha hızlı bir kurulum isteyen geliştiriciler
- Yeni ekip üyelerini Claude Code workflow'una dahil eden takımlar
- Context limitine takılanlar, beklenmedik maliyetlerle karşılaşanlar ya da kuralların neden takip edilmediğini anlamak isteyenler

Bu rehber; genel prompt mühendisliğini, Claude.ai web arayüzünü veya doğrudan Anthropic API'sini **kapsamaz**.

---

## Katkıda Bulunma

Güncel olmayan, hatalı veya eksik bir şey mi buldun? Pull request'ler memnuniyetle karşılanır.

Claude Code hızla gelişiyor — yeni bir sürümdeki davranış değişikliği rehberin bir bölümünü geçersiz kılarsa, lütfen bir issue aç veya düzeltme öner.

---

## Lisans

[MIT](LICENSE) — serbestçe kullanabilir, uyarlayabilir ve paylaşabilirsin.

---

## Teşekkürler

- Shrivu Shankar (Anthropic mühendisi) — slash command anti-pattern üzerine görüşü
- Anthropic'in resmi Claude Code dokümantasyonu — bu rehberin üzerine inşa edildiği asıl kaynak
