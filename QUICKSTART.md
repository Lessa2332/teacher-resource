# ⚡ Швидкий старт — «Вчитель в ресурсі»

## За 2 хвилини до першого використання

### Для педагогів 👨‍🏫

**1. Відкрийте на смартфоні:**
```
https://YOUR_USERNAME.github.io/teacher-resource/
```

**2. Натисніть "Встановити" (якщо запропонує)**
- iOS: Share → Add to Home Screen → Add
- Android: Menu → Install app

**3. Почніть користуватися!**
- Змініть метрики (перетягуйте слайдери)
- Дивіться, як змінюється Квітка-ресурс 🌸
- Натисніть на дію для вправ

### Що означають кольори?

| Колір | Стан | Рекомендація |
|-------|------|--------------|
| 🟢 Зелена | Енергійний | Працюйте на повну, плануйте важливе |
| 🟡 Жовта | Втомлений | Пауза на 5 хвилин: чай, прогулянка |
| 🟠 Помаранчева | На межі | Дихальна вправа, зв'яжіться з психологом |
| 🔴 Червона | Критичний | Гаряча лінія: 0 800 500 450 |

---

## Для розробників 👨‍💻

### Встановлення локально

```bash
# 1. Клонуйте
git clone https://github.com/YOUR_USERNAME/teacher-resource.git
cd teacher-resource

# 2. Запустіть на локальному сервері
python3 -m http.server 8000

# 3. Відкрийте браузер
open http://localhost:8000
```

### Файлова структура поточної версії

```
index.html              (основна версія, 15 KB)
├─ HTML структура
├─ CSS стилі (вбудовані)
└─ JavaScript (vanilla)

advanced.html           (розширена версія, 18 KB)
├─ Tab-based навігація
├─ Прогрес сторінка
└─ Налаштування
```

### Як модифікувати?

#### 1. Змінити Квітку-ресурс (flower.svg)

```html
<!-- Поточний SVG (180px) -->
<svg id="flower" width="240" height="240" viewBox="0 0 240 240">
  <circle id="petal-top" cx="120" cy="50" r="28" fill="#10b981" class="petal"/>
  <!-- 6 пелюсток + центр -->
</svg>

<!-- Для розширення: додайте більше кругів або змініть форму -->
<path id="custom-petal" d="..." fill="#10b981" class="petal"/>
```

#### 2. Додати нову метрику

```javascript
// У секції state management
let metrics = {
  anxiety: 25,
  exhaustion: 30,
  irritability: 20,
  stress: 28,
  // ДОДАЙТЕ:
  fatigue: 35  // нова метрика
};

// Оновіть updateUI()
document.getElementById('m-fatigue').textContent = Math.round(metrics.fatigue);
```

#### 3. Нова вправа

```javascript
const exercises = {
  'new-exercise': {
    name: '🧘 Назва вправи',
    duration: 300, // 5 хвилин
    description: 'Опис для користувача',
    steps: [
      'Крок 1: ...',
      'Крок 2: ...'
    ]
  }
};

// Додайте кнопку в UI
<div onclick="startExercise('new-exercise')">🧘 Назва вправи</div>
```

#### 4. Змінити кольори (за станами)

```javascript
// Поточні кольори
const colorMap = {
  green: '#10b981',    // Зелена
  yellow: '#fbbf24',   // Жовта
  orange: '#f97316',   // Помаранчева
  red: '#ef4444'       // Червона
};

// Змініть на свої (hex або RGB)
const colorMap = {
  green: '#059669',    // Темніша зелень
  yellow: '#fcd34d',   // Яскравіша жовть
  // ...
};
```

### Додавання даних на цей час (future: IndexedDB)

```javascript
// Майбутня структура для зберігання
async function saveMetrics() {
  const db = await openDatabase('TeacherResource');
  const timestamp = new Date().toISOString();
  
  await db.store('metrics').add({
    timestamp,
    anxiety: metrics.anxiety,
    exhaustion: metrics.exhaustion,
    state: calculateState()
  });
}

async function getHistory(days = 7) {
  const db = await openDatabase('TeacherResource');
  return await db.store('metrics').getAll();
}
```

---

## Тестування

### Симулювати різні стани

```javascript
// У браузері Console (F12)

// Зелена зона
metrics.anxiety = 20;
metrics.exhaustion = 25;
updateUI();

// Червона зона
metrics.anxiety = 95;
metrics.exhaustion = 90;
updateUI();
```

