# Antigravity (AGY) Çoklu Mimari Agent Orkestrasyon Paketleri

**(Graphify + Addy Osmani Agent-Skills + Ponytail + Stack Uzmanlık Paketleri)**

Bu klasör (`agent-kuralları`), bir projede yapay zeka asistanının **en az kodla, sıfır kod tekrarıyla, mevcut mimariyi bozmadan ve donanım/güvenlik sınırlarına sadık kalarak** çalışmasını sağlayan tüm kuralları, becerileri (`skills`), alt ajanları (`subagents`) ve iş akışlarını (`workflows`) barındırır.

Farklı teknolojilerde geliştirme yaparken **bağlam şişmesini (context bloat)** ve **mimari kural çakışmalarını** %100 önlemek için **4 izole profil** halinde yapılandırılmıştır.

---

## 📁 1. Klasör İçeriği ve Yapısı (4 İzole Profil)

```text
agent-kuralları/
├── README.md                  # Bu ana kullanım ve kurulum kılavuzu
├── dotnet-backend/            # .NET / C# / ASP.NET Core / EF Core Paketi (25 Beceri, 3 Alt Ajan)
│   ├── AGENTS.md              # .NET & Clean Architecture / CQRS özel orkestrasyon kuralları
│   └── .agents/               # rules/, skills/, agents/, workflows/
├── kotlin-compose/            # Android Jetpack Compose & KMP Paketi (29 Beceri, 3 Alt Ajan)
│   ├── AGENTS.md              # Compose Route/Screen & MVI özel orkestrasyon kuralları
│   └── .agents/               # rules/, skills/, agents/, workflows/
├── flutter-mobile/            # Flutter & Dart Paketi (26 Beceri, 3 Alt Ajan)
│   ├── AGENTS.md              # Flutter const Widget & Bloc/Riverpod özel orkestrasyon kuralları
│   └── .agents/               # rules/, skills/, agents/, workflows/
└── react-nextjs/              # React 19 & Next.js App Router Paketi (24 Beceri, 4 Alt Ajan)
    ├── AGENTS.md              # RSC / Client Component & Web UI özel orkestrasyon kuralları
    └── .agents/               # rules/, skills/, agents/, workflows/
```

---

## 🧠 2. Sistem Nasıl Çalışır? (Kim Ne İş Yapar?)

Bu sistemde tüm araçlar birbiriyle çakışmadan **4 aşamalı** bir sıra ile çalışır:

| Aşama                              | Yöneten Sistem                            | Görevi                                                                                                                                                                                                                                                                                                 |
| :--------------------------------- | :---------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Keşif & Bağlam**              | **`Graphify`**                            | Kod yazmadan veya dosyaları rastgele okumadan önce projenin bilgi grafiğini (`graphify-out/graph.json`) sorgular. Mevcut yardımcı fonksiyonları bulur, **aynı kodun ikinci kez yazılmasını engeller**.                                                                                                 |
| **2. Analiz, Tasarım & Planlama**  | **`addyosmani/agent-skills` + `ui-ux`**   | İhtiyaç belirsizse sorular sorar (`@interview-me`), büyük işleri küçük ve güvenli adımlara böler (`@planning-and-task-breakdown`), arayüzlerde tasarım sistemini (`@ui-ux-pro-max`) kurar.                                                                                                             |
| **3. Kod Yazımı (Diff Disiplini)** | **`Ponytail` + Stack Uzmanlık Becerileri** | **"Tembel Kıdemli Yazılımcı"** modudur. Gereksiz wrapper, interface, ekstra kütüphane ve "belki ileride lazım olur" kodlarını yasaklar. **Ancak projenin mevcut mimari katmanlarına (Clean Architecture, CQRS, MVI, `Route`/`Screen`, `const Widget`, RSC) dokunmaz**, sadeleştirmeyi katmanların _içinde_ yapar. |
| **4. Test, Güvenlik & Doğrulama**  | **`agent-skills` + `Ponytail` + Ajanlar** | İş mantığı ve bug çözümlerinde TDD (`@test-driven-development`), güvenlikte `@security-and-hardening`, son kontrolde ise `/ponytail-review`, `/ponytail-audit` ve `@code-review-and-quality` çalıştırır.                                                                                               |

---

