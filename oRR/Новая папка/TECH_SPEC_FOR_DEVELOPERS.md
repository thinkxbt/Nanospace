# ТЕХНИЧЕСКАЯ СПЕЦИФИКАЦИЯ ДЛЯ ПРОГРАММИСТОВ

## Проект: KELECHEK Premium Website
## Дата: 11 ноября 2025

---

## 🎯 ЦЕЛЬ

Создать **ПРЕМИУМ сайт мирового уровня** с:
- Сложными CSS/JS анимациями
- 3D эффектами
- Параллакс
- Интерактивностью
- Плавными transitions

**Стандарт качества:** как у 5scontent.com/about/, abdysh-ata.kg

---

## 📁 ФАЙЛЫ ПРОЕКТА

### Основной эскиз:
**`kelechek_premium_mockup.html`** - интерактивный HTML с полным функционалом

### Референсы:
1. https://5scontent.com/about/ - уровень анимаций и интерактивности
2. https://abdysh-ata.kg - карта мира, премиум дизайн
3. https://trymeridian.com - минимализм и чистота
4. https://coca-cola.com/kg/ru - мировой стандарт

---

## 🎨 РЕАЛИЗОВАННЫЕ АНИМАЦИИ

### 1. HERO СЕКЦИЯ (Главный экран)

#### Параллакс при движении мыши:
```javascript
// Фон двигается за курсором
window.addEventListener('mousemove', (e) => {
    const x = (e.clientX / window.innerWidth - 0.5) * 20;
    const y = (e.clientY / window.innerHeight - 0.5) * 20;
    heroBg.style.transform = `translate(${x}px, ${y}px)`;
});
```

**Эффект:** Горы Кыргызстана двигаются за курсором, создавая глубину

#### Fade In анимация:
- H1 появляется снизу вверх (800ms delay)
- Tagline появляется (delay 300ms)
- CTA кнопка появляется (delay 500ms)

#### Scroll Indicator:
- Анимированная стрелка вниз (bounce эффект)
- Исчезает при скролле
- Клик = плавный скролл к следующей секции

---

### 2. ПРОДУКЦИЯ - 3D БУТЫЛКИ

#### Структура бутылки:
```html
<div class="bottle-container"> <!-- Perspective контейнер -->
    <div class="bottle-3d">     <!-- 3D трансформация -->
        <div class="bottle-cap">  <!-- Крышка -->
        <div class="bottle">      <!-- Тело бутылки -->
            <div class="water-level"> <!-- Вода внутри -->
                <div class="bubble">  <!-- Пузырьки -->
```

#### 3D эффект при hover:
```css
.product-card:hover .bottle-3d {
    transform: rotateY(15deg) rotateX(5deg) translateZ(30px);
}
```

**Результат:** Бутылка вращается в 3D при наведении

#### Цвета бутылок:
1. **Минеральная вода** - голубой градиент, красная крышка
2. **Гималаи** - прозрачный, голубая крышка
3. **Лимонад** - желтый градиент, желтая крышка

#### Анимация воды:
```css
@keyframes waterWave {
    0%, 100% { transform: translateY(0); }
    50% { transform: translateY(-5px); }
}
```

**Результат:** Вода внутри бутылки плавно колышется

#### Пузырьки:
- 3 пузырька в каждой бутылке
- Поднимаются снизу вверх
- Разная скорость и размер
- Бесконечный цикл

```css
@keyframes bubbleRise {
    0% {
        transform: translateY(0) scale(1);
        opacity: 0;
    }
    10% { opacity: 0.8; }
    90% { opacity: 0.8; }
    100% {
        transform: translateY(-200px) scale(0.5);
        opacity: 0;
    }
}
```

---

### 3. SCROLL ANIMATIONS (Intersection Observer)

#### Типы анимаций:

**Fade In:**
- Элемент появляется снизу с opacity 0→1
- Transform: translateY(50px) → 0

**Slide Left:**
- Элемент въезжает слева
- Transform: translateX(-100px) → 0

**Slide Right:**
- Элемент въезжает справа
- Transform: translateX(100px) → 0

**Scale In:**
- Элемент увеличивается
- Transform: scale(0.8) → 1

#### Реализация:
```javascript
const observerOptions = {
    threshold: 0.2,
    rootMargin: '0px 0px -100px 0px'
};

const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('visible');
        }
    });
}, observerOptions);
```

**Применяется к:**
- Текст "О воде"
- Изображение гор
- Карточки продукции
- Карта мира
- Статистика
- Карточки партнёров
- Иконки качества

---

### 4. КАРТА МИРА (SVG Animation)

#### Структура:
- SVG с упрощённой картой мира
- Кыргызстан - ACTIVE класс (красный, пульсирует)
- Страны СНГ - обычные (белые полупрозрачные)
- Маркеры - золотые точки (пульсируют)
- Линии связи - анимированные пунктиры

