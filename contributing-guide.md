# 🤝 Contributing Guide

Дякуємо за інтерес до проєкту! Ми раді вашому внеску.

## 📋 Зміст

- [Як внести вклад](#як-внести-вклад)
- [Процес розробки](#процес-розробки)
- [Стандарти коду](#стандарти-коду)
- [Тестування](#тестування)
- [Pull Request](#pull-request)

## Як внести вклад

### 🐛 Знайшли баг?

1. Перевірте чи немає існуючого [Issue](https://github.com/yourusername/telegram-referral-competition/issues)
2. Якщо немає - створіть новий Issue з:
   - Чіткий заголовок
   - Детальний опис проблеми
   - Кроки для відтворення
   - Очікувана поведінка
   - Скріншоти (якщо можливо)
   - Версія N8N
   - Версія Node.js

**Шаблон Bug Report:**

```markdown
## Опис бага
[Короткий опис проблеми]

## Кроки для відтворення
1. Перейти до '...'
2. Натиснути на '...'
3. Прокрутити до '...'
4. Побачити помилку

## Очікувана поведінка
[Що має відбуватися]

## Фактична поведінка
[Що відбувається насправді]

## Середовище
- N8N версія: [напр. 1.0.0]
- Node.js версія: [напр. 18.0.0]
- ОС: [напр. Ubuntu 22.04]

## Додаткова інформація
[Логи, скріншоти, тощо]
```

### 💡 Є ідея для нової функції?

1. Створіть [Feature Request Issue](https://github.com/yourusername/telegram-referral-competition/issues/new)
2. Опишіть:
   - Проблему яку вирішує
   - Пропоноване рішення
   - Альтернативи
   - Приклади використання

**Шаблон Feature Request:**

```markdown
## Опис функції
[Що ви хочете додати]

## Проблема
[Яку проблему це вирішує]

## Пропоноване рішення
[Як це має працювати]

## Альтернативи
[Інші способи вирішення]

## Приклад використання
```javascript
// Код приклад
```

## Додаткова інформація
[Скріншоти, діаграми, тощо]
```

## Процес розробки

### 1. Fork репозиторію

```bash
# Клонуйте ваш fork
git clone https://github.com/YOUR_USERNAME/telegram-referral-competition.git
cd telegram-referral-competition

# Додайте upstream
git remote add upstream https://github.com/ORIGINAL_OWNER/telegram-referral-competition.git
```

### 2. Створіть гілку

```bash
# Оновіть main
git checkout main
git pull upstream main

# Створіть нову гілку
git checkout -b feature/your-feature-name
# або
git checkout -b fix/your-bug-fix
```

**Найменування гілок:**
- `feature/` - нові функції
- `fix/` - виправлення багів
- `docs/` - документація
- `refactor/` - рефакторинг
- `test/` - тести

### 3. Зробіть зміни

#### Перед початком роботи:

```bash
# Встановіть N8N локально (якщо ще не встановлено)
npm install -g n8n

# Запустіть N8N
n8n start
```

#### Під час розробки:

1. Імпортуйте workflow в N8N
2. Внесіть зміни через N8N UI
3. Експортуйте оновлений workflow
4. **Важливо:** Замініть всі реальні дані на placeholder'и

```bash
# Перевірка на реальні дані
grep -r "bot.*token" telegram-referral-workflow.json
grep -r "[0-9]{9,}" telegram-referral-workflow.json  # User IDs
```

### 4. Тестування

#### Мануальне тестування:

- [ ] Тестовий бот створено
- [ ] Тестова Google Sheets таблиця налаштована
- [ ] Протестовано реєстрацію нового користувача
- [ ] Протестовано реферальну систему
- [ ] Протестовано команду /myref
- [ ] Протестовано команду /top
- [ ] Протестовано захист від самозапрошення
- [ ] Протестовано систему бонусів
- [ ] Протестовано щотижневий лідерборд

#### Перевірка безпеки:

```bash
# Запустіть security check
./scripts/security-check.sh

# Або вручну:
# 1. Немає реальних токенів
# 2. Немає реальних User ID
# 3. Немає приватних посилань
# 4. Все замінено на YOUR_* або placeholder'и
```

### 5. Commit зміни

Використовуйте [Conventional Commits](https://www.conventionalcommits.org/):

```bash
# Приклади:
git commit -m "feat: add multi-language support"
git commit -m "fix: prevent self-referral bug"
git commit -m "docs: update installation guide"
git commit -m "refactor: improve code readability"
git commit -m "test: add unit tests for validation"
```

**Формат commit message:**

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat` - нова функція
- `fix` - виправлення бага
- `docs` - документація
- `style` - форматування
- `refactor` - рефакторинг
- `test` - тести
- `chore` - технічні зміни

### 6. Push та створення Pull Request

```bash
# Push в ваш fork
git push origin feature/your-feature-name

# Створіть Pull Request через GitHub UI
```

## Стандарти коду

### JavaScript код в N8N вузлах

```javascript
// ✅ Добре
const items = $input.all();
const config = $('Workflow Configuration').first().json;

// Перевірка на існування
if (!items || items.length === 0) {
  return [];
}

// Чіткі назви змінних
const userId = String(from.id);
const isParticipant = allowedParticipants.includes(userId);

// Коментарі для складної логіки
// Захист від самозапрошення
if (referrer.user_id === newUserId) {
  console.log('⛔️ Self-referral attempt blocked');
  return [];
}
```

```javascript
// ❌ Погано
const i = $input.all();
const c = $('Workflow Configuration').first().json;

// Немає перевірок
items[0].json.something

// Незрозумілі назви
const x = String(from.id);

// Немає коментарів до складної логіки
if (r.u === n) return [];
```

### Структура workflow

1. **Логічне групування вузлів**
   - Пов'язані вузли поруч
   - Використовуйте Sticky Notes для пояснень

2. **Називання вузлів**
   - Чіткі описові назви
   - Англійською мовою для consistency
   - Приклад: "Check If Participant" замість "IF Node 1"

3. **Обробка помилок**
   - Завжди додавайте Error Workflow
   - Логуйте помилки
   - Повертайте зрозумілі повідомлення користувачу

### Документація

1. **README.md**
   - Оновлюйте при додаванні функцій
   - Додавайте приклади використання
   - Оновлюйте FAQ

2. **Коментарі в коді**
   - Поясніть "чому", а не "що"
   - Документуйте складні алгоритми
   - Використовуйте JSDoc для функцій

3. **CHANGELOG.md**
   - Документуйте всі зміни
   - Використовуйте semantic versioning
   - Групуйте по типах змін

## Pull Request

### Checklist перед створенням PR

- [ ] Код протестовано
- [ ] Документація оновлена
- [ ] Немає реальних чутливих даних
- [ ] Commit messages слідують Conventional Commits
- [ ] Пройшли всі перевірки
- [ ] Додано приклади (якщо потрібно)

### Шаблон Pull Request

```markdown
## Опис
[Що змінюється та чому]

## Тип зміни
- [ ] Bug fix (non-breaking change)
- [ ] New feature (non-breaking change)
- [ ] Breaking change
- [ ] Documentation update

## Тестування
[Як це було протестовано]

## Screenshots (якщо є)
[Додайте скріншоти]

## Checklist
- [ ] Код протестовано
- [ ] Документація оновлена
- [ ] Немає чутливих даних
- [ ] Слідую contribution guidelines
```

### Review процес

1. Maintainer перевірить ваш PR
2. Можливі запити на зміни
3. Після approve - merge
4. Ваш внесок у CONTRIBUTORS.md!

## Стиль коду

### Загальні правила

- Використовуйте 2 spaces для відступів
- Максимальна довжина рядка: 100 символів
- Завжди використовуйте `const` замість `var`
- Використовуйте template literals: `${variable}`
- Додавайте trailing comma в об'єктах

### Приклад:

```javascript
// ✅ Добре
const userData = {
  userId: user.id,
  username: user.username,
  isActive: true,
};

const message = `User ${userData.username} has ${count} invites`;

// ❌ Погано
var userData = {userId: user.id,username: user.username,isActive: true}
var message = 'User ' + userData.username + ' has ' + count + ' invites'
```

## Спільнота

### Комунікація

- 💬 GitHub Discussions - загальні питання
- 🐛 GitHub Issues - баги та features
- 📧 Email - приватні питання безпеки

### Поведінка

- Будьте ввічливими та професійними
- Конструктивна критика вітається
- Повага до різних думок
- Фокус на проблемі, а не на особистостях

### Визнання

Всі contributor'и будуть додані до:
- CONTRIBUTORS.md
- Release notes
- GitHub contributors list

## Ліцензія

Вносячи вклад, ви погоджуєтесь що ваш код буде під MIT License.

---

**Дякуємо за ваш внесок! 🎉**

Questions? Open an issue or contact [your-email@example.com]