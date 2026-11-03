# Antigravity (AGY) Çoklu Mimari Agent Orkestrasyon Paketleri

Bu depo (`agent-kuralları`), farklı teknolojilerde geliştirme yaparken **bağlam şişmesini (context bloat)** ve **mimari kural çakışmalarını** %100 önlemek için **4 izole profil** halinde yapılandırılmıştır.

Her profil kendi içinde **Ortak Çekirdeği (`Graphify` + `Addy Osmani Agent-Skills` + `Ponytail`)** barındırır ve üzerine sadece ilgili teknolojinin **en sade, yüksek sinyalli uzmanlık becerilerini** ekler.

---

## 📁 1. Klasör Yapısı (4 İzole Profil)

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

## 🧱 2. Tüm Profillerde Bulunan Ortak Çekirdek (18 Beceri)

Her 4 paketin içinde aşağıdaki temel mühendislik omurgası standart olarak yer alır:

1. **`graphify` (1 Beceri + Otomatik Kural):** Kod yazmadan veya `grep` çalıştırmadan önce projenin AST bilgi grafiğini (`graphify-out/graph.json`) sorgular; aynı yardımcı fonksiyonun ikinci kez yazılmasını önler.
2. **`ponytail` (6 Beceri + Otomatik Kural):** `ponytail`, `ponytail-review`, `ponytail-audit`, `ponytail-debt`, `ponytail-gain`, `ponytail-help`. Gereksiz soyutlamaları ve şişkinliği engeller; ancak her profilin `AGENTS.md` dosyasındaki mimari sınır kurallarına tabidir.
3. **`addyosmani/agent-skills` (11 Çekirdek Mühendislik Becerisi):**
   - **Analiz & Planlama:** `interview-me`, `spec-driven-development`, `planning-and-task-breakdown`, `api-and-interface-design`
   - **Kodlama & Test:** `incremental-implementation`, `test-driven-development`, `debugging-and-error-recovery`
   - **Kalite & Güvenlik:** `code-review-and-quality`, `security-and-hardening`, `performance-optimization`, `git-workflow-and-versioning`

---

## 🛠️ 3. Profillere Özel Eklenen Beceriler ve Çakışma Çözümleri

### A. `dotnet-backend/` (.NET / C# / EF Core — +7 Uzmanlık Becerisi)
- **Eklenen Beceriler:**
  - `dotnet-webapi`, `optimizing-ef-core-queries`, `analyzing-dotnet-performance` *(Resmi `dotnet/skills` reposundan)*
  - `modern-csharp-coding-standards`, `efcore-patterns`, `csharp-concurrency-patterns`, `dependency-injection-patterns` *(`Aaronontheweb/dotnet-skills` reposundan)*
- **Giderilen Çakışmalar:**
  - Ponytail'in *"tek yerden çağrılan sınıfları birleştir"* kuralının Clean Architecture, Vertical Slice, MediatR CQRS (`Command`/`Query`/`Handler`/`Validator`) ve `Entity` ↔ `record DTO` katmanlarını ezmesi `dotnet-backend/AGENTS.md` ile engellendi.
  - Sadece Web DOM/CSS için olan `frontend-ui-engineering`, `browser-testing-with-devtools` ve `web-performance-auditor.md` bu paketten çıkarıldı.

### B. `kotlin-compose/` (Jetpack Compose & KMP — +11 Uzmanlık Becerisi)
- **Eklenen Beceriler:**
  - `compose` (giriş karar ağacı), `compose-ui`, `compose-feature`, `compose-architecture`, `compose-data`, `compose-project`, `compose-platform` *(`Meet-Miyani/compose-skill` tam set)*
  - `kotlin-tooling-agp9-migration`, `kotlin-tooling-java-to-kotlin`, `kotlin-tooling-native-build-performance` *(`Kotlin/kotlin-agent-skills` içinden sadece araç becerileri; Spring/JPA backend becerileri hariç)*
  - `ui-ux-pro-max` *(Tasarım sistemi ve renk/tipografi motoru)*