## 🛠️ 3. Tüm Profillerde Bulunan Ortak Çekirdek Beceriler ve Alt Ajanlar

### A. Ponytail Becerileri (6 Adet — Tüm Profillerde Ortak)

- **`ponytail`**: Ana minimal kod yazım modu (`lite`, `full` [varsayılan], `ultra`).
- **`ponytail-review`**: Yazılan kodu inceler; gereksiz satırları, yük altında patlayacak yerleri ve güvenlik açıklarını bulur.
- **`ponytail-audit`**: Tüm repoyu tarar; bug'ları, güvenlik deliklerini, gerçek yük altında kırılacak noktaları, testsiz riskli kodları ve silinebilecek ölü kodları önem sırasına göre raporlar.
- **`ponytail-debt`**: Kod içindeki `shortcut:` etiketli teknik borçları listeler.
- **`ponytail-gain`**: Ponytail'in sağladığı kod ve maliyet tasarrufunu gösterir.
- **`ponytail-help`**: Hızlı komut referansı.

### B. Graphify Becerisi (1 Adet — Tüm Profillerde Ortak)

- **`graphify`**: Projeyi AST (Tree-sitter) ile analiz edip `graphify-out/` altında bilgi grafiği (`graph.json`, `graph.html`, `GRAPH_REPORT.md`) oluşturur ve sorgular.

### C. Addy Osmani Mühendislik Becerileri (11 Çekirdek Adet — Tüm Profillerde Ortak)

- **`@interview-me`**: Belirsiz bir fikri kodlamadan önce size tek tek soru sorarak netleştirir.
- **`@spec-driven-development`**: Koddan önce net bir teknik şartname (Spec/PRD) çıkarır.
- **`@planning-and-task-breakdown`**: Büyük bir özelliği küçük, test edilebilir görevlere böler.
- **`@api-and-interface-design`**: Kararlı REST/GraphQL uç noktaları, modül sınırları ve tip sözleşmeleri tasarlar.
- **`@incremental-implementation`**: Kodu tek seferde 1000 satır yazmak yerine ince, doğrulanabilir dilimler halinde yazar.
- **`@test-driven-development`**: Önce testi yazar (Red), sonra kodu geçirir (Green), sonra sadeleştirir (Refactor).
- **`@debugging-and-error-recovery`**: Rastgele deneme-yanılma yapmak yerine kök neden analiziyle bug çözer.
- **`@code-review-and-quality`**: Doğruluk, okunabilirlik, mimari, güvenlik ve performans eksenlerinde kod denetimi yapar.
- **`@security-and-hardening`**: OWASP Top 10, kimlik doğrulama, girdi doğrulama, veri saklama ve bağımlılık güvenliği denetimi yapar.
- **`@performance-optimization`**: Veri tabanı (N+1), bellek, sorgu ve arayüz darboğazlarını tespit edip optimize eder.
- **`@git-workflow-and-versioning`**: Temiz atomik commit'ler, anlamlı commit mesajları, PR ve sürüm yönetimi sağlar.

### D. Alt Ajanlar / Subagents (`.agents/agents/` altında)

- **`code-reviewer.md`** *(Tüm profillerde)*: Bağımsız kıdemli kod inceleme uzmanı.
- **`security-auditor.md`** *(Tüm profillerde)*: Güvenlik açıkları, tehdit modellemesi ve yetkilendirme denetçisi.
- **`test-engineer.md`** *(Tüm profillerde)*: Birim, entegrasyon ve uç durum (edge-case) testi uzmanı.
- **`web-performance-auditor.md`** *(Sadece `react-nextjs` profilinde)*: Core Web Vitals (LCP, INP, CLS), bundle ve DOM/ağ darboğaz denetçisi.

---

## 🧩 4. Profillere Özel Uzmanlık Becerileri ve Çakışma Çözümleri

