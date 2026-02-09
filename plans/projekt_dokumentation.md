# 🚀 Projekt Dokumentation: SBS Recruitment Portal

> **Hvad er dette dokument?**  
> Her finder du en komplet gennemgang af hvordan SBS Recruitment Portal er bygget. Dokumentet er skrevet så alle kan følge med - uanset teknisk baggrund. Vi har fokuseret på at forklare *hvorfor* vi har valgt de forskellige løsninger, ikke bare *hvad* de gør.
>
> 📌 **Målgruppe:** Multimediedesigner-eksamen med fokus på frontend-kompetencer.

---

## 📖 Officiel Dokumentation (Links)

Her er links til al den officielle dokumentation for de teknologier vi bruger:

### Kerneværktøjer
| Teknologi | Dokumentation | Beskrivelse |
|-----------|---------------|-------------|
| **Vue 3** | [vuejs.org](https://vuejs.org/guide/introduction.html) | Frontend framework |
| **TypeScript** | [typescriptlang.org](https://www.typescriptlang.org/docs/) | Typed JavaScript |
| **Vite** | [vitejs.dev](https://vitejs.dev/guide/) | Bygge-værktøj |
| **SCSS/Sass** | [sass-lang.com](https://sass-lang.com/documentation/) | CSS preprocessor |

### UI & Komponenter
| Bibliotek | Dokumentation | Hvad bruges det til? |
|-----------|---------------|----------------------|
| **Element Plus** | [element-plus.org](https://element-plus.org/en-US/) | UI komponenter (knapper, forms, modals) |
| **Video.js v7** | [videojs.com](https://videojs.org/guides/vue/) | Videospiller |
| **@videojs-player/vue** | [github.com/surmon-china/videojs-player](https://github.com/surmon-china/videojs-player) | Vue 3 wrapper til Video.js |
| **OverlayScrollbars** | [kingsora.github.io/OverlayScrollbars](https://kingsora.github.io/OverlayScrollbars/) | Custom scrollbars |
| **@kalimahapps/vue-icons** | [kalimahapps.com](https://github.com/nicepkg/vue-icons) | 50.000+ ikoner fra alle populære pakker |
| **@tato30/vue-pdf** | [vue-pdf-embed](https://github.com/nicepkg/vue-pdf-embed) | PDF-visning i HR Dashboard |

### Vite Plugins
| Plugin | Dokumentation | Funktion |
|--------|---------------|----------|
| **unplugin-auto-import** | [github.com/unplugin/unplugin-auto-import](https://github.com/unplugin/unplugin-auto-import) | Auto-import af Vue funktioner |
| **unplugin-vue-components** | [github.com/unplugin/unplugin-vue-components](https://github.com/unplugin/unplugin-vue-components) | Auto-import af komponenter |

### Node.js & Værktøjer
| Værktøj | Dokumentation | Brug |
|---------|---------------|------|
| **Node.js** | [nodejs.org](https://nodejs.org/docs/latest/api/) | JavaScript runtime |
| **npm** | [docs.npmjs.com](https://docs.npmjs.com/) | Package manager |
| **FFmpeg** | [ffmpeg.org](https://ffmpeg.org/documentation.html) | Video/billed-behandling |
| **Express.js** | [expressjs.com](https://expressjs.com/) | Backend server |
| **better-sqlite3** | [github.com/WiseLibs](https://github.com/WiseLibs/better-sqlite3) | SQLite database |
| **Nodemailer** | [nodemailer.com](https://nodemailer.com/) | Email-afsendelse |
| **Multer** | [github.com/expressjs/multer](https://github.com/expressjs/multer) | Fil-upload håndtering |
| **Axios** | [axios-http.com](https://axios-http.com/) | HTTP requests |

### Browser APIs vi bruger
| API | Dokumentation | Hvad bruges det til? |
|-----|---------------|----------------------|
| **IntersectionObserver** | [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API) | Lazy loading af billeder |
| **MediaQueryList** | [MDN](https://developer.mozilla.org/en-US/docs/Web/API/MediaQueryList) | Responsive billeder |
| **Drag and Drop API** | [MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTML_Drag_and_Drop_API) | CV upload |

---

## 1. 🎯 Hvad er SBS Recruitment Portal?

Forestil dig at du leder efter et nyt job hos SBS (Scandinavian Brake Systems). I stedet for at sende en kedelig e-mail, kan du nu:

- 🎬 **Se videoer** om hvordan det er at arbejde hos SBS
- 💼 **Udforske jobmuligheder** med billeder og beskrivelser
- 🧠 **Tage en personlighedstest** så SBS kan lære dig bedre at kende
- 📅 **Booke din egen samtaletid** - du vælger selv hvornår det passer dig
- 📄 **Uploade dit CV** (PDF) med drag & drop

Det hele sker i én samlet webapp - ingen sideskift, ingen ventetid. Det er det vi kalder en **Single Page Application (SPA)**.

> 💡 **Bonus:** HR-afdelingen har deres eget dashboard hvor de kan administrere alle ansøgninger og samtaletider. Smart, ikke?

---

## 2. 🧰 Vores Værktøjskasse (Teknologier)

### 2.1 De Fire Hjørnesten

Tænk på disse som fundamentet i et hus - uden dem kan vi ikke bygge noget:

| Teknologi | Hvad er det? | Hvorfor bruger vi det? | Docs |
|-----------|--------------|------------------------|------|
| **Vue 3** | Et JavaScript framework | Gør det nemt at bygge interaktive brugerflader. Tænk på det som LEGO-klodser til websider! | [📖](https://vuejs.org/guide/introduction.html) |
| **TypeScript** | JavaScript med superkræfter | Hjælper os med at fange fejl *før* brugeren ser dem. Som en stavekontrol for kode! | [📖](https://www.typescriptlang.org/docs/) |
| **Vite** | Bygge-værktøj | Gør det lynhurtigt at udvikle og teste. Ændringer vises på under 1 sekund! | [📖](https://vitejs.dev/guide/) |
| **SCSS** | CSS med variabler | Lader os genbruge farver og størrelser overalt. Ændrer vi én farve, opdateres hele siden! | [📖](https://sass-lang.com/documentation/) |

---

#### 🎨 Vue 3 - Hvordan vi skriver kode

Vi bruger den nyeste måde at skrive Vue-komponenter på, kaldet **Composition API** med `<script setup>`. Her er et simpelt eksempel:

```vue
<script setup lang="ts">
import { ref, computed } from 'vue'

// 🎬 Vi holder styr på om videoen afspiller
const isPlaying = ref(false)

// 🏷️ Knappens tekst ændrer sig automatisk!
const buttonLabel = computed(() => 
  isPlaying.value ? '⏸️ Pause' : '▶️ Afspil'
)
</script>
```

**Hvad sker der her?**
- `ref(false)` er en "reaktiv variabel" - Vue holder øje med den
- `computed()` er en smart funktion der automatisk opdaterer sig selv
- Når `isPlaying` ændres, opdaterer `buttonLabel` sig helt automatisk!

> 🎉 **Det smarte:** I stedet for at skrive 50 linjer kode for at opdatere en knap, klarer Vue det på 2 linjer!

---

### 2.2 Færdige Komponenter vi Bruger

Hvorfor opfinde den dybe tallerken? Vi bruger nogle fantastiske biblioteker:

#### 🎨 Element Plus - Vores UI-værktøjskasse
**Hvad er det?** En samling af over 70 færdige komponenter - knapper, formularer, kalendere, pop-ups og meget mere.

📖 **Dokumentation:** [element-plus.org](https://element-plus.org/en-US/component/button.html)

**Det smarte:** Vi har sat det op så kun de komponenter vi *faktisk bruger* bliver inkluderet i den endelige fil. Det er som at pakke en kuffert - hvorfor tage hele garderoben med hvis du kun skal bruge 3 t-shirts?

```typescript
// 🪄 Magien i vite.config.ts
Components({
  resolvers: [ElementPlusResolver()]  // "Hent kun det vi bruger!"
})
```

> 💾 **Resultat:** I stedet for at downloade 2MB, downloader brugeren måske kun 200KB. Hurtigere = gladere brugere!

---

#### 📜 OverlayScrollbars - Pæne scrollbars overalt
**Problemet:** Scrollbars ser vidt forskellige ud på Windows, Mac og Linux. Det ødelægger designet!

**Løsningen:** OverlayScrollbars giver os smukke, ensartede scrollbars der matcher SBS' design - uanset hvilken computer du bruger.

📖 **Dokumentation:** [kingsora.github.io/OverlayScrollbars](https://kingsora.github.io/OverlayScrollbars/)

---

#### 🎭 Vue Icons - 50.000+ ikoner
**Hvad er det?** En kæmpe samling af ikoner fra Material Design, Font Awesome og mange flere.

**Det smarte:** Ligesom med Element Plus, inkluderer vi kun de ikoner vi faktisk bruger. Ingen spild!

---

### 2.3 🎬 Video & Medier

#### VideoPlayerV2 - Smart Video.js Wrapper
Vi bruger **[Video.js v7](https://docs.videojs.com/)** som videospiller og har bygget en custom Vue-komponent ovenpå (~213 linjer kode) med **[@videojs-player/vue](https://github.com/surmon-china/videojs-player)**.

**Vores spiller kan:**
- ✨ Vise SBS-brandede kontrolknapper (ikke standard browser-design)
- 📱 Opføre sig smart på både computer og telefon
- ⌨️ Styres med tastaturet (vigtigt for tilgængelighed!)
- 🔊 Vise undertekster for hørehæmmede
- 📺 Gå i fullscreen - selv på besværlige iPhones!
- 🖼️ Auto-generere poster fra video-path
- ⏳ Smart preload-kontrol (undgår unødvendig data-download)

#### ⏳ Video Preload - Undgå Unødvendig Download

**Problemet:** Uden kontrol downloader browseren ALLE videoer med det samme - også dem i lukkede modals!

**Løsningen:** Vi har tilføjet en `preload` prop til VideoPlayerV2:

```typescript
// 📱 Props interface i VideoPlayerV2.vue
interface Props {
  src: string
  poster?: string
  preload?: 'auto' | 'metadata' | 'none'  // 👈 NY PROP!
  // ... andre props
}

// Default værdier
const props = withDefaults(defineProps<Props>(), {
  preload: 'auto'  // Standard: download med det samme
})
```

**De tre preload-værdier:**
| Værdi | Hvad sker der? | Hvornår bruges det? |
|-------|----------------|---------------------|
| `auto` | Hele videoen downloades med det samme | Hero video (skal vises hurtigt) |
| `metadata` | Kun video-info (varighed, dimensioner) | Modal videoer (downloades ved play) |
| `none` | Ingenting downloades før play | Videoer langt nede på siden |

**Sådan bruges det:**
```vue
<!-- Hero video: Download med det samme -->
<VideoPlayerV2 src="/videos/hero.mp4" preload="auto" />

<!-- Modal video: Vent med download til brugeren trykker play -->
<VideoPlayerV2 src="/videos/emma.mp4" preload="metadata" />
```

> 🏆 **Resultat:** I stedet for at downloade 4 videoer (30MB+) ved page load, downloader vi kun hero-videoen. De andre venter til brugeren faktisk vil se dem!
>
> 📖 **Læs mere:** [Video preload attribute på MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/video#preload)

---

### 2.4 🗺️ Navigation (Routing)

**Hvad er routing?** Det er måden vi navigerer mellem forskellige "sider" i vores app - selvom det teknisk set er samme side!

📖 **Dokumentation:** [Vue Router](https://router.vuejs.org/guide/)

```typescript
// 🗺️ Vores ruter
routes: [
  { path: '/', component: LandingPage },           // Forsiden - altid klar!
  { path: '/hr-dashboard', component: () => import(...) }  // Kun hentet når nødvendigt
]
```

**Bemærk det smarte:** HR Dashboard'et bruger `() => import(...)`. Det betyder at koden først downloades når nogen faktisk besøger den side. 

> 🚀 **Resultatet:** Almindelige ansøgere skal ikke vente på at admin-kode downloader. Win-win!

---

### 2.5 🌐 Axios - HTTP Kommunikation

**Hvad er Axios?** Et bibliotek der gør det nemt at sende HTTP-requests fra frontend til backend.

📖 **Dokumentation:** [axios-http.com](https://axios-http.com/)

**Hvorfor Axios i stedet for `fetch()`?**

`fetch()` er browserens indbyggede måde at hente data på, men Axios gør mange ting nemmere. Her er en sammenligning:

#### 1️⃣ Automatisk JSON parsing
```javascript
// ❌ Med fetch() - skal manuelt konvertere til JSON
const response = await fetch('/api/applications')
const data = await response.json()  // 👈 Ekstra trin!

// ✅ Med Axios - JSON kommer automatisk
const response = await api.get('/applications')
const data = response.data  // 👈 Allerede JSON!
```

#### 2️⃣ Bedre fejlhåndtering
```javascript
// ❌ Med fetch() - 404 og 500 fejl er IKKE exceptions!
const response = await fetch('/api/users/999')
// response.ok er false, men koden fortsætter! 😱
if (!response.ok) {
  throw new Error('Noget gik galt')  // Skal selv tjekke og kaste fejl
}

// ✅ Med Axios - HTTP fejl thrower automatisk
try {
  const response = await api.get('/users/999')
} catch (error) {
  // Axios kaster automatisk fejl ved 4xx og 5xx status! 🎉
  console.log(error.response.status)  // 404
  console.log(error.response.data)    // Fejlbesked fra server
}
```

#### 3️⃣ Interceptors - Automatiske headers
```javascript
// 🔐 Tilføj authorization til ALLE requests automatisk
api.interceptors.request.use((config) => {
  config.headers['Authorization'] = 'Bearer ' + getToken()
  return config
})

// 🚨 Håndter alle 401 fejl ét sted (f.eks. redirect til login)
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response.status === 401) {
      router.push('/login')  // Auto-redirect ved udløbet session
    }
    return Promise.reject(error)
  }
)
```

#### 4️⃣ Timeout - Undgå uendelig ventetid
```javascript
// ❌ Med fetch() - ingen timeout! Kan vente for evigt
const response = await fetch('/api/slow-endpoint')

// ✅ Med Axios - timeout efter 5 sekunder
const api = axios.create({
  timeout: 5000  // 👈 Afbryd efter 5000ms
})
// Kaster fejl hvis server ikke svarer inden 5 sekunder
```

> 💡 **Kort sagt:** Axios sparer os for masser af boilerplate-kode og gør fejlhåndtering meget nemmere!

**Vores API-konfiguration (`src/config/api.ts`):**

```typescript
import axios from 'axios'

// 🔧 Opret en axios "instance" med fælles indstillinger
const api = axios.create({
  baseURL: '/api',              // Alle requests starter med /api
  headers: {
    'Content-Type': 'application/json'
  }
})

// 🔐 Tilføj authorization header (til HR-login)
export const setAuthHeader = (password: string) => {
  api.defaults.headers.common['Authorization'] = password
}

export default api
```

**Sådan bruges det i komponenter:**

```typescript
import api from '@/config/api'

// 📤 Send ansøgning til backend
const response = await api.post('/apply', formData)

// 📥 Hent ledige samtaletider
const slots = await api.get('/slots')

// 🔄 Opdater ansøgningsstatus (HR)
await api.patch(`/applications/${id}`, { status: 'accepted' })
```

**Smart miljø-detection:**
```typescript
// I produktion: Brug relativ URL (samme server)
// I udvikling: Brug proxy eller localhost:3000
const API_BASE_URL = import.meta.env.PROD 
  ? ''  // Relativ - frontend og backend på samme server
  : 'http://localhost:3000'  // Separat backend-server
```

> 💡 **Fordelen:** Vi behøver ikke ændre URLs når vi deployer - det virker automatisk!

---

### 2.6 📋 TypeScript - Vores Sikkerhedsnet

TypeScript lader os beskrive præcis hvordan vores data skal se ud. Her er vores faktiske types fra `src/types/index.ts`:

#### 📊 Grundlæggende Types (Begrænsede Værdier)
```typescript
// DISC personlighedsprofiler - kun disse 4 bogstaver er gyldige!
type DiscProfile = 'D' | 'I' | 'S' | 'C'

// Ansøgningsstatus - følger hele processen fra start til slut
type ApplicationStatus =
  | 'pending'              // Afventer behandling
  | 'reviewing'            // Under vurdering
  | 'interview-scheduled'  // Samtale booket
  | 'interview-completed'  // Samtale afholdt
  | 'accepted'             // Ansat! 🎉
  | 'rejected'             // Afvist

// Job-stillinger hos SBS
type JobPosition = 'pakkeri' | 'produktion' | 'andre'

// Alder - gemmes som string fra dropdown (16-99 år)
type Age = string  // f.eks. "25", "42", "67"
```

#### 📅 Interview Slots (Samtaletider)
```typescript
interface InterviewSlot {
  id: string                      // Unikt ID
  date: string                    // "2024-02-15"
  time: string                    // "10:00"
  type: 'fysisk' | 'virtuel'      // Mødetype
  isBooked: boolean               // Er tiden optaget?
  bookedBy?: string               // Hvem har booket (ansøger ID)
  reservedBy?: string             // Midlertidig reservation
  reservedAt?: string             // Hvornår blev den reserveret
}
```

#### 🧠 DISC Test Types
```typescript
// Et enkelt svar i DISC-testen
interface DiscOption {
  text: string          // "Jeg tager gerne styringen..."
  profile: DiscProfile  // 'D', 'I', 'S' eller 'C'
  points: number        // 1, 2 eller 3 point
}

// Resultatet af DISC-testen
interface DiscResult {
  totalPoints: number      // Samlet score (max 15)
  isQualified: boolean     // Opfylder threshold (11+)?
  dominantProfile: DiscProfile  // Højeste profil
  profileScores: {         // Score per profil
    D: number
    I: number
    S: number
    C: number
  }
}
```

#### 👤 Den Komplette Ansøgning
```typescript
interface Application {
  id: string                    // Unikt UUID
  fullName: string              // Navn
  phone: string                 // Telefon
  email: string                 // E-mail
  age: Age                      // Specifik alder som string (f.eks. "25")
  jobPosition: JobPosition      // Stilling
  cvFileName?: string           // CV-filnavn (valgfrit)
  discResult: DiscResult        // Personlighedstest-resultat
  selectedSlots: string[]       // Valgte samtaletider (IDs)
  confirmedSlot?: InterviewSlot // Bekræftet samtale
  status: ApplicationStatus     // Nuværende status
  createdAt: string             // Oprettet tidspunkt
  updatedAt: string             // Sidst opdateret
  expiresAt: string             // Udløber (14 dage - GDPR)
}
```

**Hvorfor er det smart?**
1. 🚨 **Stavefejl fanges med det samme** - skriver du `jopPosition`, får du rød understregning!
2. 💡 **Autokomplete** - editoren foreslår `'pending' | 'reviewing' | ...` når du skriver status
3. 📖 **Selvdokumenterende** - vi kan se præcis hvad data indeholder
4. 🔒 **Begrænsede værdier** - `type: 'fysisk' | 'virtuel'` sikrer at ingen skriver `'online'` ved en fejl

> 🎯 **Tænk på det som:** En opskrift der fortæller dig præcis hvilke ingredienser du skal bruge - og advarer dig hvis du glemmer noget eller bruger det forkerte!

---

## 3. 📁 Sådan er Projektet Organiseret

Et velorganiseret projekt er et glædeligt projekt! Her er vores mappestruktur:

```
sbs-recruitment-app/
├── � index.html        → Hoved HTML-fil (entry point)
├── 📄 vite.config.ts    → Vite build konfiguration
├── 📄 package.json      → Dependencies og scripts
├── 📄 tsconfig.json     → TypeScript konfiguration
│
├── 📂 plugins/          → Vores egne Vite-plugins
│   └── vite-plugin-critical-media.ts
│
├── 📂 scripts/          → Automatiserings-scripts
│   ├── generate-posters.js      → Laver poster-billeder fra videoer
│   └── convert-videos-8bit.js   → Konverterer 10-bit til 8-bit video
│
├── 📂 public/           → Statiske filer (kopieres direkte)
│   ├── images/          → Billeder (posters, job-billeder)
│   └── videos/          → Videoer (_8bit versioner)
│
├── 📂 server/           → Backend (Node.js + Express)
│   ├── index.js         → Express server
│   ├── database.js      → SQLite database setup
│   ├── seed-dummy-data.js → Test-data generator
│   ├── uploads/         → CV-filer uploadet af ansøgere (kun PDF)
│   └── data/            → Database-filer
│
└── 📂 src/              → Frontend-kode (Vue 3)
    ├── 📄 main.ts       → App entry point
    ├── 📄 App.vue       → Root komponent
    │
    ├── 📂 assets/scss/  → Styling
    │   ├── abstracts/   → Variabler, mixins (ingen CSS output)
    │   ├── base/        → Reset, typography, utilities
    │   ├── components/  → Knapper, cards, forms, modals
    │   ├── layout/      → Footer, grid
    │   └── vendors/     → Element Plus, Video.js overrides
    │
    ├── 📂 components/   → Vue komponenter
    │   ├── ApplicationModal.vue   → Ansøgningsflow (2418 linjer)
    │   ├── VideoPlayerV2.vue      → Video.js wrapper (213 linjer)
    │   ├── CalendarSlotPicker.vue → Kalender (816 linjer)
    │   └── ... (9 komponenter i alt)
    │
    ├── 📂 views/        → Side-komponenter
    │   ├── LandingPage.vue   → Forside
    │   └── HRDashboard.vue   → Admin panel
    │
    ├── 📂 config/       → Konfigurationsfiler
    │   ├── api.ts       → API endpoints
    │   └── discQuestions.ts → DISC test spørgsmål
    │
    ├── 📂 router/       → Vue Router
    │   └── index.ts     → Rute-definitioner
    │
    ├── 📂 types/        → TypeScript types
    │   └── index.ts     → Alle interfaces og types
    │
    └── 📂 utils/        → Hjælpe-funktioner
        └── mediaPreloader.ts → Preload af billeder/video
```

---

### 3.1 🎨 Vores SCSS-System (7-1 Pattern)

Vi har organiseret vores CSS efter et velkendt mønster kaldet **7-1 Pattern**. Tænk på det som et arkivsystem for styles!

#### 📦 `abstracts/` - Design-byggeklodserne
Her gemmer vi ting der *ikke* selv laver CSS, men som bruges overalt:

**Farver:**
```scss
// 🎨 SBS' brandfarver - ændr her, og hele siden opdateres!
$color-dark-gray: #2d2d2d;    // Primær farve (tekst, knapper)
$color-red: #ee3123;          // "Ansøg nu" knappen
$color-yellow: #f1db53;       // Kalenderdatoer med ledige tider
$color-green: #2ec700;        // Success (godkendt)
$color-light-gray: #ebebeb;   // Baggrunde, borders
$color-white: #ffffff;        // Cards, modal-baggrunde
```

**Mellemrum (Spacing):**
```scss
// 📏 Vi bruger et 6px grid - alt er deleligt med 6!
$spacing-xs: 6px;    // Lille afstand
$spacing-sm: 12px;   // Standard padding
$spacing-md: 18px;   // Medium mellemrum
$spacing-lg: 24px;   // Stor afstand
$spacing-xl: 48px;   // Kæmpe mellemrum
```

**Border Radius & Shadows:**
```scss
// 🔲 Afrundede hjørner
$border-radius-sm: 6px;
$border-radius-lg: 12px;
$border-radius-circle: 50%;

// 🌑 Skygger - bruger CSS custom properties for dynamisk farve
$shadow-modal: 0px 0px 18px 6px rgba(var(--el-color-primary-rgb), 0.25);
$shadow-card: 0px 6px 6px rgba(var(--el-color-primary-rgb), 0.1);
```

> 💡 **Hvorfor 6px?** Det skaber visuel harmoni! Når alt er deleligt med samme tal, ser designet mere "rent" ud.

---

**Mixins - Genbrugelige style-opskrifter:**

Vi har over **795 linjer** med mixins! Her er et eksempel:

```scss
// 🔘 En mørk knap - bruges mange steder
@mixin button-colors-dark {
  background-color: $c-primary;  // Mørk baggrund
  color: $c-bg;                  // Hvid tekst
  
  // 🖱️ Kun hover-effekt på computere med mus!
  @media (hover: hover) {
    &:hover {
      background-color: $c-fill-light;  // Lys baggrund ved hover
      color: $c-primary;                 // Mørk tekst ved hover
    }
  }
}
```

**Det smarte ved mixins:** I stedet for at skrive de samme 10 linjer CSS 20 forskellige steder, skriver vi bare `@include button-colors-dark;` og så er vi færdige!

---

#### 🪄 Global Injection - Automatisk Tilgængelighed

Vi har sat Vite op til automatisk at gøre alle variabler og mixins tilgængelige:

```typescript
// ✨ Alle Vue-komponenter har automatisk adgang til vores design-tokens!
additionalData: `@use "@/assets/scss/abstracts" as *;`
```

> 🎉 **Resultatet:** Vi behøver aldrig at importere farver eller mixins manuelt. De er bare *der*!

---

## 4. ⭐ Frontend Highlights (De Fede Ting!)

Her kommer de teknikker der virkelig gør projektet specielt. Disse er perfekte at fremhæve til eksamen!

---

### 4.1 🖼️ Smart Billede-System (`ResponsiveImage.vue`)

**Problemet:** Normale responsive billeder downloader et nyt billede hver gang du ændrer vinduesstørrelse. Det er spild af data!

**Vores løsning:** Vi har bygget en smart komponent der:

1. **Loader alle billedstørrelser på én gang** (men viser kun én)
2. **Husker hvad der allerede er downloadet**
3. **Skifter øjeblikkeligt** mellem størrelser - ingen ny download!

```vue
<!-- 📸 Sådan bruger vi den -->
<ResponsiveImage
  base-name="job-pakkeri"
  breakpoints="400,0;800,400;1200,800"
  alt="Pakkeri arbejde"
/>
```

**Hvad betyder `breakpoints="400,0;800,400;1200,800"`?**
- Fra 0px skærmbredde: Brug 400px bred version
- Fra 400px skærmbredde: Brug 800px bred version  
- Fra 800px skærmbredde: Brug 1200px bred version

**Bonus-feature - Lazy Loading med `rootMargin`:**
```typescript
// 👀 Billedet loader når det er 300px fra at blive synligt!
lazyObserver = new IntersectionObserver(
  (entries) => {
    if (entries[0].isIntersecting) {
      shouldLoad.value = true  // "Nu er det tid til at loade!"
    }
  },
  { rootMargin: '300px', threshold: 0 }  // 👈 Starter 300px før billedet er synligt!
)
```

**Hvad er `rootMargin`?** Det er en "buffer zone" omkring viewport. Ved at sætte den til `300px` begynder billedet at loade *før* brugeren scroller helt ned til det. Så ser de aldrig en grå baggrund!

> 🚀 **Resultatet:** Hurtigere sider, mindre dataforbrug, og ingen "hoppende" layout når billeder loader!
>
> 📖 **Læs mere:** [IntersectionObserver på MDN](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API)

---

### 4.2 🔌 Vores Eget Vite Plugin (Critical Media Preload)

**Problemet:** Det store hero-billede på forsiden skal vises HURTIGT. Ellers oplever brugeren en langsom side.

**Vores løsning:** Vi har skrevet vores eget plugin der automatisk:

1. 🔍 Scanner efter vigtige billeder når vi bygger projektet
2. 📝 Tilføjer specielle "preload" tags i HTML'en
3. 🚀 Fortæller browseren: "Hent dette billede FØR alt andet!"
4. ✅ Kun preloader `_8bit` posters (de optimerede versioner vi faktisk bruger)

📖 **Læs mere om Vite Plugins:** [vitejs.dev/guide/api-plugin](https://vitejs.dev/guide/api-plugin.html)

**Resultatet i den færdige HTML:**
```html
<!-- ✨ Auto-genereret af vores plugin! -->
<link rel="preload" 
      href="/images/HERO_V2_8bit-poster.webp" 
      as="image" 
      fetchpriority="high">
```

> 🏆 **Hvorfor er dette imponerende?** Vi har ikke bare *brugt* Vite - vi har *udvidet* det med vores egen funktionalitet!

---

### 4.3 🎬 Automatisk Video-Poster Generering

**Problemet:** Videoer tager tid at loade. Mens de loader, ser brugeren en sort boks. Ikke særlig indbydende!

**Løsningen:** Et script der automatisk laver "poster-billeder" fra videoernes første frame med **[FFmpeg](https://ffmpeg.org/)**:

```javascript
// 📸 Snup det første frame fra videoen og gem som WebP
ffmpeg -i "video.mp4" -vframes 1 "video-poster.webp"
```

**Det smarte:**
- ✅ Kører automatisk når vi bygger projektet (`prebuild` script)
- ✅ Springer over hvis poster allerede eksisterer (up to date check)
- ✅ Bruger WebP-format (meget mindre filer!)
- ✅ Genererer kun posters for `_8bit` videoer (de optimerede versioner vi bruger)

> 💡 **Resultatet:** Brugeren ser straks et billede mens videoen loader i baggrunden. Meget bedre oplevelse!

---

### 4.4 🎥 Video Konvertering & Komprimering Script

**Problemet:** Videoer fra filmproduktion er ofte for store og i forkert format:
- **10-bit farver** (Firefox understøtter ikke dette!)
- **Høj bitrate** (22.000+ kbps = kæmpe filer)
- **60fps** (unødvendigt for web)

**Løsningen:** Et custom script (`scripts/convert-videos-8bit.js`) der:

1. 🔍 Scanner alle `.mp4` filer i `/public/videos/`
2. 📊 Analyserer pixel format og bitrate med FFprobe
3. 🎬 Konverterer kun de videoer der har behov
4. 🚀 Bruger **NVIDIA NVENC** GPU-acceleration (RTX 3070)

```javascript
// 📊 Automatisk detektion af hvad der skal konverteres
function needsConversion(info) {
  // 10-bit formater → skal konverteres for Firefox
  const tenBitFormats = ["yuv420p10le", "yuv420p10be"];
  if (tenBitFormats.includes(info.pix_fmt)) {
    return { needs: true, reason: "10-bit" };
  }
  
  // Høj bitrate (> 4200 kbps) → skal komprimeres
  if (bitrateKbps > 4200) {
    return { needs: true, reason: "høj bitrate" };
  }
  
  return { needs: false };
}
```

**Indstillinger:**
| Parameter | Værdi | Beskrivelse |
|-----------|-------|-------------|
| Target Bitrate | 4000 kbps | God kvalitet, lille fil |
| Max Bitrate | 6000 kbps | Buffer til komplekse scener |
| Preset | p7 | Højeste NVENC kvalitet |
| Pixel Format | yuv420p | 8-bit (browser kompatibel) |
| FPS | 30 (fra 60fps) | Halverer filstørrelse |

**Smart FPS-håndtering:**
```javascript
// 60fps → 30fps (halverer data, ser stadig smooth ud)
if (sourceFps >= 58 && sourceFps <= 62) return 30;

// ~30fps → præcis 30fps (normalisering)
if (sourceFps >= 29 && sourceFps <= 31) return 30;

// Andre fps (25, 24, etc.) → bevar original
return null; // passthrough
```

**Resultater:**
| Video | Original | Komprimeret | Besparelse |
|-------|----------|-------------|------------|
| EMMA | 22,439 kbps | ~4,177 kbps | **81%** |
| HERO | 9,334 kbps | ~4,221 kbps | **55%** |
| MARCO | 22,444 kbps | ~4,161 kbps | **81%** |

> 🚀 **GPU Power:** RTX 3070 konverterer med 340+ fps - en 6 minutters video tager kun ~60 sekunder!

---

### 4.5  Element Plus i SBS-Farver

Vi har "overridet" Element Plus' standard-farver så alt matcher SBS' brand:

```scss
:root {
  // 🎨 Alt der før var blåt, er nu SBS' mørke grå!
  --el-color-primary: #2d2d2d;
  
  // 🟡 "Warning" er nu gul (bruges i kalenderen)
  --el-color-warning: #f1db53;
  
  // 🔴 "Danger" er SBS' røde
  --el-color-danger: #ee3123;
}
```

> ✨ **Magien:** Vi ændrer ét sted, og ALLE Element Plus komponenter opdaterer sig!

---

### 4.6 📦 Code Splitting - Smart Opdeling

**Problemet:** Én stor JavaScript-fil = lang ventetid.

**Løsningen:** Vi deler koden op i mindre "chunks":

```typescript
manualChunks: {
  'vue-vendor': ['vue', 'vue-router'],                    // Vue-kerne (ændres sjældent)
  'element-plus': ['element-plus', '@element-plus/icons-vue'],  // UI-komponenter
  'video-player': ['video.js', '@videojs-player/vue'],   // Videospiller
  'scrollbar': ['overlayscrollbars', 'overlayscrollbars-vue']   // Custom scrollbars
}
```

**Fordelene:**
- 🚀 Parallel download - flere små filer hentes samtidig
- 💾 Bedre caching - Vue ændrer sig sjældent, så browseren husker den
- ⏱️ Video.js loades kun på sider med video

---

### 4.7 📱 Touch Device Hover-Fix

**Problemet:** På telefoner "hænger" hover-effekter efter du har trykket. Irriterende!

**Løsningen:** Vi bruger en smart CSS media query:

```scss
// 🖱️ Kun hover på enheder der faktisk har en mus!
@media (hover: hover) and (pointer: fine) {
  &:hover {
    background-color: $c-fill-light;
  }
}
```

**Hvad betyder det?**
- ✅ Desktop med mus = hover virker
- ❌ Telefon/tablet = ingen hover (ingen sticky effekter!)

---

### 4.8 ⚡ Passive Event Listeners

**Problemet:** Nogle biblioteker gør scrolling langsom ved at blokere touch-events.

**Løsningen:** Vi tilføjer automatisk `passive: true` til alle touch-events:

```typescript
// 🏎️ Gør scrolling butter-smooth!
const passiveEvents = ['touchstart', 'touchmove', 'wheel']
if (passiveEvents.includes(type)) {
  options.passive = true  // "Bloker ikke scrolling!"
}
```

> 🎯 **Resultatet:** Silkeblød scrolling, selv på langsomme telefoner!

---

### 4.9 📝 Multi-Step Ansøgningsflow

Ansøgningsprocessen er opdelt i trin - som en guide der leder brugeren igennem:

```
📋 Step 1: Hvem er du? (Navn, e-mail, telefon)
     ↓
🧠 Step 2: Personlighedstest (5 hurtige spørgsmål)
     ↓
📅 Step 3: Vælg samtaletider (op til 2 tidspunkter)
     ↓
✅ Step 4: Bekræft og send
     ↓
🎉 Step 5: Tak for din ansøgning!
```

**Smart animation mellem steps:**
```typescript
// 👈👉 Glid til venstre eller højre baseret på retning
const slideDirection = computed(() => 
  currentStep.value > previousStep.value ? 'slide-left' : 'slide-right'
)
```

---

#### 🧠 DISC Personlighedstest

Vi bruger DISC-modellen til at lære ansøgere at kende. Der er 5 spørgsmål og et kvalifikations-threshold på 11 point (max 15):

```typescript
// 📊 Hver svarmulighed giver point til en profil
options: [
  { text: 'Jeg tager gerne styringen...', profile: 'D', points: 1 },
  { text: 'Jeg elsker at samarbejde...', profile: 'I', points: 2 },
  { text: 'Jeg foretrækker stabilitet...', profile: 'S', points: 3 },
  { text: 'Jeg fokuserer på detaljer...', profile: 'C', points: 3 }
]

// Kvalifikations-grænse
export const QUALIFICATION_THRESHOLD = 11  // Minimum for at være kvalificeret
export const MAX_POINTS = 15               // 5 spørgsmål × max 3 point
```

> 💡 **S og C profiler** (stabile, detaljeorienterede) scorer højest - de passer bedst til produktionsarbejde!

---

#### 📄 Drag & Drop CV Upload (Kun PDF)

Ansøgere kan uploade deres CV ved at trække filen ind i upload-zonen eller klikke for at vælge:

```vue
<!-- 📂 Træk din PDF-fil herhen! -->
<el-upload
  accept=".pdf,application/pdf"
  @change="handleFileChange"
>
  <div
    @dragenter="onDragEnter"
    @dragover="onDragOver"
    @drop="onDrop"
    :class="{ 'is-dragover': isDragOver }"
  >
    Træk dit CV hertil, eller klik for at vælge
  </div>
</el-upload>
```

**Validering på 3 niveauer:**
| Niveau | Hvor | Hvad sker der? |
|--------|------|----------------|
| 1️⃣ **HTML** | `accept=".pdf"` | Fil-dialog viser kun PDF-filer |
| 2️⃣ **Frontend** | `isValidFileType()` | Viser fejlbesked hvis forkert type |
| 3️⃣ **Backend** | Multer `fileFilter` | Afviser upload og returnerer fejl |

```typescript
// Frontend validering (ApplicationModal.vue)
const allowedExtensions = ['.pdf']
const FILE_TYPE_WARNING = 'Kun PDF filer er tilladt'

const isValidFileType = (file: File): boolean => {
  return file.name.toLowerCase().endsWith('.pdf')
}
```

```javascript
// Backend validering (server/index.js)
const upload = multer({
  fileFilter: (req, file, cb) => {
    const ext = path.extname(file.originalname).toLowerCase()
    if (ext === '.pdf') {
      cb(null, true)
    } else {
      cb(new Error('Invalid file type. Only PDF documents are allowed.'))
    }
  }
})
```

> 🔒 **Sikkerhed:** Vi validerer fil-typen både frontend OG backend - aldrig stol kun på klient-validering!

---

### 4.10 📅 Kalender & Booking-System

Ansøgere kan selv vælge deres samtaletider:

1. 🟡 **Gule datoer** = der er ledige tider
2. ⬜ **Grå datoer** = ingen ledige tider eller i fortiden
3. 📍 **Blå ring** = dags dato

```vue
<!-- 📅 Kalenderen viser kun relevante datoer som klikkbare -->
<div :class="{
  'has-slots': hasTimeSlotsOnDate(day),     // Gul = ledige tider
  'disabled': isDateInPast(day),            // Grå = fortid
  'today': isToday(day)                     // Blå ring = i dag
}">
```

---

### 4.11 🔤 Smart Font-Loading

**Problemet:** Når custom fonts loader, "hopper" teksten (FOUT - Flash of Unstyled Text).

**Løsningen:** Vi skjuler teksten indtil fonten er klar:

```scss
// 🔄 Mens fonts loader - skjul teksten
.wf-loading body {
  opacity: 0;
}

// ✅ Når fonts er klar - fade ind!
.wf-active body {
  opacity: 1;
  transition: opacity 0.3s;
}
```

> 🎯 **Resultatet:** Ingen hoppende tekst - bare en smooth fade-in når alt er klart!

---

### 4.12 🔴 Flydende "Ansøg Nu" Knap

En knap der altid er synlig - uanset hvor du scroller:

```scss
.floating-apply-button {
  position: fixed;           // 📌 Fastgjort til skærmen
  top: 70%;                  // 📍 70% nede fra toppen
  right: 48px;               // 📏 48px fra højre kant
  transform: rotate(-90deg); // 🔄 Roteret for at spare plads
  z-index: 900;              // ⬆️ Altid ovenpå andet indhold
  
  &:hover {
    transform: rotate(-90deg) translateY(-6px);  // ⬆️ Hop op ved hover!
  }
}
```

---

### 4.13 ✨ Modal Animationer

Vores modals (pop-ups) har flotte animationer:

```scss
// 🎬 Fade ind + zoom ind
.modal-enter-from {
  opacity: 0;
  transform: scale(0.95);  // Lidt mindre
}

.modal-enter-to {
  opacity: 1;
  transform: scale(1);     // Normal størrelse
}
```

**Step-navigation glider til siden:**
```scss
// ➡️ Næste step glider ind fra højre
.slide-left-enter-from {
  opacity: 0;
  transform: translateX(30px);
}
```

---

## 5. 📊 Komponent-Oversigt

Her er alle vores Vue-komponenter med en kort beskrivelse:

### 5.1 Komponenter (`src/components/`)

| Komponent | Hvad gør den? | Størrelse |
|-----------|---------------|-----------|
| 🎬 `VideoPlayerV2.vue` | Video.js wrapper med smart preload og auto-poster | ~213 linjer |
| 📝 `ApplicationModal.vue` | Hele ansøgningsflowet (6 steps) med DISC-test, CV-upload og kalender | ~2418 linjer |
| 🖼️ `ResponsiveImage.vue` | Smart billedhåndtering med lazy loading og breakpoints | ~177 linjer |
| 📅 `CalendarSlotPicker.vue` | Interaktiv kalender til valg af samtaletider | ~816 linjer |
| 📅 `CustomTimeSlotPicker.vue` | Manuelle tidsintervaller (til HR) | ~277 linjer |
| 💼 `JobModal.vue` | Vis job-detaljer med video/billede og beskrivelse | ~192 linjer |
| 🔴 `FloatingApplyButton.vue` | Den røde "Ansøg nu" knap der altid er synlig | ~29 linjer |
| ✅ `ConsentModal.vue` | GDPR samtykke med focus trap | ~310 linjer |
| ❌ `ModalCloseButton.vue` | Genbrugelig luk-knap til modals | ~88 linjer |

### 5.2 Views (`src/views/`)

| View | Hvad gør den? | Størrelse |
|------|---------------|-----------|
| 🏠 `LandingPage.vue` | Forsiden med hero-video, job-cards og benefits-sektion | ~387 linjer |
| 📋 `HRDashboard.vue` | Admin-dashboard med ansøgningsliste, PDF-viewer og statistik | ~2613 linjer |

### 5.3 Samlet Statistik

| Kategori | Antal filer | Samlede linjer |
|----------|-------------|----------------|
| Komponenter | 9 | ~4,520 |
| Views | 2 | ~3,000 |
| SCSS Mixins | 1 | ~795 |
| **Total** | **12** | **~8,315** |

---

## 6. 🎨 Design System

### 6.1 Farvepalet

| Farve | Kode | Hvad bruges den til? |
|-------|------|---------------------|
| 🖤 **Mørk Grå** | `#2d2d2d` | Tekst, knapper, ikoner |
| 🔴 **Rød** | `#ee3123` | "Ansøg nu", advarsler |
| 🟡 **Gul** | `#f1db53` | Success, ledige kalenderdatoer |
| 🟢 **Grøn** | `#2ec700` | Godkendt, success states |
| ⬜ **Lys Grå** | `#ebebeb` | Baggrunde, borders |
| ⚪ **Hvid** | `#ffffff` | Cards, modal-baggrunde |

---

### 6.2 Typografi

| Font | Bruges til | Backup-fonts |
|------|------------|--------------|
| **Neo Sans** | Titler, overskrifter, knapper | System-fonts |
| **Helvetica Neue** | Brødtekst, labels | System-fonts |

> 💡 **Backup-fonts:** Hvis Adobe Fonts ikke loader, bruger vi system-fonts så teksten stadig ser god ud!

---

### 6.3 Spacing System (6px Grid)

```scss
$spacing-xs: 6px;   // 👶 Lille - mellem ikon og tekst
$spacing-sm: 12px;  // 📐 Standard - padding i knapper
$spacing-md: 18px;  // 📏 Medium - mellem elementer
$spacing-lg: 24px;  // 📐 Stor - section padding
$spacing-xl: 48px;  // 🏗️ Kæmpe - mellem store sektioner
```

---

### 6.4 Breakpoints (Skærmstørrelser)

| Navn | Bredde | Typisk enhed |
|------|--------|--------------|
| `sm` | 576px | Store telefoner |
| `md` | 768px | Tablets |
| `lg` | 990px | Små laptops |
| `xl` | 1200px | Desktop |

---

## 7. 🖥️ Backend Server (Node.js + Express)

Vores backend håndterer al data - ansøgninger, CV-filer og samtaletider.

### 7.1 Teknologier

| Teknologi | Hvad gør den? |
|-----------|---------------|
| **Express.js** | Web framework - håndterer API requests |
| **better-sqlite3** | Database - gemmer alle ansøgninger |
| **Multer** | Fil-upload - modtager CV-filer (kun PDF) |
| **Nodemailer** | Email - sender bekræftelser |
| **UUID** | Unikke ID'er - til ansøgninger og filer |

### 7.2 API Endpoints

```typescript
// 📋 Ansøgninger
POST   /api/apply              // Indsend ny ansøgning
GET    /api/applications       // Hent alle (HR kun)
GET    /api/applications/:id   // Hent én ansøgning
PATCH  /api/applications/:id   // Opdater status

// 📅 Samtaletider
GET    /api/slots              // Ledige tider (offentlig)
POST   /api/slots              // Opret ny tid (HR kun)
DELETE /api/slots/:id          // Slet tid (HR kun)

// 📄 Filer
GET    /api/cv/:filename       // Download CV (HR kun)

// 🔐 Auth
POST   /api/hr/login           // HR login
```

### 7.3 Database (SQLite)

Vi bruger SQLite - en simpel fil-baseret database. Perfekt til mindre projekter!

```javascript
// 📊 Ansøgnings-tabel
CREATE TABLE applications (
  id TEXT PRIMARY KEY,
  fullName TEXT NOT NULL,
  email TEXT NOT NULL,
  phone TEXT,
  age TEXT,
  jobPosition TEXT,
  cvFileName TEXT,
  discResult TEXT,        // JSON med DISC-score
  selectedSlots TEXT,     // JSON med valgte tider
  status TEXT DEFAULT 'pending',
  createdAt TEXT,
  expiresAt TEXT          // Auto-sletning efter 14 dage
)
```

> 💡 **Hvorfor SQLite?** Ingen separat database-server nødvendig. Databasen er bare en fil (`server/data/recruitment.db`).

---

### 7.4 🔄 Dataflow: Frontend → Backend → Database

**Hvordan hænger det hele sammen?** Her er et komplet eksempel på hvad der sker når en ansøger sender sin ansøgning:

```
┌─────────────────────────────────────────────────────────────────────────┐
│  1. FRONTEND (Vue)                                                      │
│  ───────────────────                                                    │
│  Bruger udfylder formular og trykker "Send ansøgning"                   │
│                                                                         │
│  // ApplicationModal.vue                                                │
│  const response = await api.post('/apply', formData)                    │
│                           │                                             │
│                           ▼                                             │
├─────────────────────────────────────────────────────────────────────────┤
│  2. AXIOS (HTTP Client)                                                 │
│  ───────────────────────                                                │
│  Sender HTTP POST request til backend                                   │
│                                                                         │
│  POST http://localhost:3000/api/apply                                   │
│  Headers: { 'Content-Type': 'application/json' }                        │
│  Body: { fullName, email, phone, discResult, selectedSlots, ... }       │
│                           │                                             │
│                           ▼                                             │
├─────────────────────────────────────────────────────────────────────────┤
│  3. EXPRESS.JS (Backend Server)                                         │
│  ──────────────────────────────                                         │
│  Modtager request og behandler data                                     │
│                                                                         │
│  // server/index.js                                                     │
│  app.post('/api/apply', upload.single('cv'), (req, res) => {            │
│    const { fullName, email, phone, discResult } = req.body              │
│    // Validering, generering af UUID, osv.                              │
│                           │                                             │
│                           ▼                                             │
├─────────────────────────────────────────────────────────────────────────┤
│  4. BETTER-SQLITE3 (Database)                                           │
│  ─────────────────────────────                                          │
│  Gemmer data i SQLite database-fil                                      │
│                                                                         │
│  // server/database.js                                                  │
│  db.prepare(`                                                           │
│    INSERT INTO applications (id, fullName, email, ...)                  │
│    VALUES (?, ?, ?, ...)                                                │
│  `).run(id, fullName, email, ...)                                       │
│                           │                                             │
│                           ▼                                             │
├─────────────────────────────────────────────────────────────────────────┤
│  5. RESPONSE TILBAGE                                                    │
│  ───────────────────                                                    │
│  Database → Express → Axios → Vue                                       │
│                                                                         │
│  res.json({ success: true, applicationId: id })                         │
│                           │                                             │
│                           ▼                                             │
│  Frontend viser "Tak for din ansøgning!" besked                         │
└─────────────────────────────────────────────────────────────────────────┘
```

**Konkret kodeeksempel - Hele flowet:**

```typescript
// ═══════════════════════════════════════════════════════════════════════
// 1. FRONTEND: ApplicationModal.vue sender ansøgning
// ═══════════════════════════════════════════════════════════════════════
const submitApplication = async () => {
  const formData = new FormData()
  formData.append('fullName', form.fullName)
  formData.append('email', form.email)
  formData.append('discResult', JSON.stringify(discResult.value))
  formData.append('cv', cvFile.value)  // PDF fil
  
  // 📤 Axios sender til backend
  const response = await api.post('/apply', formData)
  
  if (response.data.success) {
    showSuccessMessage()  // 🎉 Vis "Tak for din ansøgning!"
  }
}
```

```javascript
// ═══════════════════════════════════════════════════════════════════════
// 2. BACKEND: server/index.js modtager og gemmer
// ═══════════════════════════════════════════════════════════════════════
const multer = require('multer')     // Håndterer fil-uploads
const { v4: uuidv4 } = require('uuid')
const db = require('./database')

// Multer konfiguration - gemmer CV i /uploads mappe
const upload = multer({ dest: 'uploads/' })

app.post('/api/apply', upload.single('cv'), (req, res) => {
  const id = uuidv4()  // Generér unikt ID
  const { fullName, email, phone, discResult, selectedSlots } = req.body
  const cvFileName = req.file?.filename  // CV fil-navn
  
  // 💾 Gem i database
  db.prepare(`
    INSERT INTO applications 
    (id, fullName, email, phone, cvFileName, discResult, selectedSlots, status, createdAt)
    VALUES (?, ?, ?, ?, ?, ?, ?, 'pending', datetime('now'))
  `).run(id, fullName, email, phone, cvFileName, discResult, selectedSlots)
  
  // ✅ Send svar tilbage til frontend
  res.json({ success: true, applicationId: id })
})
```

```javascript
// ═══════════════════════════════════════════════════════════════════════
// 3. DATABASE: server/database.js opsætning
// ═══════════════════════════════════════════════════════════════════════
const Database = require('better-sqlite3')
const db = new Database('data/recruitment.db')

// Opret tabeller hvis de ikke findes
db.exec(`
  CREATE TABLE IF NOT EXISTS applications (
    id TEXT PRIMARY KEY,
    fullName TEXT NOT NULL,
    email TEXT NOT NULL,
    -- ... flere felter
  )
`)

module.exports = db
```

**Opsummering af rollerne:**

| Teknologi | Rolle | Fil |
|-----------|-------|-----|
| **Axios** | 📤 Sender HTTP requests fra browser til server | `src/config/api.ts` |
| **Express.js** | 🔀 Modtager requests, validerer, router til rigtig handler | `server/index.js` |
| **Multer** | 📁 Håndterer fil-uploads (kun PDF) | `server/index.js` |
| **better-sqlite3** | 💾 Læser/skriver data til database-fil | `server/database.js` |

> 🎯 **Nøglepunktet:** Axios er *kun* HTTP-kommunikation. Den ved intet om databasen! Express-serveren er "mellemmanden" der oversætter HTTP-requests til database-operationer.

---

## 8. 🚀 Performance (Hastighed)

Her er et overblik over alle vores optimeringsteknikker:

| Optimering | Hvad gør vi? | Resultat |
|------------|--------------|----------|
| **LCP** | Preloader hero-billede | Første billede vises hurtigt |
| **CLS** | Pre-render alle billedstørrelser | Ingen "hoppende" layout |
| **FID** | Passive event listeners | Smooth scrolling |
| **Bundle** | Tree-shaking, code splitting | Mindre filer at downloade |
| **Caching** | Opdeler i chunks | Browser husker mere |
| **Media** | WebP, lazy loading | Mindre data at overføre |
| **Routes** | Lazy loading af admin-side | Hurtigere for ansøgere |
| **Fonts** | FOUT prevention | Ingen font-hop |

> 💡 **LCP, CLS, FID** er Google's "Core Web Vitals" - de vigtigste målinger for hastighed!
>
> 📖 **Læs mere:** [web.dev/vitals](https://web.dev/vitals/)

---

## 8.1 🚫 Søgemaskine & AI Crawler Blokering

**Problemet:** Siden er ikke solgt endnu, så vi vil ikke have at den dukker op i Google-søgninger eller bruges til at træne AI-modeller.

**Løsningen:** Vi bruger to teknikker der arbejder sammen:

### 1️⃣ Meta robots tags (index.html)

```html
<!-- 🚫 Bloker søgemaskiner og AI chatbots -->
<meta name="robots" content="noindex, nofollow, noarchive, noimageindex">
<meta name="googlebot" content="noindex, nofollow">
<meta name="GPTBot" content="noindex, nofollow">
<meta name="anthropic-ai" content="noindex, nofollow">
```

**Hvad betyder de forskellige værdier?**
| Værdi | Betydning |
|-------|-----------|
| `noindex` | Vis ikke siden i søgeresultater |
| `nofollow` | Følg ikke links på siden |
| `noarchive` | Gem ikke en cached version |
| `noimageindex` | Indexér ikke billeder fra siden |

📖 **Kilde:** [Google - Robots meta tag](https://developers.google.com/search/docs/crawling-indexing/robots-meta-tag)

### 2️⃣ robots.txt (public/robots.txt)

```txt
# Bloker alle crawlere
User-agent: *
Disallow: /

# Bloker AI crawlere specifikt
User-agent: GPTBot        # OpenAI / ChatGPT
Disallow: /

User-agent: anthropic-ai  # Anthropic / Claude
Disallow: /

User-agent: CCBot         # Common Crawl (bruges af mange AI)
Disallow: /
```

**Hvilke AI crawlere blokerer vi?**
| Crawler | Firma | Kilde |
|---------|-------|-------|
| `GPTBot` | OpenAI (ChatGPT) | [openai.com/gptbot](https://platform.openai.com/docs/gptbot) |
| `ChatGPT-User` | OpenAI | [openai.com/gptbot](https://platform.openai.com/docs/gptbot) |
| `anthropic-ai` | Anthropic (Claude) | [anthropic.com](https://www.anthropic.com/) |
| `CCBot` | Common Crawl | [commoncrawl.org](https://commoncrawl.org/ccbot) |
| `Google-Extended` | Google (Bard/Gemini AI) | [developers.google.com](https://developers.google.com/search/docs/crawling-indexing/overview-google-crawlers) |
| `PerplexityBot` | Perplexity AI | [perplexity.ai](https://docs.perplexity.ai/docs/perplexitybot) |
| `Bytespider` | ByteDance (TikTok AI) | [bytedance.com](https://www.bytedance.com/) |

📖 **Kilde:** [Google - robots.txt specifikation](https://developers.google.com/search/docs/crawling-indexing/robots/intro)

### 🔓 Når siden er klar til lancering

**Fjern blokering ved at:**

1. **index.html** - Slet alle `<meta name="robots"...>` tags
2. **public/robots.txt** - Erstat med:
```txt
User-agent: *
Allow: /
Sitemap: https://karriere.sbs-friction.dk/sitemap.xml
```

> ⚠️ **Vigtigt:** `robots.txt` er kun en *anmodning* - ikke alle crawlere respekterer den! Meta tags giver stærkere beskyttelse fordi de er i selve HTML'en.

---

## 9. ♿ Tilgængelighed (WCAG 2.1 AA)

Vi har implementeret omfattende tilgængelighed for at sikre siden er brugbar for alle - også mennesker med handicap.

📖 **WCAG Retningslinjer:** [w3.org/WAI/WCAG21](https://www.w3.org/WAI/WCAG21/quickref/)  
📖 **Fuld WCAG Audit:** Se `plans/wcag_2-1_aa_plan.md` for komplet status

---

### 9.1 Implementerede WCAG Kriterier

| WCAG Ref | Kriterium | Implementation | Fil(er) |
|----------|-----------|----------------|---------|
| **2.4.1** | Bypass Blocks | Skip link der springer til hovedindhold | `LandingPage.vue`, `_utilities.scss` |
| **4.1.2** | Name, Role, Value | ARIA dialog på modals, role/aria-label på job cards | `JobModal.vue`, `LandingPage.vue` |
| **2.3.3** | Animation from Interactions | `prefers-reduced-motion` media query | `_reset.scss` |
| **4.1.3** | Status Messages | Global `aria-live` region + announce utility | `App.vue`, `utils/announce.ts` |
| **1.2.1** | Audio-only/Video-only | Screen reader beskrivelse af hero video | `LandingPage.vue` |
| **1.3.5** | Identify Input Purpose | Native HTML inputs med `autocomplete` + korrekt `type` | `ApplicationModal.vue`, `_forms.scss` |
| **2.1.1** | Keyboard | Fuld tastatur-navigation på job cards og modals | `LandingPage.vue`, `JobModal.vue` |
| **2.4.7** | Focus Visible | Konsistent 2px gul border på alle interaktive elementer | `_forms.scss`, `_cards.scss`, `_buttons.scss` |
| **1.4.3** | Contrast (Minimum) | Mørk tekst på lys baggrund (4.5:1 ratio) | Design system |

---

### 9.2 Skip Link (2.4.1 Bypass Blocks)

Brugere kan springe direkte til hovedindholdet ved at trykke Tab:

```vue
<!-- LandingPage.vue -->
<a href="#main-content" class="skip-link" @click="skipToMain">
  Spring til hovedindhold
</a>

<main id="main-content" tabindex="-1">
  <!-- Indhold -->
</main>
```

```scss
// _utilities.scss - Styled som light button
.skip-link {
  @include button-base;
  @include button-colors-light;
  position: fixed;
  top: -100%;  // Skjult som udgangspunkt
  
  &:focus {
    top: $spacing-md;  // Vises ved tastatur-fokus
  }
}
```

> 💡 **Hvorfor click handler?** Vi bruger OverlayScrollbars, så native anchor-navigation virker ikke. `skipToMain()` scroller viewporten manuelt.

---

### 9.3 Modal Tilgængelighed (4.1.2)

Modals har fuld ARIA-support og fokus-fælde:

```vue
<!-- JobModal.vue -->
<el-dialog
  role="dialog"
  aria-modal="true"
  :aria-labelledby="selectedJob ? 'job-modal-title' : undefined"
  @keydown="handleKeydown"
>
  <h2 :id="'job-modal-title'">{{ selectedJob?.title }}</h2>
</el-dialog>
```

```typescript
// Fokus-fælde holder brugeren inde i modal
const handleFocusTrap = (e: KeyboardEvent) => {
  const focusableElements = modalRef.value?.querySelectorAll(
    'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
  )
  // Tab-cycling mellem første og sidste element
}

// ESC lukker modal
const handleKeydown = (e: KeyboardEvent) => {
  if (e.key === 'Escape') closeModal()
  if (e.key === 'Tab') handleFocusTrap(e)
}
```

---

### 9.4 Job Card Tastatur-Navigation (2.1.1)

Job cards kan fokuseres og aktiveres med tastatur:

```vue
<!-- LandingPage.vue -->
<article
  class="job-card"
  tabindex="0"
  role="button"
  :aria-label="`Åbn ${job.title} jobopslag`"
  @click="openJobModal(job)"
  @keydown.enter.self.prevent="openJobModal(job)"
  @keydown.space.self.prevent="openJobModal(job)"
>
```

> ⚠️ **`.self.prevent` modifiers:** Forhindrer event bubbling fra nested buttons og default browser-adfærd.

---

### 9.5 Reduced Motion (2.3.3)

Respekterer brugerens system-præference for reduceret bevægelse:

```scss
// _reset.scss
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

### 9.6 ARIA Live Region (4.1.3)

Global announcer til skærmlæsere:

```vue
<!-- App.vue -->
<div 
  id="a11y-announcer"
  aria-live="polite"
  aria-atomic="true"
  class="sr-only"
></div>
```

```typescript
// utils/announce.ts
export function announce(message: string, priority: 'polite' | 'assertive' = 'polite') {
  const announcer = document.getElementById('a11y-announcer')
  if (announcer) {
    announcer.setAttribute('aria-live', priority)
    announcer.textContent = ''
    setTimeout(() => { announcer.textContent = message }, 100)
  }
}

export function announceError(message: string) {
  announce(message, 'assertive')
}
```

---

### 9.7 Focus-Visible Styling (2.4.7)

**Alle interaktive elementer** har konsistent 2px gul focus-styling:

```scss
// _forms.scss - Konsistent focus på ALLE form elementer
.el-input__wrapper:has(:focus),
.el-input__wrapper:has(:focus-visible),
.el-select__wrapper:has(:focus),
.el-select__wrapper:has(:focus-visible),
.el-select__wrapper.is-focused,
.el-textarea__inner:focus,
.el-textarea__inner:focus-visible {
  box-shadow: inset 0 0 0 2px $c-warning !important;
  border-color: transparent !important;
}

// Native inputs (brugt for autofill kompatibilitet)
.el-form-item input[type='text'],
.el-form-item input[type='tel'],
.el-form-item input[type='email'] {
  &:focus,
  &:focus-visible {
    border-color: transparent;
    box-shadow: inset 0 0 0 2px $c-warning;
    outline: none;
  }
}
```

```scss
// _cards.scss - Job cards med hover-animation på fokus
.job-card {
  @include focus-visible;  // Gul outline
  
  // Hover-animation også på keyboard-fokus
  &:focus-visible {
    transform: translateY(-4px);
    box-shadow: $shadow-card-hover;
  }
}

// _mixins.scss
@mixin focus-visible {
  &:focus-visible {
    outline: 2px solid $c-warning;
    outline-offset: 2px;
  }
}
```

---

### 9.8 Native HTML Inputs for Autofill (1.3.5)

**Problemet:** Element Plus `<el-input>` komponenter er custom web components der kan bryde browser-autofill. Chrome fyldte "Hr" (honorific prefix) i alle felter i stedet for korrekte værdier.

**Løsningen:** Vi bruger native HTML `<input>` elementer for navn, telefon og email - med styling der matcher Element Plus:

```vue
<!-- ApplicationModal.vue -->
<el-form-item label="Fulde navn" prop="fullName" required>
  <input
    v-model="formData.fullName"
    id="fullName"
    name="fullName"
    type="text"
    placeholder="Skriv her..."
    autocomplete="name"
    class="el-input__inner"
  />
</el-form-item>

<el-form-item label="Telefonnummer" prop="phone" required>
  <input
    v-model="formData.phone"
    id="phone"
    name="phone"
    type="tel"
    placeholder="Skriv her..."
    autocomplete="tel"
    class="el-input__inner"
  />
</el-form-item>

<el-form-item label="E-mail" prop="email" required>
  <input
    v-model="formData.email"
    id="email"
    name="email"
    type="email"
    placeholder="Skriv her..."
    autocomplete="email"
    class="el-input__inner"
  />
</el-form-item>
```

**Styling:** Native inputs styles i `_forms.scss` for at matche Element Plus visuelt:
- Font: `$font-body` (Helvetica Neue)
- Størrelse: `$font-size-input` (16px - forhindrer iOS Safari auto-zoom)
- Border radius: `$border-radius-sm` (6px)
- Focus: 2px gul border (`$c-warning`) - konsistent med dropdowns

**Nøglepunkter fra [web.dev best practices](https://web.dev/articles/payment-and-address-form-best-practices):**
- Native `<input>` giver browser direkte adgang til autocomplete
- `type="tel"` og `type="email"` giver yderligere hints til browser
- Stabile `id` og `name` attributter (ikke dynamiske)
- Undgå custom elements til autofillable felter

---

### 9.9 Screen Reader Only Utility

```scss
// _utilities.scss
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

Bruges til tekst der kun skal læses af skærmlæsere:
```vue
<span class="sr-only">Video: Præsentation af arbejdspladsen hos SBS Friction</span>
```

---

## 10. 🛠️ Udviklings-Workflow

### NPM Scripts (Kommandoer)

| Kommando | Hvad gør den? |
|----------|---------------|
| `npm run dev` | Start frontend udviklingsserver (Vite) |
| `npm run dev:server` | Start backend server med --watch |
| `npm run dev:all` | Kør frontend + backend samtidig (concurrently) |
| `npm run build` | Byg til produktion (kører automatisk poster-generering først) |
| `npm run preview` | Test produktion lokalt |
| `npm run type-check` | Tjek TypeScript for fejl |
| `npm run start` | Start produktion server (Linux/Mac) |
| `npm run start:win` | Start produktion server (Windows) |
| `npm run prod` | Byg og start produktion |
| `npm run generate-posters` | Generer poster-billeder fra videoer manuelt |

### Dev Server Proxy

```typescript
// 🔄 API-kald sendes automatisk til backend
proxy: {
  '/api': {
    target: 'http://localhost:3000'
  }
}
```

> 💡 **Hvad betyder det?** Når frontend kalder `/api/jobs`, sender Vite det videre til backend på port 3000. Ingen CORS-problemer!

---

## 11. 🧩 Sådan Fungerer Koden (For Begyndere)

Her forklarer vi de grundlæggende koncepter - så alle kan forstå hvordan projektet hænger sammen!

---

### 10.1 📦 Hvad er npm?

**npm** (Node Package Manager) er som en "app store" for kode-biblioteker.

```bash
# Installer alle projektets dependencies (første gang)
npm install

# Start udviklingsserver
npm run dev

# Byg til produktion
npm run build
```

**package.json** er projektets "opskrift" - den fortæller npm hvilke biblioteker vi bruger:
```json
{
  "dependencies": {
    "vue": "^3.4.0",          // Frontend framework
    "element-plus": "^2.4.4", // UI komponenter
    "video.js": "^7.21.5"     // Videospiller
  }
}
```

📖 **Læs mere:** [npm dokumentation](https://docs.npmjs.com/)

---

### 10.2 ⚡ Hvad er Vite?

**Vite** er vores bygge-værktøj. Det gør to ting:

1. **Development:** Starter en lokal server med "hot reload" (ændringer vises med det samme!)
2. **Production:** Bygger optimerede filer til hosting

```typescript
// vite.config.ts - Vores konfiguration
export default defineConfig({
  plugins: [vue()],           // 👈 Aktiver Vue support
  server: { port: 5173 },     // 👈 Lokal server på port 5173
  build: { outDir: 'dist' }   // 👈 Byg til /dist mappe
})
```

**Hot Module Replacement (HMR):** Når du gemmer en fil, opdaterer Vite kun den ændrede del - uden at genindlæse hele siden!

📖 **Læs mere:** [Vite Guide](https://vitejs.dev/guide/)

---

### 10.3 🟢 Hvad er Vue 3?

**Vue** er et framework der gør det nemt at bygge interaktive websider. 

**Grundkoncepter:**

#### 1. Komponenter = Genbrugelige Dele
En komponent er en selvstændig del af UI'et (knap, modal, kort, osv.)

```vue
<!-- MyButton.vue -->
<template>
  <button class="my-button">{{ label }}</button>
</template>

<script setup lang="ts">
defineProps<{ label: string }>()  // 👈 Input fra parent
</script>
```

#### 2. Reaktivitet = Automatiske Opdateringer
```typescript
const count = ref(0)  // 👈 Reaktiv variabel

// Når count ændres, opdaterer Vue automatisk alle steder den bruges!
count.value++  // UI opdaterer sig selv!
```

#### 3. Computed = Smarte Variabler
```typescript
const firstName = ref('Anders')
const lastName = ref('And')

// 👈 Opdaterer sig selv når firstName eller lastName ændres!
const fullName = computed(() => `${firstName.value} ${lastName.value}`)
```

#### 4. Props = Input til Komponenter
```typescript
// Parent sender data ned til child
<VideoPlayer src="/video.mp4" :autoplay="true" />

// Child modtager data
const props = defineProps<{ src: string, autoplay: boolean }>()
```

#### 5. Emits = Output fra Komponenter
```typescript
// Child sender events op til parent
const emit = defineEmits<{ play: [], pause: [] }>()
emit('play')  // 👈 Fortæl parent at video startede

// Parent lytter efter events
<VideoPlayer @play="handlePlay" @pause="handlePause" />
```

📖 **Læs mere:** [Vue 3 Dokumentation](https://vuejs.org/guide/introduction.html)

---

### 10.4 📝 Hvad er TypeScript?

**TypeScript** er JavaScript med "typer" - det fortæller koden hvilken slags data den arbejder med.

```typescript
// ❌ JavaScript - ingen sikkerhed
function greet(name) {
  return "Hello " + name.toUppercase()  // Stavefejl! Crasher ved runtime
}

// ✅ TypeScript - fejl fanges med det samme!
function greet(name: string): string {
  return "Hello " + name.toUpperCase()  // 👈 Editor foreslår korrekt metode
}
```

**Interfaces = Skabeloner for data:**
```typescript
// Definer hvordan en ansøgning skal se ud
interface Application {
  fullName: string      // Påkrævet tekst
  email: string         // Påkrævet tekst
  age?: number          // Valgfri (?) tal
  status: 'pending' | 'approved' | 'rejected'  // Kun disse 3 værdier!
}

// Nu får vi fejl hvis vi glemmer et felt eller staver forkert!
const app: Application = {
  fullName: 'Anders And',
  email: 'anders@and.dk',
  status: 'pending'
}
```

📖 **Læs mere:** [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)

---

### 10.5 🎨 Hvad er SCSS?

**SCSS** er CSS med superkræfter - variabler, funktioner og nesting!

```scss
// Variabler - ændr ét sted, opdater overalt!
$primary-color: #2d2d2d;
$spacing: 12px;

// Nesting - mere læsbar kode
.card {
  padding: $spacing;
  
  &__title {           // Bliver til .card__title
    color: $primary-color;
  }
  
  &:hover {            // Bliver til .card:hover
    transform: scale(1.02);
  }
}

// Mixins - genbrugelige style-blokke
@mixin button-style {
  padding: $spacing;
  border-radius: 4px;
  cursor: pointer;
}

.btn {
  @include button-style;  // 👈 Indsætter alle styles fra mixin
}
```

📖 **Læs mere:** [Sass Dokumentation](https://sass-lang.com/documentation/)

---

## 12. 🎯 Konklusion

Dette projekt viser avanceret frontend-udvikling på flere niveauer:

1. **🏗️ Moderne Vue 3** - Composition API, TypeScript, `<script setup>`
2. **⚡ Performance** - Custom plugins, lazy loading, code splitting
3. **🎨 Design System** - SCSS-arkitektur, mixins, CSS Custom Properties
4. **♿ Tilgængelighed** - Tastatur, ARIA, undertekster, fokus-synlighed
5. **📱 Cross-platform** - Touch vs. mus, iOS workarounds, scrollbar-normalisering
6. **🤖 Automatisering** - Poster-generering, preload-injection, video-konvertering
7. **✨ UX** - Multi-step wizard, animationer, form-validering
8. **🎯 Branding** - Konsistent farvepalet, custom fonts

> 🏆 **Kort sagt:** Vi har bygget en professionel rekrutteringsplatform der er hurtig, tilgængelig og ser fantastisk ud!

---

## 📚 Appendix: Filreferencer

Hurtig oversigt over hvor du finder hvad:

| Emne | Fil |
|------|-----|
| **Dokumentation** | |
| Projekt Dokumentation | `plans/projekt_dokumentation.md` |
| WCAG 2.1 AA Audit | `plans/wcag_2-1_aa_plan.md` |
| **Konfiguration** | |
| Vite Config | `vite.config.ts` |
| TypeScript Config | `tsconfig.json` |
| Package Dependencies | `package.json` |
| **SCSS / Styling** | |
| Farver & Spacing | `src/assets/scss/abstracts/_variables.scss` |
| Mixins (~795 linjer) | `src/assets/scss/abstracts/_mixins.scss` |
| Form Styling (~150 linjer) | `src/assets/scss/components/_forms.scss` |
| Element Plus Theming | `src/assets/scss/vendors/_element-plus-vars.scss` |
| Video.js Styling | `src/assets/scss/vendors/_video-js.scss` |
| A11y Utilities | `src/assets/scss/base/_utilities.scss` |
| **TypeScript** | |
| Type Definitions | `src/types/index.ts` |
| DISC Spørgsmål | `src/config/discQuestions.ts` |
| API Config | `src/config/api.ts` |
| ARIA Announcer Utility | `src/utils/announce.ts` |
| **Routing** | |
| Router | `src/router/index.ts` |
| **Scripts & Plugins** | |
| Critical Media Plugin | `plugins/vite-plugin-critical-media.ts` |
| Video Konvertering | `scripts/convert-videos-8bit.js` |
| Poster Generator | `scripts/generate-posters.js` |
| **Komponenter** | |
| VideoPlayerV2 (Video.js, ~213 linjer) | `src/components/VideoPlayerV2.vue` |
| ResponsiveImage (~177 linjer) | `src/components/ResponsiveImage.vue` |
| ApplicationModal (~2440 linjer) | `src/components/ApplicationModal.vue` |
| CalendarSlotPicker (~816 linjer) | `src/components/CalendarSlotPicker.vue` |
| ConsentModal (~310 linjer) | `src/components/ConsentModal.vue` |
| JobModal (~192 linjer) | `src/components/JobModal.vue` |
| **Views** | |
| LandingPage (~387 linjer) | `src/views/LandingPage.vue` |
| HRDashboard (~2613 linjer) | `src/views/HRDashboard.vue` |
| **Backend** | |
| Express Server | `server/index.js` |
| SQLite Database | `server/database.js` |
| Dummy Data Seeding | `server/seed-dummy-data.js` |