### Перевірити offline функцію

```javascript
// Chrome DevTools → Application → Service Workers
// Оберіть "Offline" → Перевірте, як працює додаток
```

---

## Розширення функціоналу

### Інтеграція з Claude API (Python backend)

```python
# future: backend/generate_action.py
from anthropic import Anthropic

def generate_action(anxiety, exhaustion, irritability):
    client = Anthropic()
    
    message = client.messages.create(
        model="claude-sonnet-4-6",
        max_tokens=1024,
        messages=[
            {
                "role": "user",
                "content": f"""
                Педагог у стресі:
                - Тривога: {anxiety}/100
                - Виснаження: {exhaustion}/100
                - Дратівливість: {irritability}/100
                
                Дайте одну конкретну порцію на українській.
                Максимум 1-2 речення.
                """
            }
        ]
    )
    
    return message.content[0].text
```

### Інтеграція з Web Speech API (голос)

```javascript
//識認основні команди голосом
const recognition = new webkitSpeechRecognition();
recognition.lang = 'uk-UA';

recognition.onresult = (event) => {
  const command = event.results[0][0].transcript;
  
  if (command.includes('дихання')) {
    startBreathingExercise();
  } else if (command.includes('помощь')) {
    callHotline();
  }
};

recognition.start();
```

---

## Поширені проблеми

### Проблема: Квітка не зміниться колір

**Рішення:**
```javascript
// Переконайтеся, що updateFlower() викликається після updateUI()
function updateUI() {
  // ...
  updateFlower(); // ← важливо!
}
```

### Проблема: Мобільна версія виглядає дивно

**Рішення:**
```html
<!-- Перевірте viewport мета-тег -->
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">

<!-- Тестуйте на реальному смартфоні, не в DevTools-->
<!-- DevTools не показує "safe areas" на iPhone з вирізом -->
```

### Проблема: Offline не працює

**Рішення:**
```javascript
// Переконайтеся, що Service Worker зареєстрований
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('sw.js');
}

// Перевірте Chrome DevTools → Application → Service Workers
```

---

## Гайдлайни для контрибюторів

### Перед публікацією PR

- [ ] Код працює на мобілі (iPhone 12, Samsung Galaxy)
- [ ] Темна тема підтримується (dark mode)
- [ ] Немає JavaScript помилок у консолі
- [ ] Текст 100% українською
- [ ] Розмір файлу < 50 KB (разом)
- [ ] Тести пройдені (якщо є)

### Посилання на ресурси

- **Українські психологічні вправи**: https://psychofy.ua/
- **GDPR для EdTech**: https://gdpr-info.eu/
- **Web Accessibility (WCAG 2.1)**: https://www.w3.org/WAI/

---

## Додаткові матеріали

### Для педагогів

📚 **Книга:** "Емоційна стійкість педагога" (Л.М. Мітіна)  
📖 **Стаття:** "Синдром вигорання у вчителів" (Журнал психології)  
🎬 **Відео:** [гайд по дихальним вправам] (YouTube)

### Для розробників

💻 **Web Components Best Practices**: https://webcomponents.dev/  
🛠️ **Progressive Web Apps**: https://web.dev/progressive-web-apps/  
📱 **React Native Alternative**: https://expo.dev/  

---

## План розвитку

```
2027-Q2: v1.0 (поточна)
├─ Квітка-ресурс ✓
├─ Метрики ✓
└─ Базові вправи ✓

2027-Q3: v1.1
├─ IndexedDB історія
├─ Web Speech (голос)
├─ Google Classroom API
└─ Push-сповіщення

2027-Q4: v2.0
├─ MediaPipe Face Detection
├─ Claude AI рекомендації
├─ Синхронізація психологом
└─ Множинні мови

2028: v3.0
├─ AR окуляри
├─ Розпізнавання емоцій у класі
└─ Батьківський додаток
```

---

## Контакти та підтримка

- 🐛 **Баги** → GitHub Issues
- 💬 **Питання** → Discussions
- 🤝 **PR** → Будь ласка, відкривайте!
- 📧 **Контакт** → [email]

---

## Ліцензія

**Ukrainian Teacher Open Source License (UТOSL)**

Використовуйте вільно для освітніх цілей. Детальніше: див. `LICENSE.md`

---

**Успіхів у розробці! 🚀**

Пам'ятайте: цей додаток створений для вчителів, які продовжують працювати у складні часи. Кожна розробка має значення! ❤️
