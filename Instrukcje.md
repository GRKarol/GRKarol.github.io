# 📋 INSTRUKCJE ZARZĄDZANIA STRONĄ PROGRESSIO

## 🖼️ DODAWANIE ZDJĘĆ DO GALERII

### Krok 1: Dodanie nowego zdjęcia
W sekcji `gallery-track` dodaj nowy slajd:

```html
<div class="gallery-slide">
  <h3 class="image-title">Nazwa zdjęcia (opcjonalna)</h3>
  <img src="nazwa-pliku.jpg" alt="Opis alternatywny" loading="lazy" onclick="toggleDescription(X)">
  <p class="image-description" id="desc-X">Opis zdjęcia (opcjonalny)</p>
</div>
```
**Zastąp X numerem kolejnego slajdu (0, 1, 2, itd.)**

### Krok 2: Dodanie kropki nawigacyjnej
W sekcji `gallery-dots` dodaj:
```html
<button class="gallery-dot" onclick="goToSlide(X)" aria-label="Przejdź do zdjęcia X"></button>
```

### Krok 3: Aktualizacja JavaScript
Znajdź w kodzie linię:
```javascript
const totalSlides = 1; // ZMIEŃ TĘ WARTOŚĆ gdy dodasz więcej zdjęć
```
Zmień `1` na liczbę wszystkich slajdów.

### Warianty bez nazwy/opisu:

#### Bez nazwy i opisu:
```html
<div class="gallery-slide">
  <img src="nazwa-pliku.jpg" alt="Opis alternatywny" loading="lazy">
</div>
```

#### Tylko z nazwą (bez opisu):
```html
<div class="gallery-slide">
  <h3 class="image-title">Nazwa zdjęcia</h3>
  <img src="nazwa-pliku.jpg" alt="Opis alternatywny" loading="lazy">
</div>
```

---

## 💿 AKTYWACJA DYSKU GRANKERS TEAM

### Krok 1: Zmiana przycisku na aktywny

**Przed aktywacją:**
```html
<button class="disc-button" disabled>
  <svg>...</svg>
  Odwiedź Nasz Dysk / Visit Our Disc
</button>
```

**Po aktywacji:**
```html
<a href="LINK_DO_DYSKU" target="_blank" class="nav-button">
  <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="currentColor" style="margin-right: 8px;">
    <path d="M14,3V5H17.59L7.76,14.83L9.17,16.24L19,6.41V10H21V3M19,19H5V5H12V3H5C3.89,3 3,3.9 3,5V19A2,2 0 0,0 5,21H19A2,2 0 0,0 21,19V12H19V19Z"/>
  </svg>
  Odwiedź Nasz Dysk / Visit Our Disc
</a>
```

### Krok 2: Ukrycie sekcji powiadomień
Dodaj CSS:
```css
.notification-setup { display: none; }
```
Lub usuń całą sekcję `notification-setup`.

### Krok 3: Aktualizacja stylów
W CSS znajdź `.disc-button` i zmień na:
```css
.disc-button-active {
  border: 1px solid var(--accent-color);
  color: var(--accent-color);
  cursor: pointer;
  opacity: 1;
}
.disc-button-active:hover {
  background-color: var(--accent-color);
  color: var(--bg-color);
}
```

---

## 🔔 WYSYŁANIE POWIADOMIEŃ O GOTOWOŚCI DYSKU

### OPCJA 1 - URL Parameter (Najłatwiejsza)
Dodaj na końcu obu plików HTML (przed `</body>`):
```html
<script>
if (window.location.search.includes('discReady=true')) {
    setTimeout(() => sendDiscReadyNotification(), 1000);
}
</script>
```

**Sposób użycia:**
- Polska: `yoursite.com/?discReady=true`
- Angielska: `yoursite.com/en.html?discReady=true`

### OPCJA 2 - Admin Button (Ukryty przycisk)
Dodaj gdzieś na stronie:
```html
<button onclick="sendDiscReadyNotification()" 
        style="position:fixed;top:0;right:0;z-index:9999;background:red;color:white;padding:10px;">
    ADMIN: Wyślij Powiadomienia
</button>
```

### OPCJA 3 - Console (Manualne)
Poproś użytkowników o wpisanie w konsoli przeglądarki:
```javascript
sendDiscReadyNotification()
```

### OPCJA 4 - Service Worker (Profesjonalne)
1. Stwórz plik `sw.js` w głównym folderze:
```javascript
self.addEventListener('message', event => {
  if (event.data && event.data.type === 'SEND_DISC_NOTIFICATION') {
    self.registration.showNotification('🎉 Dysk Grankers Team\'u jest już dostępny!', {
      body: 'Wszystkie materiały edukacyjne czekają na Ciebie.',
      icon: '/logo.png',
      badge: '/logo.png',
      tag: 'disc-ready',
      requireInteraction: true,
      actions: [
        { action: 'open', title: 'Otwórz Dysk' },
        { action: 'later', title: 'Później' }
      ]
    });
  }
});
```

2. Zarejestruj Service Worker w HTML:
```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js');
}
```