#### Анимация Кыргызстана:
```css
.country.active {
    fill: #D32F2F;
    animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.7; }
}
```

#### Анимация маркеров:
```css
@keyframes markerPulse {
    0%, 100% { 
        transform: scale(1);
        filter: drop-shadow(0 0 5px #FFD700);
    }
    50% { 
        transform: scale(1.2);
        filter: drop-shadow(0 0 15px #FFD700);
    }
}
```

#### Анимация линий:
```svg
<path stroke-dasharray="5,5">
    <animate attributeName="stroke-dashoffset" 
             from="100" to="0" 
             dur="3s" 
             repeatCount="indefinite"/>
</path>
```

**Эффект:** Пунктирные линии "бегут" от Кыргызстана к другим странам

---

### 5. СЧЁТЧИКИ (Animated Counters)

#### Функционал:
- Счётчик запускается когда секция видна
- Цифры считаются от 0 до финального значения
- Длительность: 2 секунды
- Плавное ускорение

```javascript
function animateCounter(element, target, duration = 2000) {
    const start = 0;
    const increment = target / (duration / 16);
    let current = start;
    
    const timer = setInterval(() => {
        current += increment;
        if (current >= target) {
            element.textContent = target + '+';
            clearInterval(timer);
        } else {
            element.textContent = Math.floor(current) + '+';
        }
    }, 16);
}
```

**Применяется к:**
- "7+ стран СНГ"
- "34 года качества"
- "100% натуральность"

---

### 6. HOVER ЭФФЕКТЫ

#### Карточки продукции:
```css
.product-card:hover {
    transform: translateY(-20px) scale(1.03);
    box-shadow: 0 30px 80px rgba(0,0,0,0.12);
}
```
- Поднимается вверх на 20px
- Увеличивается на 3%
- Тень становится глубже

#### Карточки партнёров:
```css
.partner-card:hover {
    transform: translateY(-10px) scale(1.02);
}

.partner-card:hover .partner-icon {
    transform: scale(1.2) rotate(5deg);
}
```
- Карточка поднимается
- Иконка увеличивается и поворачивается

#### Кнопки:
```css
.cta-button:hover {
    transform: translateY(-5px) scale(1.05);
    box-shadow: 0 15px 50px rgba(211, 47, 47, 0.5);
}
```
- Блестящий эффект (::before с градиентом)
- Поднимается вверх
- Тень усиливается

---

### 7. HEADER ЭФФЕКТЫ

#### При скролле:
```javascript
window.addEventListener('scroll', () => {
    if (window.pageYOffset > 100) {
        header.classList.add('scrolled');
    }
});
```

**Изменения при scrolled:**
- Тень становится глубже
- Фон более непрозрачный
- Backdrop blur усиливается

#### Меню:
- Подчёркивание при hover (анимированная линия)
- Smooth scroll при клике на anchor links

---

### 8. WHATSAPP КНОПКА

#### Анимация:
```css
@keyframes whatsappPulse {
    0%, 100% {
        transform: scale(1);
        box-shadow: 0 8px 30px rgba(37, 211, 102, 0.4);
    }
    50% {
        transform: scale(1.05);
        box-shadow: 0 10px 40px rgba(37, 211, 102, 0.6);
    }
}
```

#### Hover:
```css
.whatsapp-btn:hover {
    transform: scale(1.15) rotate(10deg);
}
```

**Эффект:** Постоянное пульсирование + вращение при hover

---

### 9. ИЗОБРАЖЕНИЯ И МЕДИА

#### Изображение гор:
```css
.about-image:hover::before {
    left: 100%;
}
```
- Блестящий эффект проходит через изображение
- Масштабирование при hover (102%)
- Углубление тени

---

### 10. ГРАДИЕНТЫ И ЦВЕТА

#### Используемая палитра:

**Основные:**
- Красный: `#D32F2F` (бренд)
- Тёмно-красный: `#B71C1C` (hover)
- Голубой: `#42A5F5` (вода, небо)
- Синий: `#1976D2` (акценты)

**Нейтральные:**
- Белый: `#FFFFFF`
- Серый светлый: `#FAFAFA`
- Серый: `#999`, `#666`, `#333`
- Чёрный: `#1a1a1a`

**Дополнительные:**
- Золотой: `#FFD700` (маркеры)
- Зелёный: `#25D366` (WhatsApp)
- Жёлтый: `#FDD835` (лимонад)

#### Градиенты:

**Hero фон:**
```css
linear-gradient(135deg, #1e3c72 0%, #2a5298 50%, #7e8ba3 100%)
```

**Карта мира:**
```css
linear-gradient(135deg, #0D47A1 0%, #1976D2 50%, #42A5F5 100%)
```

**CTA кнопка:**
```css
background: #D32F2F;
hover: #B71C1C;
```

---

