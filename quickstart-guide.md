# 🚀 Quick Start Guide

Запустіть реферальну систему за 15 хвилин!

## ⚡️ За 3 кроки до запуску

### 📦 Крок 1: Підготовка (5 хвилин)

#### 1.1 Створіть Telegram бота

```
1. Відкрийте @BotFather в Telegram
2. Надішліть /newbot
3. Вкажіть назву: "My Referral Bot"
4. Вкажіть username: "my_referral_bot"
5. ЗБЕРЕЖІТЬ токен: 1234567890:ABCdefGHIjklMNOpqrsTUVwxyz
```

#### 1.2 Створіть Google Sheets таблицю

```
1. Перейдіть на sheets.google.com
2. Створіть нову таблицю: "Referral Competition"
3. Перейменуйте лист на: "Users"
4. Додайте заголовки:
   user_id | username | chat_id | invited_by | join_date | total_invites | is_participant | reward_sent
5. СКОПІЮЙТЕ ID з URL: 
   https://docs.google.com/spreadsheets/d/[ЦЕ_ВАШ_ID]/edit
```

#### 1.3 Дізнайтеся User ID учасників

**Варіант А: Через @userinfobot**
```
1. Учасники пишуть @userinfobot
2. Бот показує їх User ID
3. Зберігаємо список ID
```

**Варіант Б: Через ваш бот (після налаштування)**
```
1. Учасники пишуть /start вашому боту
2. Дивитесь логи N8N
3. Знаходите user_id в Parse Telegram Message
```

---

### ⚙️ Крок 2: Налаштування N8N (5 хвилин)

#### 2.1 Імпорт workflow

```bash
# У N8N:
1. Workflows → Import from File
2. Виберіть telegram-referral-workflow.json
3. Натисніть Import
```

#### 2.2 Налаштування Credentials

**Telegram Bot:**
```
1. Credentials → Add New → Telegram API
2. Вставте Bot Token з BotFather
3. Збережіть як "Referral Bot"
```

**Google Sheets:**
```
1. Credentials → Add New → Google Sheets OAuth2 API
2. Натисніть "Connect Google Account"
3. Авторизуйтесь через Google
4. Збережіть як "Sheets Account"
```

#### 2.3 Налаштування конфігурації

Відкрийте вузол **"Workflow Configuration"**:

```javascript
{
  "channelLink": "https://t.me/your_channel",
  "botUsername": "my_referral_bot",  // БЕЗ @
  "spreadsheetId": "1AbCdEfG...",    // З п.1.2
  "sheetName": "Users",
  "allowedParticipants": [
    "123456789",  // ID учасника 1
    "987654321",  // ID учасника 2
    "111222333",  // ID учасника 3
    "444555666",  // ID учасника 4
    "777888999",  // ID учасника 5
    "100200300",  // ID учасника 6
    "400500600",  // ID учасника 7
    "700800900"   // ID учасника 8
  ],
  "bonusThreshold": 5
}
```

#### 2.4 Підключення credentials до вузлів

**Для всіх Telegram вузлів:**
```
1. Клік на вузол (Send Welcome Message, тощо)
2. Credential → "Referral Bot"
```

**Для всіх Google Sheets вузлів:**
```
1. Клік на вузол (Get User Stats, тощо)
2. Credential → "Sheets Account"
```

---

### 🎯 Крок 3: Запуск (5 хвилин)

#### 3.1 Тестування

```
1. Збережіть workflow (Ctrl+S)
2. Активуйте workflow (кнопка Active вгорі)
3. Відкрийте свого бота в Telegram
4. Напишіть /start
```

**Очікуваний результат:**
```
Вітаємо! Ви успішно приєдналися. 🎉

Приєднуйтесь до нашого каналу:
https://t.me/your_channel
```

#### 3.2 Перевірка функцій

**Тест 1: Реєстрація учасника**
```
1. Учасник пише /start
2. Перевірте Google Sheets - з'явився новий рядок
3. is_participant = true
```

