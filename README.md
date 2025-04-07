<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ShowerStreak</title>
  <link rel="stylesheet" href="styles.css">
  <link rel="manifest" href="manifest.json">
</head>
<body>

  <h1>ShowerStreak</h1>
  <div id="status">🤔 Let's find out...</div>
  <div id="quote"></div>
  <button id="yesBtn" onclick="markShowered()">Yes, I Showered</button>
  <button id="noBtn" onclick="confirmNoShower()">Nope, Not Yet</button>
  <div id="streak"></div>
  <button id="themeToggle" onclick="toggleTheme()">Toggle Dark Mode</button>

  <script src="script.js"></script>
</body>
</html>
:root {
  --bg: linear-gradient(45deg, #ff6b6b, #ffb347);
  --text: #333;
  --btn-bg: #4CAF50;
  --btn-hover: #45a049;
  --btn-no-bg: #e53935;
  --btn-no-hover: #d32f2f;
}

[data-theme="dark"] {
  --bg: linear-gradient(45deg, #6200ea, #03a9f4);
  --text: #f4f4f4;
  --btn-bg: #3a7f3a;
  --btn-hover: #2e6d2e;
  --btn-no-bg: #e57373;
  --btn-no-hover: #ef5350;
}

body {
  font-family: 'Roboto', sans-serif;
  background: var(--bg);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  text-align: center;
  color: var(--text);
  transition: background 0.3s, color 0.3s;
  margin: 0;
}

h1 {
  font-size: 3rem;
  margin-bottom: 1rem;
  letter-spacing: 1px;
}

#status {
  font-size: 1.5rem;
  margin: 1rem 0;
  font-weight: 500;
}

#quote {
  margin-top: 1rem;
  font-size: 1.1rem;
  font-style: italic;
  max-width: 80%;
  color: var(--text);
}

button {
  color: white;
  padding: 1rem 2rem;
  border: none;
  border-radius: 12px;
  font-size: 1.2rem;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
  margin: 0.5rem;
  min-width: 200px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

#yesBtn {
  background: var(--btn-bg);
}

#yesBtn:hover {
  background: var(--btn-hover);
}

#noBtn {
  background: var(--btn-no-bg);
}

#noBtn:hover {
  background: var(--btn-no-hover);
}

#streak {
  margin-top: 1rem;
  font-style: italic;
}

#themeToggle {
  margin-top: 20px;
  cursor: pointer;
  font-size: 0.9rem;
  background: transparent;
  border: 1px solid var(--text);
  padding: 0.5rem 1rem;
  border-radius: 8px;
  transition: border-color 0.3s ease-in-out;
}

#themeToggle:hover {
  border-color: var(--btn-bg);
}

/* Mobile responsiveness */
@media (max-width: 600px) {
  h1 {
    font-size: 2rem;
  }

  button {
    font-size: 1rem;
    min-width: 150px;
  }

  #status {
    font-size: 1.2rem;
  }
}
const statusEl = document.getElementById('status');
const streakEl = document.getElementById('streak');
const quoteEl = document.getElementById('quote');

const showerQuotes = [
  "Fresh as a daisy and twice as confident. Well done!",
  "You smell like success. And soap. Nice!",
  "The shower gods are pleased. Go conquer your day!",
  "Clean body, clean mind. You’re unstoppable!",
  "You did it! Adulting level: unlocked 🛁",
  "Squeaky clean and ready for the scene!",
  "Shower complete. You may now be hugged.",
  "You didn’t just shower… you thrived.",
  "Soap: 1 | Bacteria: 0 🧼",
  "You're now legally allowed to sit on your own couch."
];

const noShowerQuotes = [
  "Your ancestors fought wars. You can face the water.",
  "You're not dirty dirty… but you ain't clean.",
  "Is that your thoughts… or body odor clouding your brain?",
  "Don't make us send Febreze to your house.",
  "If motivation had a smell, it wouldn't be this.",
  "Your pillow just texted me. It's begging for mercy.",
  "One shower away from being the main character.",
  "A stinky legend is still a legend. But c’mon.",
  "Still haven’t showered? Bold strategy, Cotton.",
  "This is your sign. Lather up, champ."
];

function getRandom(arr) {
  return arr[Math.floor(Math.random() * arr.length)];
}

function markShowered() {
  const audio = new Audio('https://www.soundjay.com/buttons/sounds/button-3.mp3');
  audio.play();

  const today = new Date().toDateString();
  localStorage.setItem('showerDate', today);

  let streak = Number(localStorage.getItem('streak') || '0');
  const lastDate = localStorage.getItem('lastDate');
  if (lastDate && new Date(lastDate).getDate() === new Date(today).getDate() - 1) {
    streak += 1;
  } else if (lastDate !== today) {
    streak = 1;
  }

  localStorage.setItem('lastDate', today);
  localStorage.setItem('streak', streak);
  updateStatus();
}

function confirmNoShower() {
  if (confirm("Are you sure you didn't shower?")) {
    const audio = new Audio('https://www.soundjay.com/buttons/sounds/button-9.mp3');
    audio.play();

    const today = new Date().toDateString();
    localStorage.setItem('showerDate', '');
    localStorage.setItem('lastDate', today);
    localStorage.setItem('streak', '0');
    updateStatus();
  }
}

function updateStatus() {
  const today = new Date().toDateString();
  const showerDate = localStorage.getItem('showerDate');

  if (showerDate === today) {
    statusEl.textContent = '🧼 Yep! You showered today!';
    quoteEl.textContent = getRandom(showerQuotes);
  } else {
    statusEl.textContent = "😬 Doesn't look like it...";
    quoteEl.textContent = getRandom(noShowerQuotes);
  }

  const streak = localStorage.getItem('streak') || '0';
  streakEl.textContent = `Shower streak: ${streak} day(s)`;
}

function toggleTheme() {
  const currentTheme = document.documentElement.getAttribute('data-theme');
  const newTheme = currentTheme === 'dark' ? 'light' : 'dark';
  document.documentElement.setAttribute('data-theme', newTheme);
  localStorage.setItem('theme', newTheme);
}

function loadTheme() {
  const savedTheme = localStorage.getItem('theme') || 'light';
  document.documentElement.setAttribute('data-theme', savedTheme);
}

updateStatus();
loadTheme();

if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('sw.js').then(
      reg => console.log('Service worker registered.', reg),
      err => console.error('Service worker registration failed:', err)
    );
  });
}
const cacheName = 'shower-tracker-v1';
const assetsToCache = [
  '/',
  '/index.html',
  '/styles.css',
  '/script.js',
  '/manifest.json',
  '/images/icon-192x192.png',
  '/images/icon-512x512.png',
];

self.addEventListener('install', (e) => {
  e.waitUntil(
    caches.open(cacheName).then((cache) => {
      return cache.addAll(assetsToCache);
    })
  );
});

self.addEventListener('activate', (e) => {
  const cacheWhitelist = [cacheName];
  e.waitUntil(
    caches.keys().then((cacheNames) => {
      return Promise.all(
        cacheNames.map((cache) => {
          if (!cacheWhitelist.includes(cache)) {
            return caches.delete(cache);
          }
        })
      );
    })
  );
});

self.addEventListener('fetch', (e) => {
  e.respondWith(
    caches.match(e.request).then((cachedResponse) => {
      return cachedResponse || fetch(e.request);
    })
  );
});
{
  "name": "ShowerStreak",
  "short_name": "ShowerStreak",
  "description": "Track your daily shower habits and streaks with fun reminders!",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#ff6b6b",
  "theme_color": "#ff6b6b",
  "icons": [
    {
      "src": "images/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "images/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