- **Giderilen Çakışmalar:**
  - Ponytail'in `Route` (stateful) ve `Screen` (stateless `@Preview`) composable fonksiyonlarını tek fonksiyonda birleştirmesi ve MVI `Contract.kt` (`State`/`Event`/`Effect`) ile `DTO -> Domain -> UiModel` dönüşümlerini silmesi `kotlin-compose/AGENTS.md` ile yasaklandı.
  - Günlük Gradle/modül yönetimi `compose-project`'e, AGP 9 yükseltmesi ise sadece manuel çağrıda `kotlin-tooling-agp9-migration`'a bağlandı.

### C. `flutter-mobile/` (Flutter & Dart — +8 Uzmanlık Becerisi)
- **Eklenen Beceriler:**
  - `flutter-apply-architecture-best-practices`, `flutter-build-responsive-layout`, `flutter-fix-layout-issues`, `flutter-setup-declarative-routing`, `dart-use-pattern-matching`, `dart-run-static-analysis`, `flutter-add-widget-test` *(Resmi `flutter/skills` reposundan)*
  - `ui-ux-pro-max` *(Görsel token ve `design-system/MASTER.md` üretimi)*
- **Giderilen Çakışmalar:**
  - Ponytail'in `const` constructor'lı küçük `StatelessWidget` sınıflarını *"sadece bir yerden çağrılıyor"* diyerek tek bir dev `build()` metoduna gömmesi (ve Flutter'ın rebuild optimizasyonunu bozması) `flutter-mobile/AGENTS.md` ile engellendi.

### D. `react-nextjs/` (React 19 & Next.js App Router — +6 Uzmanlık Becerisi + `web-performance-auditor`)
- **Eklenen Beceriler:**
  - `vercel-react-best-practices`, `vercel-composition-patterns`, `web-design-guidelines` *(Resmi `vercel-labs/agent-skills` reposundan)*
  - `frontend-ui-engineering`, `browser-testing-with-devtools` *(`addyosmani/agent-skills` Web UI becerileri)*
  - `ui-ux-pro-max` *(Tasarım sistemi motoru)*
- **Giderilen Çakışmalar:**
  - Ponytail'in Next.js Server Component (`page.tsx`) ile Client Component (`'use client'`) dosyalarını tek dosyada birleştirip tüm sayfayı client bundle'a sızdırması `react-nextjs/AGENTS.md` ile engellendi.
  - Görsel kararlar `ui-ux-pro-max`'a, HTML/CSS/WCAG yapısı `frontend-ui-engineering`'e, React 19 / Next.js çalışma modeli ise `vercel-react-best-practices`'e bölüştürüldü.

---

## 🚀 4. Yeni Bir Projeye Hangi Paketi Nasıl Kurarsınız?

Geliştireceğiniz projenin teknolojisine göre ilgili klasörün içeriğini **tek komutla** projenizin kök dizinine kopyalayın:

```bash
# Seçenek A: .NET / C# Backend Projesi İçin
cp -r "/home/murathan_akkus/Desktop/New Folder/agent-kuralları/dotnet-backend/." /hedef/proje/yolu/

# Seçenek B: Kotlin / Jetpack Compose / KMP Projesi İçin
cp -r "/home/murathan_akkus/Desktop/New Folder/agent-kuralları/kotlin-compose/." /hedef/proje/yolu/

# Seçenek C: Flutter Projesi İçin
cp -r "/home/murathan_akkus/Desktop/New Folder/agent-kuralları/flutter-mobile/." /hedef/proje/yolu/

# Seçenek D: React / Next.js Projesi İçin
cp -r "/home/murathan_akkus/Desktop/New Folder/agent-kuralları/react-nextjs/." /hedef/proje/yolu/
```

Kopyaladıktan sonra hedef projede bilgi grafiğini başlatın:

```bash
cd /hedef/proje/yolu/
graphify update .
graphify hook install
```
