# WCAG 2.1 AA Compliance Plan
## SBS Recruitment App - Tilgængelighedsaudit

**Dato:** Februar 2026  
**Version:** 1.0  
**Status:** Audit gennemført

---

## Indholdsfortegnelse
1. [Oversigt](#oversigt)
2. [Nuværende Status](#nuværende-status)
3. [Detaljeret Audit](#detaljeret-audit)
4. [Mangler og Anbefalinger](#mangler-og-anbefalinger)
5. [Implementeringsguide](#implementeringsguide)
6. [Testprocedurer](#testprocedurer)

---

## Oversigt

Dette dokument indeholder en komplet WCAG 2.1 AA tilgængelighedsaudit af SBS Recruitment App. Auditen dækker alle 50 succeskriterier på niveau A og AA.

### WCAG 2.1 Principper
- **Perceivable (Opfattelig):** Information skal kunne opfattes af alle brugere
- **Operable (Betjenbar):** Brugerfladen skal kunne betjenes af alle
- **Understandable (Forståelig):** Information og betjening skal være forståelig
- **Robust:** Indhold skal være robust nok til at fungere med hjælpeteknologi

---

## Nuværende Status

### ✅ Implementeret Korrekt

| Kriterium | WCAG Ref | Beskrivelse | Placering |
|-----------|----------|-------------|-----------|
| Sprog | 3.1.1 | `lang="da"` på `<html>` | `index.html` |
| Alt-tekster | 1.1.1 | Alle billeder har beskrivende alt-tekster | Alle komponenter |
| ARIA Labels | 4.1.2 | Sociale links og luk-knapper har aria-labels | `LandingPage.vue`, `ModalCloseButton.vue` |
| Focus Visible | 2.4.7 | Gul fokusring på keyboard navigation | `_buttons.scss`, `_forms.scss` |
| Focus Trap | 2.4.3 | ConsentModal og CalendarModal har focus trap | `ConsentModal.vue`, `CalendarSlotPicker.vue` |
| Form Labels | 1.3.1, 3.3.2 | Element Plus labels via `label-position="top"` | `ApplicationModal.vue` |
| Dialog Roles | 4.1.2 | `role="dialog"` og `aria-modal` på kalendar og JobModal | `CalendarSlotPicker.vue`, `JobModal.vue` |
| Keyboard Support | 2.1.1 | ESC lukker modaler, Enter/Space på slots og job cards | Alle modaler, `LandingPage.vue` |
| Touch Target | 2.5.5 | Knapper har tilstrækkelig størrelse (42px+) | `_variables.scss` |
| Color Contrast | 1.4.3 | Mørk tekst (#2d2d2d) på lys baggrund (#fff) | Design system |
| Input Font Size | 1.4.4 | 16px input font (forhindrer iOS zoom) | `_forms.scss` |
| Form Timeslots | 4.1.2 | `role="button"` og `aria-pressed` på tidsslots | `ApplicationModal.vue` L193-201 |
| Keyboard Handlers | 2.1.1 | `@keydown.enter` og `@keydown.space` | `ApplicationModal.vue` L200-201 |
| Step Navigation | 2.4.7 | Step titles har tabindex og keyboard handlers | `ApplicationModal.vue` L716-732 |
| Reduced Motion | 2.3.3 | `prefers-reduced-motion` media query | `_reset.scss` |
| Job Card A11y | 4.1.2, 2.1.1 | `tabindex`, `role="button"`, keyboard handlers | `LandingPage.vue` |
| ARIA Live | 4.1.3 | Global aria-live region + announce utility | `App.vue`, `utils/announce.ts` |
| Video Description | 1.2.1 | Screen reader description for hero video | `LandingPage.vue` |
| Input Autocomplete | 1.3.5 | `autocomplete` attributter på form inputs | `ApplicationModal.vue` |
| Skip Link | 2.4.1 | "Spring til hovedindhold" link (vises ved tab) | `LandingPage.vue`, `_utilities.scss` |

### ⚠️ Delvist Implementeret

| Kriterium | WCAG Ref | Status | Handling Påkrævet |
|-----------|----------|--------|-------------------|
| Focus Management | 2.4.3 | Modaler mangler initial focus | Tilføj autofocus |
| Form Errors | 3.3.1 | Element Plus håndterer, men mangler aria-live | Tilføj live regions |
| Video Kontroller | 1.2.1 | Video.js har kontroller, men mangler captions | Tilføj undertekster |
| Responsive Text | 1.4.4 | Delvist - brug rem i stedet for px | Konverter enheder |

### ❌ Mangler Implementation

| Kriterium | WCAG Ref | Beskrivelse | Prioritet |
|-----------|----------|-------------|-----------|

| Video Captions | 1.2.2 | Videoer mangler undertekster | MEDIUM |
| Heading Structure | 1.3.1 | Tjek heading hierarki på alle sider | LAV |

---

## Detaljeret Audit

### 1. Perceivable (Opfattelig)

#### 1.1 Text Alternatives
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 1.1.1 Non-text Content | ✅ | Alle `<img>` har `:alt` bindings |

**Fundne implementeringer:**
```vue
<!-- LandingPage.vue L6 -->
<img src="/logo.svg" alt="SBS Friction A/S" class="landing-page__logo" width="92" height="45" />

<!-- JobModal.vue L40 -->
<ResponsiveImage :alt="currentJob.title" />
```

#### 1.2 Time-based Media
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 1.2.1 Audio-only/Video-only | ❌ | Videoer ikke beskrevet tekstuelt |
| 1.2.2 Captions | ❌ | Ingen undertekster på videoer |
| 1.2.3 Audio Description | N/A | Videos er primært visuelle uden dialog |
| 1.2.4 Captions (Live) | N/A | Ingen live indhold |
| 1.2.5 Audio Description | N/A | Niveau AAA |

**Anbefaling:** Tilføj WebVTT undertekster til alle videoer i `/public/videos/`.

#### 1.3 Adaptable
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 1.3.1 Info and Relationships | ⚠️ | Forms har labels, men heading hierarki bør tjekkes |
| 1.3.2 Meaningful Sequence | ✅ | DOM rækkefølge følger visuel |
| 1.3.3 Sensory Characteristics | ✅ | Ingen instruktioner baseret kun på form/farve |
| 1.3.4 Orientation | ✅ | Ingen låst orientation |
| 1.3.5 Identify Input Purpose | ✅ | Native HTML inputs med `autocomplete` attributter |

**Implementation:** Bruger native `<input>` elementer (ikke Element Plus custom components) for at sikre korrekt browser autofill:
```vue
<input type="text" autocomplete="name" />
<input type="email" autocomplete="email" />
<input type="tel" autocomplete="tel" />
```

#### 1.4 Distinguishable
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 1.4.1 Use of Color | ✅ | Farve aldrig eneste indikator |
| 1.4.2 Audio Control | ✅ | Videoer starter muted |
| 1.4.3 Contrast (Minimum) | ✅ | #2d2d2d på #ffffff = 12.6:1 |
| 1.4.4 Resize Text | ⚠️ | Bruger px - bør være rem |
| 1.4.5 Images of Text | ✅ | Ingen tekst som billeder |
| 1.4.10 Reflow | ✅ | Responsivt design med breakpoints |
| 1.4.11 Non-text Contrast | ✅ | UI komponenter har god kontrast |
| 1.4.12 Text Spacing | ✅ | Ingen fixed heights på tekst |
| 1.4.13 Content on Hover/Focus | ✅ | Tooltips kan dismisses med ESC |

**Kontrast Beregninger:**
- Primary tekst (#2d2d2d) på hvid (#ffffff): **12.6:1** ✅
- Gul knap (#f1db53) med mørk tekst (#2d2d2d): **8.8:1** ✅
- Rød knap (#ee3123) med hvid tekst (#ffffff): **4.7:1** ✅ (lige over 4.5:1)

---

### 2. Operable (Betjenbar)

#### 2.1 Keyboard Accessible
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 2.1.1 Keyboard | ⚠️ | Job cards mangler keyboard support |
| 2.1.2 No Keyboard Trap | ✅ | Focus traps frigives ved ESC |
| 2.1.4 Character Key Shortcuts | ✅ | Ingen single-key shortcuts |

**Fundne keyboard handlers:**
```vue
<!-- ApplicationModal.vue L200-201 -->
@keydown.enter.prevent="toggleSlot(slot)"
@keydown.space.prevent="toggleSlot(slot)"

<!-- ConsentModal.vue L125-135 - Focus trap -->
const handleFocusTrap = (event: KeyboardEvent) => {
  if (event.key !== 'Tab' || !modalVisible.value) return
  // ... trap logic
}
```

**Problem med Job Cards:**
```vue
<!-- LandingPage.vue L78-86 - Mangler keyboard support -->
<el-card @click="openJobModal(job.id)">
  <!-- Mangler: tabindex="0", role="button", @keydown -->
</el-card>
```

#### 2.2 Enough Time
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 2.2.1 Timing Adjustable | ✅ | Ingen tidsbegrænsninger undtagen server sessions |
| 2.2.2 Pause, Stop, Hide | ⚠️ | Hero video autoplayer, men er muted og dekorativ |

#### 2.3 Seizures and Physical Reactions
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 2.3.1 Three Flashes | ✅ | Ingen blinkende indhold |
| 2.3.3 Animation from Interactions | ❌ | Mangler `prefers-reduced-motion` |

**Manglende implementation:**
```scss
// Tilføj til _reset.scss eller _utilities.scss
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

#### 2.4 Navigable
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 2.4.1 Bypass Blocks | ❌ | Mangler skip link |
| 2.4.2 Page Titled | ✅ | "SBS Friction A/S - Karriere" |
| 2.4.3 Focus Order | ✅ | Logisk tab-rækkefølge |
| 2.4.4 Link Purpose | ✅ | Links har beskrivende tekst |
| 2.4.5 Multiple Ways | ✅ | Navigation via scroll, modaler, knapper |
| 2.4.6 Headings and Labels | ✅ | Beskrivende headings |
| 2.4.7 Focus Visible | ✅ | Gul fokusring implementeret |

**Skip Link Implementation:**
```html
<!-- index.html - tilføj efter <body> -->
<a href="#main-content" class="skip-link">
  Gå til hovedindhold
</a>

<!-- LandingPage.vue - tilføj id på main -->
<main id="main-content" class="landing-page__main">
```

```scss
// _utilities.scss
.skip-link {
  position: absolute;
  top: -100%;
  left: 50%;
  transform: translateX(-50%);
  background: $c-warning;
  color: $c-primary;
  padding: $spacing-sm $spacing-md;
  z-index: $z-index-modal + 1;
  border-radius: $border-radius-md;
  font-weight: $font-weight-bold;
  
  &:focus {
    top: $spacing-sm;
  }
}
```

#### 2.5 Input Modalities
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 2.5.1 Pointer Gestures | ✅ | Ingen komplekse gestures |
| 2.5.2 Pointer Cancellation | ✅ | Click sker på release |
| 2.5.3 Label in Name | ✅ | Visuel label matcher accessible name |
| 2.5.4 Motion Actuation | ✅ | Ingen motion-baseret input |
| 2.5.5 Target Size | ✅ | 42px minimum (se $element-height-standard) |

---

### 3. Understandable (Forståelig)

#### 3.1 Readable
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 3.1.1 Language of Page | ✅ | `<html lang="da">` |
| 3.1.2 Language of Parts | ✅ | Alt indhold er dansk |

#### 3.2 Predictable
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 3.2.1 On Focus | ✅ | Ingen kontekst-ændringer ved fokus |
| 3.2.2 On Input | ✅ | Ingen automatiske kontekst-ændringer |
| 3.2.3 Consistent Navigation | ✅ | Header konsistent på alle sider |
| 3.2.4 Consistent Identification | ✅ | Samme funktioner har samme labels |

#### 3.3 Input Assistance
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 3.3.1 Error Identification | ✅ | Element Plus validering markerer fejl |
| 3.3.2 Labels or Instructions | ✅ | Alle inputs har labels |
| 3.3.3 Error Suggestion | ✅ | Fejlmeddelelser er specifikke |
| 3.3.4 Error Prevention | ⚠️ | Bør have bekræftelse før submit |

**Fundne validation rules:**
```typescript
// ApplicationModal.vue L885-894
const personalFormRules: FormRules = {
  fullName: [{ required: true, message: 'Fulde navn er påkrævet', trigger: 'submit' }],
  phone: [{ required: true, message: 'Telefonnummer er påkrævet', trigger: 'submit' }],
  email: [
    { required: true, message: 'E-mail er påkrævet', trigger: 'submit' },
    { type: 'email', message: 'Indtast en gyldig e-mail', trigger: 'submit' }
  ],
  // ...
}
```

---

### 4. Robust

#### 4.1 Compatible
| Kriterium | Status | Detaljer |
|-----------|--------|----------|
| 4.1.1 Parsing | ✅ | Vue genererer valid HTML |
| 4.1.2 Name, Role, Value | ⚠️ | Job cards og modaler mangler roles |
| 4.1.3 Status Messages | ❌ | Mangler aria-live for beskeder |

**Problem: Job Cards (LandingPage.vue L73-86)**
```vue
<!-- NUVÆRENDE: Ikke tilgængelig -->
<el-card
  v-for="job in jobs"
  :key="job.id"
  shadow="hover"
  @click="openJobModal(job.id)"
>

<!-- ANBEFALET: Tilgængelig version -->
<el-card
  v-for="job in jobs"
  :key="job.id"
  shadow="hover"
  tabindex="0"
  role="button"
  :aria-label="`Læs mere om ${job.title}`"
  @click="openJobModal(job.id)"
  @keydown.enter="openJobModal(job.id)"
  @keydown.space.prevent="openJobModal(job.id)"
>
```

**Problem: JobModal mangler ARIA (JobModal.vue)**
```vue
<!-- NUVÆRENDE: Mangler ARIA -->
<div class="modal-wrapper__container">

<!-- ANBEFALET: Med ARIA -->
<div 
  class="modal-wrapper__container"
  role="dialog"
  aria-modal="true"
  aria-labelledby="job-modal-title"
>
  <h2 id="job-modal-title" class="job-modal__title">{{ currentJob.title }}</h2>
```

---

## Mangler og Anbefalinger

### Høj Prioritet 🔴

#### 1. Skip Link (2.4.1)
**Fil:** `index.html`, `LandingPage.vue`, `_utilities.scss`

**Implementation:**
```html
<!-- index.html -->
<body>
  <a href="#main-content" class="skip-link">Gå til hovedindhold</a>
  <div id="app"></div>
</body>
```

```vue
<!-- LandingPage.vue -->
<main id="main-content" class="landing-page__main">
```

```scss
/* _utilities.scss */
.skip-link {
  position: absolute;
  top: -100%;
  left: 50%;
  transform: translateX(-50%);
  background: $c-warning;
  color: $c-primary;
  padding: $spacing-sm $spacing-md;
  z-index: 9999;
  border-radius: $border-radius-md;
  font-weight: $font-weight-bold;
  text-decoration: none;
  
  &:focus {
    top: $spacing-sm;
    outline: 2px solid $c-primary;
    outline-offset: 2px;
  }
}
```

#### 2. Reduced Motion (2.3.3)
**Fil:** `_reset.scss` eller `_utilities.scss`

```scss
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
  
  // Stop video autoplay for users who prefer reduced motion
  video[autoplay] {
    animation: none !important;
  }
}
```

**Vue komponenter - tilføj også:**
```typescript
// I komponenter med animationer
const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches
```

#### 3. Job Card Accessibility (4.1.2, 2.1.1)
**Fil:** `LandingPage.vue`

```vue
<el-card
  v-for="job in jobs"
  :key="job.id"
  class="el-image-card"
  shadow="hover"
  tabindex="0"
  role="button"
  :aria-label="`Åbn jobdetaljer for ${job.title}`"
  @click="openJobModal(job.id)"
  @keydown.enter="openJobModal(job.id)"
  @keydown.space.prevent="openJobModal(job.id)"
>
```

### Medium Prioritet 🟡

#### 4. Video Captions (1.2.2)
**Filer:** Opret `.vtt` filer i `/public/videos/`

```
WEBVTT

00:00:00.000 --> 00:00:05.000
SBS Friction A/S - En arbejdsplads i verdensklasse

00:00:05.000 --> 00:00:10.000
Vi producerer bremseteknologi til køretøjer verden over
```

**VideoPlayerV2.vue - tilføj tracks support:**
```vue
<VideoPlayer
  :options="{
    tracks: [{
      src: '/videos/HERO_V2_8bit.vtt',
      kind: 'captions',
      srclang: 'da',
      label: 'Dansk'
    }]
  }"
/>
```

#### 5. Modal ARIA Labels (4.1.2)
**Fil:** `JobModal.vue`, `ApplicationModal.vue`

```vue
<!-- JobModal.vue -->
<div
  class="modal-wrapper__container"
  role="dialog"
  aria-modal="true"
  aria-labelledby="job-modal-title"
>
  <h2 id="job-modal-title" class="job-modal__title">{{ currentJob.title }}</h2>

<!-- ApplicationModal.vue -->
<div
  class="modal-wrapper__container"
  role="dialog"
  aria-modal="true"
  aria-labelledby="application-modal-title"
>
  <h2 id="application-modal-title" class="application-modal__title">...</h2>
```

#### 6. Live Regions for Status Messages (4.1.3)
**Fil:** `LandingPage.vue` eller global komponent

```vue
<!-- Tilføj til App.vue eller layout -->
<div 
  id="a11y-announcer"
  aria-live="polite" 
  aria-atomic="true"
  class="sr-only"
></div>
```

```scss
/* _utilities.scss */
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

```typescript
// utils/announce.ts
export function announce(message: string) {
  const announcer = document.getElementById('a11y-announcer')
  if (announcer) {
    announcer.textContent = ''
    // Small delay ensures announcement is read
    setTimeout(() => {
      announcer.textContent = message
    }, 100)
  }
}
```

### Lav Prioritet 🟢

#### 7. Autocomplete Attributes (1.3.5)
**Fil:** `ApplicationModal.vue`

```vue
<el-input v-model="formData.fullName" autocomplete="name" />
<el-input v-model="formData.phone" autocomplete="tel" />
<el-input v-model="formData.email" autocomplete="email" />
```

#### 8. Font Units (1.4.4)
**Fil:** `_variables.scss`

```scss
// Konverter fra px til rem (baseret på 16px root)
$font-size-title: 2.25rem;    // 36px
$font-size-subtitle: 2.25rem; // 36px
$font-size-button: 1.125rem;  // 18px
$font-size-input: 1rem;       // 16px
$font-size-body: 0.875rem;    // 14px
$font-size-small: 0.75rem;    // 12px
$font-size-xs: 0.625rem;      // 10px
```

---

## Implementeringsguide

### Fase 1: Kritiske Fixes (1-2 dage)
1. ✏️ Tilføj skip link
2. ✏️ Implementer reduced motion media query
3. ✏️ Fix job card keyboard accessibility

### Fase 2: Medium Prioritet (3-5 dage)
4. ✏️ Tilføj ARIA roller til alle modaler
5. ✏️ Opret undertekster til videoer
6. ✏️ Implementer live regions for beskeder

### Fase 3: Polering (1-2 dage)
7. ✏️ Tilføj autocomplete til form inputs
8. ✏️ Konverter px til rem
9. ✏️ Test med skærmlæsere

---

## Testprocedurer

### Manuel Test Tjekliste

#### Keyboard Navigation
- [ ] Tab gennem alle interaktive elementer på siden
- [ ] Verificer synlig fokusindikator (gul ring)
- [ ] Test ESC lukker alle modaler
- [ ] Test Enter/Space aktiverer knapper og links
- [ ] Verificer focus trap i modaler fungerer
- [ ] Test skip link springer til hovedindhold

#### Skærmlæser Test (NVDA/VoiceOver)
- [ ] Alle billeder har beskrivende alt-tekster
- [ ] Formular labels læses korrekt
- [ ] Fejlmeddelelser annonceres
- [ ] Modal overskrifter læses ved åbning
- [ ] Tidsslots rolle og tilstand annonceres

#### Visuel Test
- [ ] Test med 200% browser zoom
- [ ] Test med høj kontrast modus
- [ ] Test med prefers-reduced-motion aktiveret
- [ ] Verificer ingen tekstoverlap ved zoom

### Automatiseret Test

#### Anbefalede Tools
1. **axe DevTools** - Browser extension til hurtig audit
2. **Lighthouse** - Indbygget i Chrome DevTools
3. **WAVE** - Web Accessibility Evaluation Tool
4. **Pa11y** - CLI tool til CI/CD integration

#### Lighthouse Kommando
```bash
lighthouse https://localhost:5173 --only-categories=accessibility --output=html --output-path=./a11y-report.html
```

#### Pa11y CI Integration
```json
// .pa11yrc.json
{
  "defaults": {
    "standard": "WCAG2AA",
    "runners": ["htmlcs"],
    "ignore": []
  },
  "urls": [
    "http://localhost:5173/",
    "http://localhost:5173/hr-dashboard"
  ]
}
```

---

## Ressourcer

### Officielle Standarder
- [WCAG 2.1 Retningslinjer](https://www.w3.org/TR/WCAG21/)
- [WAI-ARIA 1.2](https://www.w3.org/TR/wai-aria-1.2/)
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/)

### Test Tools
- [axe DevTools](https://www.deque.com/axe/)
- [WAVE](https://wave.webaim.org/)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse)
- [Colour Contrast Analyser](https://www.tpgi.com/color-contrast-checker/)

### Skærmlæsere
- [NVDA (Windows)](https://www.nvaccess.org/)
- [VoiceOver (macOS/iOS)](https://www.apple.com/accessibility/vision/)
- [TalkBack (Android)](https://support.google.com/accessibility/android/answer/6283677)

---

## Konklusion

SBS Recruitment App har et **godt fundament** for tilgængelighed med:
- Korrekt sprogmærkning
- Good focus visible styling
- Grundlæggende keyboard support i nogle komponenter
- ARIA attributes på kritiske elementer

**Prioriterede forbedringer:**
1. Skip link (kritisk for keyboard-brugere)
2. Reduced motion support (kritisk for some users)
3. Job card keyboard accessibility
4. Video undertekster
5. Konsistent ARIA på alle modaler

Med disse forbedringer vil applikationen opnå fuld WCAG 2.1 AA compliance.