## 💻 ТЕХНИЧЕСКИЕ ДЕТАЛИ

### HTML:
- Семантическая вёрстка
- Правильная структура заголовков
- Accessibility (aria-labels)
- SEO-friendly

### CSS:
- Custom properties (переменные)
- Flexbox + Grid
- Transitions для плавности
- Transform для производительности
- Animations с keyframes
- Cubic-bezier для естественности

### JavaScript:
- Intersection Observer API (скролл анимации)
- Event Listeners (параллакс, hover)
- Smooth scroll
- Animated counters
- Минимум библиотек (чистый JS)

### Производительность:
- Transform вместо left/top
- Will-change для анимаций
- Passive event listeners
- Оптимизированные SVG
- CSS contain для изоляции

### Responsive:
- Mobile-first подход
- Breakpoints: 768px, 1024px
- Touch-friendly (увеличенные зоны клика)
- Отключение некоторых эффектов на мобильных

---

## 🎯 ЧТО НУЖНО РАЗРАБОТЧИКАМ

### Для полной реализации:

1. **Реальные изображения:**
   - Фото гор Кыргызстана (высокое разрешение)
   - Фото продукции (профессиональные)
   - Фото производства
   - Логотип в векторе

2. **3D модели бутылок (опционально):**
   - Three.js или Spline для настоящего 3D
   - Или улучшенный CSS 3D
   - WebGL для сложных эффектов

3. **Видео для Hero:**
   - Видео гор Кыргызстана (loop)
   - Или качественное фото
   - Формат: MP4, WebM

4. **Карта мира:**
   - Детальная SVG карта
   - Или интеграция с Google Maps
   - Все страны СНГ

5. **Анимации:**
   - Lottie анимации (опционально)
   - GSAP для сложных эффектов
   - ScrollMagic для scroll-based

6. **Backend:**
   - Форма обратной связи
   - CMS для контента
   - Админ-панель

---

## 📊 БИБЛИОТЕКИ (рекомендации)

### Обязательно:
- **Нет** - всё на чистом JS/CSS

### Опционально для улучшения:

**Анимации:**
- GSAP (GreenSock) - сложные анимации
- Lottie - векторные анимации
- ScrollMagic - scroll-based animations

**3D:**
- Three.js - настоящий 3D для бутылок
- Spline - 3D дизайн и экспорт

**Карты:**
- Mapbox - кастомные карты
- Leaflet - легковесные карты

**Оптимизация:**
- Lazy loading для изображений
- IntersectionObserver polyfill
- Smooth scroll polyfill

---

## 🚀 ЭТАПЫ РАЗРАБОТКИ

### Этап 1: Базовая вёрстка
- HTML структура
- CSS стили
- Responsive версия
- **Срок:** 3-5 дней

### Этап 2: Простые анимации
- Fade in/out
- Hover эффекты
- Transitions
- **Срок:** 2-3 дня

### Этап 3: Сложные анимации
- 3D бутылки
- Параллакс
- Scroll animations
- **Срок:** 5-7 дней

### Этап 4: Интеграция
- Реальные изображения
- Видео
- Формы
- Backend
- **Срок:** 3-5 дней

### Этап 5: Оптимизация
- Производительность
- Тестирование
- Bugfixes
- **Срок:** 2-3 дня

**ИТОГО:** ~15-23 дня разработки

---

## ⚠️ ВАЖНЫЕ МОМЕНТЫ

### Performance:
- Анимации должны быть 60 FPS
- Lazy loading для тяжёлых элементов
- Оптимизация для мобильных

### Browser support:
- Chrome/Edge (последние версии)
- Safari (последние версии)
- Firefox (последние версии)
- Mobile Safari
- Mobile Chrome

### Accessibility:
- Keyboard navigation
- Screen reader friendly
- Reduced motion для тех кто не хочет анимации

### SEO:
- Meta tags
- Open Graph
- Structured data
- Sitemap

---

## 📝 КОММЕНТАРИИ К ЭСКИЗУ

Файл `kelechek_premium_mockup.html` содержит:

✅ Все анимации работают  
✅ 3D эффекты реализованы  
✅ Параллакс функционирует  
✅ Scroll animations активны  
✅ Hover эффекты везде  
✅ Responsive design  
✅ Чистый код с комментариями  

**Можно открывать в браузере и сразу показывать!**

---

## 🎯 ФИНАЛЬНАЯ ЦЕЛЬ

Создать сайт уровня:
- 5scontent.com/about/ - по интерактивности
- abdysh-ata.kg - по премиальности
- Apple.com - по минимализму
- Tesla.com - по инновациям

**ПРЕМИУМ САЙТ**, которым можно ГОРДИТЬСЯ! 🚀

---

_Техническая спецификация создана: 11 ноября 2025_  
_Проект: Kelechek Premium Website_  
_Для: Команда разработчиков_