### 1. `dotnet-backend/` (.NET / C# / EF Core — +7 Uzmanlık Becerisi)
- **Beceriler:**
  - **`dotnet-webapi`**: ASP.NET Core Minimal API / Controller, OpenAPI ve global hata yönetimi.
  - **`optimizing-ef-core-queries`** & **`efcore-patterns`**: EF Core sorgu optimizasyonu, N+1 önleme, `AsNoTracking`, `DbContext` ömrü, query splitting ve migration yönetimi.
  - **`modern-csharp-coding-standards`**: C# 12+ (`record`, pattern matching, ValueObject, `Span<T>`/`Memory<T>`).
  - **`csharp-concurrency-patterns`**: `async/await`, `Channel<T>` ve lock'sız güvenli eşzamanlılık.
  - **`dependency-injection-patterns`**: Temiz `IServiceCollection` modüler DI kayıtları ve `IValidateOptions`.
  - **`analyzing-dotnet-performance`**: ~50 farklı .NET bellek, GC, LINQ ve async anti-pattern taraması.
- **Mimari Koruma:** Ponytail; Clean Architecture / Vertical Slice katmanlarını, MediatR CQRS (`Command`/`Query`/`Handler`/`Validator`) sınıflarını ve `Entity` ↔ `record DTO` ayrımlarını birleştiremez.

### 2. `kotlin-compose/` (Jetpack Compose & KMP — +11 Uzmanlık Becerisi)
- **Beceriler:**
  - **`compose`** (Ana karar ağacı) + 6 alt beceri: **`compose-ui`** (Material 3, `WindowSizeClass`, 20 Recomposition kuralı), **`compose-feature`** (`Route`/`Screen`, MVI `Contract.kt`, Navigation 3), **`compose-architecture`** (Clean Architecture, Hilt/Koin), **`compose-data`** (Room, DataStore, Ktor, DTO->Domain->UiModel), **`compose-project`** (`libs.versions.toml`, Convention Plugins), **`compose-platform`** (WorkManager, Glance, KMP `expect`/`actual`).
  - **`kotlin-tooling-agp9-migration`**, **`kotlin-tooling-java-to-kotlin`**, **`kotlin-tooling-native-build-performance`**.
  - **`ui-ux-pro-max`**: Renk paleti, tipografi ve `design-system/MASTER.md` tasarım motoru.
- **Mimari Koruma:** Ponytail `.kt` dosyalarında `Route` ve `Screen` composable'larını tek fonksiyonda birleştiremez, MVI sözleşmelerini ve `DTO -> Domain -> UiModel` dönüşümlerini silemez.

### 3. `flutter-mobile/` (Flutter & Dart — +8 Uzmanlık Becerisi)
- **Beceriler:**
  - **`flutter-apply-architecture-best-practices`** (UI / Logic / Data katmanlı mimari), **`flutter-build-responsive-layout`** (`LayoutBuilder`, `MediaQuery`), **`flutter-fix-layout-issues`** (`RenderFlex overflowed` çözümü), **`flutter-setup-declarative-routing`** (`go_router`), **`dart-use-pattern-matching`** (Dart 3 `sealed class` ve `switch`), **`dart-run-static-analysis`** (`dart analyze`), **`flutter-add-widget-test`**, **`ui-ux-pro-max`**.
- **Mimari Koruma:** Ponytail, `const` constructor'lı küçük `StatelessWidget` sınıflarını *"sadece bir yerden çağrılıyor"* diyerek tek bir dev `build()` metoduna gömemez (Flutter'ın rebuild optimizasyonunu korur).