**Тест 2: Реферальна система**
```
1. Учасник надсилає своє посилання другу
2. Друг натискає посилання → /start
3. Учасник отримує повідомлення: "+1 запрошення!"
4. В Google Sheets total_invites збільшилось
```

**Тест 3: Команди**
```
/myref → Показує статистику учасника
/top → Показує лідерборд усіх 8 учасників
```

**Тест 4: Захист**
```
1. Учасник натискає власне реферальне посилання
2. Результат: запрошення НЕ зараховується ✅
```

#### 3.3 Фінальна перевірка

✅ Checklist:
- [ ] Бот відповідає на /start
- [ ] Дані записуються в Google Sheets
- [ ] Реферальні посилання працюють
- [ ] Команди /myref та /top працюють
- [ ] Захист від самозапрошення працює
- [ ] Учасники отримують сповіщення

---

## 🎊 Готово! Ваша система працює!

### 📱 Надішліть учасникам:

```
🏆 Вітаємо! Ви учасник реферального змагання!

Ваш бот: @my_referral_bot

Команди:
/start - Початок та отримання посилання
/myref - Ваша статистика
/top - Топ учасників

Запрошуйте друзів та перемагайте! 🚀
```

---

## ❓ Швидке вирішення проблем

### Бот не відповідає

```bash
# Перевірка 1: Чи активний workflow?
→ N8N → Workflows → Active: ON

# Перевірка 2: Чи правильний токен?
→ Credentials → Telegram API → Test

# Перевірка 3: Перегляд логів
→ N8N → Executions → Latest
```

### Google Sheets помилка

```bash
# Перевірка 1: Чи правильний ID таблиці?
→ Workflow Configuration → spreadsheetId

# Перевірка 2: Чи надано доступ?
→ Google Sheets → Share → Email з credentials

# Перевірка 3: Чи правильна назва листа?
→ Має бути точно "Users"
```

### Команди не працюють

```bash
# Перевірка 1: Чи є user_id в allowedParticipants?
→ Workflow Configuration → allowedParticipants

# Перевірка 2: Перезапустіть workflow
→ Active: OFF → Active: ON

# Перевірка 3: Очистіть кеш бота
→ Telegram → Settings → Clear Cache
```

---

## 📚 Наступні кроки

### Кастомізація

1. **Зміна текстів**
   ```
   → Вузли "Send ..." → Edit Text
   ```

2. **Зміна порогу бонусу**
   ```
   → Workflow Configuration → bonusThreshold: 10
   ```

3. **Зміна частоти лідерборду**
   ```
   → Weekly Leaderboard Trigger → Schedule
   ```

### Моніторинг

1. **Перегляд статистики**
   ```
   → Google Sheets → Data → Create Chart
   ```

2. **Експорт даних**
   ```
   → Google Sheets → File → Download → CSV
   ```

3. **Логи**
   ```
   → N8N → Executions → Filter by Status
   ```

---

## 🆘 Потрібна допомога?

- 📖 [Повна документація](README.md)
- 🔐 [Безпека](SECURITY.md)
- 🤝 [Contributing](CONTRIBUTING.md)
- 🐛 [Report Bug](https://github.com/yourusername/telegram-referral-competition/issues)

---

## 💡 Pro Tips

### Tip 1: Бекапи

```bash
# Автоматичний бекап Google Sheets:
1. Google Sheets → File → Version History
2. Або: Tools → Script Editor → Automation
```

### Tip 2: Тестовий бот

```bash
# Створіть окремий тестовий бот:
1. @BotFather → /newbot → "Test Bot"
2. Клонуйте workflow в N8N
3. Тестуйте без ризику для production
```

### Tip 3: Аналітика

```bash
# Google Sheets формули для аналітики:
=COUNTIF(G:G, TRUE)  // Кількість учасників
=SUM(F:F)            // Всього запрошень
=AVERAGE(F:F)        // Середня кількість запрошень
```

---

**Успіхів з вашим змаганням! 🎉**

Є питання? → [Create Issue](https://github.com/yourusername/telegram-referral-competition/issues/new)