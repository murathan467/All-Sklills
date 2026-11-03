# Antigravity (AGY) Agent Orkestrasyon Paketi

**(Graphify + Addy Osmani Agent-Skills + Ponytail)**

Bu klasör (`agent-kuralları`), bir projede yapay zeka asistanının **en az kodla, sıfır kod tekrarıyla, mevcut mimariyi bozmadan ve donanım/güvenlik sınırlarına sadık kalarak** çalışmasını sağlayan tüm kuralları, becerileri (`skills`), alt ajanları (`subagents`) ve iş akışlarını (`workflows`) tek bir pakette toplar.

---

## 📁 1. Klasör İçeriği ve Yapısı

```text
agent-kuralları/
├── AGENTS.md                 # Ana Orkestrasyon Kural Dosyası (Proje kök dizinine konur)
├── README.md                 # Bu kullanım kılavuzu
├── .agents/                  # Herhangi bir projeye doğrudan kopyalanmaya hazır gizli klasör
│   ├── rules/                # Her oturumda otomatik yüklenen kurallar (graphify.md, ponytail.md)
│   ├── skills/               # 32 adet uzmanlık becerisi (Graphify + Addy Skills + Ponytail)
│   ├── agents/               # 4 adet özel alt ajan (Subagent) tanımı
│   └── workflows/            # Otomatik iş akışları (graphify.md)
```

---

## 🧠 2. Üçlü Sistem Nasıl Çalışır? (Kim Ne İş Yapar?)

Bu sistemde 3 farklı araç birbiriyle çakışmadan **4 aşamalı** bir sıra ile çalışır:

| Aşama                              | Yöneten Sistem                  | Görevi                                                                                                                                                                                                                                                                                     |
| :--------------------------------- | :------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Keşif & Bağlam**              | **`Graphify`**                  | Kod yazmadan veya dosyaları rastgele okumadan önce projenin bilgi grafiğini (`graphify-out/graph.json`) sorgular. Mevcut yardımcı fonksiyonları bulur, **aynı kodun ikinci kez yazılmasını engeller**.                                                                                     |
| **2. Analiz & Planlama**           | **`addyosmani/agent-skills`**   | İhtiyaç belirsizse sorular sorar (`@interview-me`), büyük işleri küçük ve güvenli adımlara böler (`@planning-and-task-breakdown`).                                                                                                                                                         |
| **3. Kod Yazımı (Diff Disiplini)** | **`Ponytail` (`full` mod)**     | **"Tembel Kıdemli Yazılımcı"** modudur. Gereksiz wrapper, interface, ekstra kütüphane ve "belki ileride lazım olur" kodlarını yasaklar. **Ancak projenin mevcut mimari katmanlarına (Clean Architecture, UseCase, Repository, MVVM) dokunmaz**, sadeleştirmeyi katmanların _içinde_ yapar. |
| **4. Test & Doğrulama**            | **`agent-skills` + `Ponytail`** | İş mantığı ve bug çözümlerinde TDD (`@test-driven-development`), son kontrolde ise `/ponytail-review` ve `@code-review-and-quality` çalıştırır.                                                                                                                                            |

---

## 🛠️ 3. İçerideki Beceriler (Skills) ve Alt Ajanlar (Subagents)

### A. Ponytail Becerileri (6 Adet)

- **`ponytail`**: Ana minimal kod yazım modu (`lite`, `full` [varsayılan], `ultra`).
- **`ponytail-review`**: Yazılan kodu inceler; gereksiz satırları, yük altında patlayacak yerleri ve güvenlik açıklarını bulur.
- **`ponytail-audit`**: Tüm repoyu tarar; silinebilecek ölü kodları ve şişkinlikleri raporlar.
- **`ponytail-debt`**: Kod içindeki `shortcut:` etiketli teknik borçları listeler.
- **`ponytail-gain`**: Ponytail'in sağladığı kod ve maliyet tasarrufunu gösterir.
- **`ponytail-help`**: Hızlı komut referansı.

### B. Graphify Becerisi (1 Adet)

- **`graphify`**: Projeyi AST (Tree-sitter) ile analiz edip `graphify-out/` altında bilgi grafiği (`graph.json`, `graph.html`, `GRAPH_REPORT.md`) oluşturur ve sorgular.

### C. Addy Osmani Mühendislik Becerileri (25 Adet)

En sık kullanacaklarınız:

- **`@interview-me`**: Belirsiz bir fikri kodlamadan önce size tek tek soru sorarak netleştirir.
- **`@spec-driven-development`**: Koddan önce net bir teknik şartname (Spec/PRD) çıkarır.
- **`@planning-and-task-breakdown`**: Büyük bir özelliği küçük, test edilebilir görevlere böler.
- **`@incremental-implementation`**: Kodu tek seferde 1000 satır yazmak yerine ince, doğrulanabilir dilimler halinde yazar.
- **`@test-driven-development`**: Önce testi yazar (Red), sonra kodu geçirir (Green), sonra sadeleştirir (Refactor).
- **`@debugging-and-error-recovery`**: Rastgele deneme-yanılma yapmak yerine kök neden analiziyle bug çözer.
- **`@code-review-and-quality`** & **`@security-and-hardening`**: Çok eksenli kod ve güvenlik denetimi yapar.
- _(Diğerleri: `api-and-interface-design`, `frontend-ui-engineering`, `performance-optimization`, `git-workflow-and-versioning`, `ci-cd-and-automation`, `deprecation-and-migration`, `documentation-and-adrs`, `doubt-driven-development`, `idea-refine`, `observability-and-instrumentation`, `shipping-and-launch`, `source-driven-development`, `constraint-driven-development`)_

### D. Alt Ajanlar / Subagents (`agents/` altında 4 Adet)

- **`code-reviewer.md`**: Bağımsız kod inceleme uzmanı.
- **`security-auditor.md`**: Güvenlik ve yetkilendirme denetçisi.
- **`test-engineer.md`**: Birim ve entegrasyon testi uzmanı.
- **`web-performance-auditor.md`**: Performans ve darboğaz denetçisi.

---

## 💬 4. Günlük Kullanımda Nasıl Komut Verilir? (Kopyala-Yapıştır Şablonları)

Antigravity sohbet ekranında eski `/plan` veya `/build` kısayolları yerine doğrudan **`@beceri-adı`** etiketini kullanın:

### Senaryo 1: Yeni Bir Özellik Eklerken (En Verimli Kalıp)

> **Komut:**  
> _"Şu özelliği eklemek istiyorum: [Özelliği yazın]. Önce Graphify ile projede benzer yapı var mı kontrol et, ardından `@planning-and-task-breakdown` ile planı çıkar ve Ponytail kurallarıyla `@incremental-implementation` olarak uygula."_

### Senaryo 2: Fikir Henüz Tam Net Değilken

> **Komut:**  
> _"Şöyle bir değişiklik düşünüyorum ama detaylardan emin değilim. `@interview-me` becerisini kullanarak beni sorgula, gereksinimleri birlikte netleştirelim."_

### Senaryo 3: Karmaşık Bir Hatayı (Bug) Çözerken

> **Komut:**  
> _"Şu loglarda/ekranda hata alıyorum: [Hata/Log]. `@debugging-and-error-recovery` ile kök nedeni bul, Ponytail kurallarıyla en az kodu değiştirerek çöz ve `@test-driven-development` ile doğrulama testini ekle."_

### Senaryo 4: Yazılan Kodu Kontrol Ettirirken

> **Komut:**  
> _"Son yaptığımız değişiklikleri `/ponytail-review` ve `@code-review-and-quality` ile incele; gereksiz kod veya risk var mı raporla."_

---

## 📊 5. Graphify ile Bağlantıları ve Kodu İzleme Komutları

Terminal üzerinden projenin mimari bağlantılarını görmek için:

1. **Görsel Haritayı Tarayıcıda Açma:**
   ```bash
   xdg-open graphify-out/graph.html
   ```
2. **Bir Sınıfın Tüm Bağlantılarını (Kim Çağırıyor / Kimi Çağırıyor) Görme:**
   ```bash
   graphify explain "UploadFirmwareUseCaseImpl"
   ```
3. **İki Sınıf Arasındaki Çağrı Zincirini Bulma:**
   ```bash
   graphify path "BootloaderViewModel" "DartBootloaderClientImpl"
   ```
4. **Kavram veya Özellik Sorgulama:**
   ```bash
   graphify query "USB cooldown ve hat sessizliği nasıl yönetiliyor?"
   ```
5. **Canlı İzleme (Siz kod yazdıkça grafiği sıfır maliyetle günceller):**
   ```bash
   graphify watch .
   ```

---

## 🚀 6. Bu Paketi Başka Bir Projeye Nasıl Kurarsınız?

Yeni bir projeye geçtiğinizde bu klasördeki her şeyi **10 saniyede** yeni projenize taşıyabilirsiniz:

```bash
# 1. AGENTS.md ve .agents klasörünü yeni projenin kök dizinine kopyalayın:
cp agent-kuralları/AGENTS.md /yeni/proje/yolu/
cp -r agent-kuralları/.agents /yeni/proje/yolu/

# 2. Yeni projede ilk bilgi grafiğini oluşturun ve git hook'u aktif edin:
cd /yeni/proje/yolu/
graphify update .
graphify hook install
```

Artık yeni projenizde de Antigravity açıldığı anda tüm bu kuralları ve becerileri otomatik olarak tanıyacaktır.