### 4. `react-nextjs/` (React 19 & Next.js App Router — +6 Uzmanlık Becerisi)
- **Beceriler:**
  - **`vercel-react-best-practices`** (RSC, App Router, waterfall önleme, bundle küçültme), **`vercel-composition-patterns`** (Compound Components, React 19 API'leri), **`web-design-guidelines`** (Web UI denetimi), **`frontend-ui-engineering`** (DOM/CSS/WCAG mühendisliği), **`browser-testing-with-devtools`**, **`ui-ux-pro-max`**.
- **Mimari Koruma:** Ponytail, Server Component (`page.tsx`) ile Client Component (`'use client'`) dosyalarını tek dosyada birleştirip tüm sayfayı client bundle'a sızdıramaz.

---

## 💬 5. Günlük Kullanımda Nasıl Komut Verilir? (Kopyala-Yapıştır Şablonları)

Antigravity sohbet ekranında doğrudan **`@beceri-adı`** etiketini veya `/ponytail-*` komutlarını kullanın:

### Senaryo 1: Yeni Bir Özellik Eklerken (En Verimli Kalıp)
> **Komut:**  
> _"Şu özelliği eklemek istiyorum: [Özelliği yazın]. Önce Graphify ile projede benzer yapı var mı kontrol et, ardından `@planning-and-task-breakdown` ile planı çıkar ve Ponytail kurallarıyla `@incremental-implementation` olarak uygula."_

### Senaryo 2: Fikir Henüz Tam Net Değilken
> **Komut:**  
> _"Şöyle bir değişiklik düşünüyorum ama detaylardan emin değilim. `@interview-me` becerisini kullanarak beni sorgula, gereksinimleri birlikte netleştirelim."_

### Senaryo 3: Karmaşık Bir Hatayı (Bug) Çözerken
> **Komut:**  
> _"Şu loglarda/ekranda hata alıyorum: [Hata/Log]. `@debugging-and-error-recovery` ile kök nedeni bul, Graphify ile bu fonksiyonu çağıran yerleri kontrol et, Ponytail kurallarıyla en az kodu değiştirerek çöz ve `@test-driven-development` ile doğrulama testini ekle."_

### Senaryo 4: Yazılan Kodu İnceletirken (Code Review & Diff Kontrolü)
> **Komut:**  
> _"Son yaptığımız değişiklikleri `@ponytail-review` ve `@code-review-and-quality` ile incele; gereksiz kod, mimari ihlal veya yük altında patlayacak bir risk var mı raporla."_

### Senaryo 5: Tüm Projeyi Tarama (Bloat, Ölü Kod ve Mimari Denetim)
> **Komut:**  
> _"Tüm repoyu `@ponytail-audit` ile tara; gizli bug'ları, gerçek yük altında kırılacak noktaları, testsiz riskli kodları ve silinebilecek ölü/gereksiz kodları önem sırasına göre raporla."_

### Senaryo 6: Güvenlik ve Yetkilendirme Taraması (Security Audit)
> **Komut:**  
> _"Bu modüldeki/projedeki API uçlarını, kullanıcı girdilerini ve veri tabanı sorgularını `@security-and-hardening` becerisiyle ve `security-auditor` alt ajanıyla denetle; OWASP Top 10, injection veya yetki atlama açığı var mı raporla."_

### Senaryo 7: Performans ve Veri Tabanı Darboğaz Taraması
> **Komut (.NET için):**  
> _"Veri erişim katmanını `@optimizing-ef-core-queries` ve `@analyzing-dotnet-performance` ile tara; N+1 sorgularını, eksik `AsNoTracking` kullanımlarını ve bellek darboğazlarını bul."_  
> **Komut (Compose / Flutter / Next.js için):**  
> _"Bu ekranı `@performance-optimization` (ve Compose ise `@compose-ui`, Next.js ise `@vercel-react-best-practices`) ile incele; gereksiz recomposition/rebuild veya waterfall sorunlarını çöz."_

---

## 📊 6. Graphify ile Bağlantıları ve Kodu İzleme Komutları

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

## 🚀 7. Yeni Bir Projeye Hangi Paketi Nasıl Kurarsınız?

Geliştireceğiniz projenin teknolojisine göre ilgili klasörün içeriğini **10 saniyede** hedef projenizin kök dizinine kopyalayın:

```bash
# Seçenek A: .NET / C# Backend Projesi İçin
cp -r "/home/murathan_akkus/Desktop/New Folder/agent-kuralları/dotnet-backend/." /yeni/proje/yolu/

# Seçenek B: Kotlin / Jetpack Compose / KMP Projesi İçin
cp -r "/home/murathan_akkus/Desktop/New Folder/agent-kuralları/kotlin-compose/." /yeni/proje/yolu/

# Seçenek C: Flutter Projesi İçin
cp -r "/home/murathan_akkus/Desktop/New Folder/agent-kuralları/flutter-mobile/." /yeni/proje/yolu/

# Seçenek D: React / Next.js Projesi İçin
cp -r "/home/murathan_akkus/Desktop/New Folder/agent-kuralları/react-nextjs/." /yeni/proje/yolu/

# Ardından hedef projede ilk bilgi grafiğini oluşturun ve git hook'u aktif edin:
cd /yeni/proje/yolu/
graphify update .
graphify hook install
```

Artık yeni projenizde Antigravity açıldığı anda seçtiğiniz teknolojiye özel tüm kuralları, becerileri ve alt ajanları otomatik olarak tanıyacaktır.