---

## 🌐 ZARZĄDZANIE JĘZYKAMI

### Powiadomienia automatycznie wybierają język:
- **Polska strona** zapisuje: `localStorage.setItem('discNotificationLanguage', 'pl')`
- **Angielska strona** zapisuje: `localStorage.setItem('discNotificationLanguage', 'en')`

### Funkcja wysyłająca zawiera oba języki:
```javascript
const messages = {
    pl: {
        title: '🎉 Dysk Grankers Team\'u jest już dostępny!',
        body: 'Wszystkie materiały edukacyjne czekają na Ciebie.',
        open: 'Otwórz Dysk',
        later: 'Później'
    },
    en: {
        title: '🎉 Grankers Team Disc is now available!',
        body: 'All educational materials are waiting for you.',
        open: 'Open Disc',
        later: 'Later'
    }
};
```

---

## 📱 PROBLEMY MOBILNE - ROZWIĄZANIA

### Problem: Animacje kroków wychodzą poza ekran
**Rozwiązanie:** Na mobile animacje wyłączone (wszystkie kroki pojawiają się ze środka):
```css
@media (max-width: 768px) {
    .story-step.reveal:nth-of-type(1) { transform: translateX(0px) translateY(30px); }
    .story-step.reveal:nth-of-type(2) { transform: translateX(0px) translateY(30px); }
    .story-step.reveal:nth-of-type(3) { transform: translateX(0px) translateY(30px); }
}
```

### Problem: Poziome przewijanie
**Rozwiązanie:** Dodane:
```css
body { overflow-x: hidden; }
.container { box-sizing: border-box; }
```

### Problem: Elementy wystają poza ekran
**Rozwiązanie:** Wszystkie kontenery mają `box-sizing: border-box`

---

## 🎨 DOSTOSOWYWANIE WYGLĄDU

### Zmiana kolorów:
```css
:root {
    --bg-color: #0a0a0a;          /* Tło główne */
    --text-color: #e0e0e0;        /* Tekst główny */
    --accent-color: #D4AF37;      /* Złoty akcent */
    --dark-gray: #111111;         /* Ciemne sekcje */
    --light-gray: #a0a0a0;        /* Tekst drugoplanowy */
    --disabled-color: #666666;    /* Elementy wyłączone */
}
```

### Zmiana animacji kroków:
```css
/* Desktop - kroki 1,3 z lewej, krok 2 z prawej */
.story-step.reveal:nth-of-type(1) { transform: translateX(-80px) translateY(50px); }
.story-step.reveal:nth-of-type(2) { transform: translateX(80px) translateY(50px); }
.story-step.reveal:nth-of-type(3) { transform: translateX(-80px) translateY(50px); }
```

---

## 🔧 TESTOWANIE FUNKCJI

### Test galerii:
1. Dodaj więcej zdjęć według instrukcji
2. Zaktualizuj `totalSlides`
3. Sprawdź działanie strzałek i kropek

### Test powiadomień:
1. Kliknij dzwonek na stronie
2. Zaakceptuj powiadomienia w przeglądarce
3. Użyj jednej z opcji wysyłania powiadomień
4. Sprawdź czy powiadomienie przychodzi w odpowiednim języku

### Test responsywności:
1. Otwórz narzędzia deweloperskie (F12)
2. Przełącz na widok mobile
3. Sprawdź czy animacje działają poprawnie
4. Sprawdź czy nie ma poziomego przewijania

---

## 📂 STRUKTURA PLIKÓW

```
projekt/
├── index.html          # Wersja polska
├── en.html            # Wersja angielska
├── logo.png           # Logo strony
├── globe.png          # Ikona przełącznika języka
├── journal-1.jpg      # Zdjęcie dziennika (dodaj więcej)
├── favicon.ico        # Ikona przeglądarki
└── sw.js             # Service Worker (opcjonalny)
```

---

## ⚠️ WAŻNE UWAGI

1. **Zawsze testuj na mobile i desktop**
2. **Każda zmiana w galerii wymaga aktualizacji totalSlides**
3. **Powiadomienia działają tylko na HTTPS (GitHub Pages to zapewnia)**
4. **Backup kodu przed większymi zmianami**
5. **Ikona external link** - `↗️` dla przycisków prowadzących poza stronę

---

## 🎯 SZYBKI CHECKLIST PRZED PUBLIKACJĄ

- [ ] Galeria ma prawidłową liczbę slajdów w `totalSlides`
- [ ] Wszystkie obrazy mają odpowiednie `alt` texty
- [ ] Sekcja dysku jest we właściwym miejscu (PL: przed formularzem, EN: po formularzu)
- [ ] Powiadomienia działają w obu językach
- [ ] Strona nie ma poziomego przewijania na mobile
- [ ] Animacje kroków działają poprawnie na wszystkich urządzeniach
- [ ] Wszystkie linki zewnętrzne mają `target="_blank"`

---

*Instrukcje przygotowane dla Progressio by Grankers Team*
*"Wiedza bez działania to tylko informacja. Działanie bez wiedzy to chaos."*
