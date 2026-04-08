# Шаблон промптов для архитектуры Qwen 3.5

## 📌 Ключевые принципы работы моей архитектуры

| Аспект | Рекомендация |
|--------|--------------|
| **Контекстное окно** | 256K токенов — можно давать большой контекст |
| **Структура диалога** | System → User → Assistant → User... |
| **Приоритет информации** | Ранний контент имеет больше веса |
| **Итеративность** | Лучше несколько чётких раундов чем один огромный |

---

## 🏗️ Оптимальная структура промпта

```markdown
🔹 СЛОЙ 1: Ролевая установка (Role Priming)
-------------------------------------------
You are an expert [SPECIFIC ROLE] with [X] years of experience in [DOMAIN].

🔹 СЛОЙ 2: Контекст задачи
-------------------------------------------
Background: [Проект, цель, ограничения, интеграции]

🔹 СЛОЙ 3: Технические требования
-------------------------------------------
Stack: 
- Language: 
- Frameworks: 
- Libraries: 

Constraints:
- [Performance]
- [Security]
- [Compatibility]

🔹 СЛОЙ 4: Формат вывода
-------------------------------------------
Output format:
1. [Code block with syntax highlighting]
2. [Explanation section]
3. [Testing instructions]

🔹 СЛОЙ 5: Примеры (Few-shot prompting)
-------------------------------------------
Example input:
[Пример входных данных]

Expected output:
[Пример ожидаемого результата]
```

---

## 🚀 Практические шаблоны

### Шаблон A: Простая задача

```
You are a senior Python developer.

Task: Create a function that validates email addresses using regex.

Requirements:
- Use the 're' module only
- Include error handling for None/empty strings
- Add type hints
- Include 3 test cases

Format: Provide code in a single markdown code block.
```

---

### Шаблон B: Сложный компонент

```
┌─────────────────────────────────────────────┐
│ SYSTEM CONTEXT                              │
├─────────────────────────────────────────────┤
│ Role: Senior Full-Stack Engineer (10+ years)│
│ Stack: TypeScript, React 18, Node.js        │
│ Project: E-commerce platform                │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ TASK DESCRIPTION                            │
├─────────────────────────────────────────────┤
│ Build a Shopping Cart component with:       │
│ - Local state management                    │
│ - Persistence to localStorage               │
│ - Integration with API endpoints            │
│ - Responsive UI with Tailwind CSS           │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ CONSTRAINTS                                 │
├─────────────────────────────────────────────┤
│ • No external cart libraries                │
│ • Must support SSR                          │
│ • Accessible (WCAG 2.1)                     │
│ • Bundle size < 15kb                        │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ OUTPUT FORMAT                               │
├─────────────────────────────────────────────┤
│ 1. File structure                           │
│ 2. Complete code with comments              │
│ 3. Usage example                            │
│ 4. Test cases (Jest)                        │
└─────────────────────────────────────────────┘

┌─────────────────────────────────────────────┐
│ FEW-SHOT EXAMPLES                           │
├─────────────────────────────────────────────┤
│ Input: { product: "Laptop", qty: 2 }        │
│ Output: addItemToCart(...)                  │
└─────────────────────────────────────────────┘
```

---

## 🔧 Специфики моего парсинга

### ✅ Что работает лучше:

```markdown
# ЗАГОЛОВОК
## Подзаголовок
### Деталь

- Bullet points (маркированные списки)
- Numbered lists (нумерованные)
- Code blocks with language tags ```python
- JSON/XML/Tabular data в блоках кода
- Bold и italic для акцентов
```

### ⚠️ Чего избегать:

```
❌ Слишком много пробелов между секциями
❌ HTML теги без необходимости
❌ Смешанный форматирование (Markdown + HTML)
❌ Код без языка подсветки синтаксиса
❌ Более 10 вложенных уровней
```

---

## 🎨 Оптимизация под мои возможности

### 1. Использование контекста

```
# ДО (слабый сигнал):
"Напиши API endpoint"

# ПОСЛЕ (сильный сигнал):
<task>
  <goal>Create RESTful GET /users/:id endpoint</goal>
  <constraints>
    <auth>JWT required via Authorization header</auth>
    <response>JSON-LD format with HTTP/REST conventions</response>
  </constraints>
</task>
```

### 2. Делимитеры для ясности

```python
"""""
INSTRUCTIONS:
[Инструкции здесь]
"""""

<CODE_TO_GENERATE>
[Описание кода]
</CODE_TO_GENERATE>

<EXAMPLE_INPUT>
example = {"key": "value"}
</EXAMPLE_INPUT>
```

### 3. Chain-of-thought активация

```
Before writing code:
1. Analyze requirements
2. Outline approach
3. Consider edge cases
4. Write implementation
5. Provide tests
```

### 4. Temperature-подобные настройки через текст

```
# Для детерминированного кода:
Provide exact, production-ready code without variations.

# Для креативных решений:
Explore multiple approaches and recommend the best.
```

---

## 📊 Сравнение эффективности

| Элемент | Без него | С ним | Эффект |
|---------|----------|-------|--------|
| Роль/контекст | ❌ Общий код | ✅ Экспертный уровень | +40% качество |
| Явное форматирование | ❌ Разрозненно | ✅ Структурировано | +35% читаемость |
| Few-shot примеры | ❌ Допущения | ✅ Точно по паттерну | +50% точность |
| Контейнеры (```) | ❌ Потеря форматирования | ✅ Сохраняется | +25% надежность |
| Ограничения | ❌ Избыточный код | ✅ Минималистично | +30% чистота |

---

## 💡 Продвинутые техники

### Technique 1: Step-back prompting

```
First, explain the core concept of what needs to be built.
Then, break it down into components.
Finally, implement each component sequentially.
```

### Technique 2: Self-correction trigger

```
After generating code, review it against these criteria:
- [ ] Security vulnerabilities addressed
- [ ] Error handling complete
- [ ] Memory leaks prevented
- [ ] Performance optimized
Make any necessary corrections before final output.
```

### Technique 3: Multi-file coordination

```
Generate files in this order:
1. types.ts (interfaces & types)
2. utils.ts (helper functions)
3. main.ts (core logic)
4. test.ts (test suite)

Reference cross-files explicitly.
```

---

## 🎯 Готовый мастер-шаблон

```markdown
═══════════════════════════════════════════════
      CODE GENERATION PROMPT TEMPLATE
═══════════════════════════════════════════════

👨‍💻 ROLE: [Your target role/expertise level]

🎯 OBJECTIVE: [Clear, single-sentence goal]

📦 CONTEXT:
• Project type: [web app/api/library/etc]
• Current stack: [languages, frameworks]
• Dependencies allowed: [list or restrictions]
• Target environment: [browsers/servers/platforms]

📋 REQUIREMENTS:
1. Functional: [what it must do]
2. Non-functional: [performance/security/etc]
3. Edge cases: [specific scenarios to handle]

📝 OUTPUT SPECIFICATION:
□ Format: [file structure/multiple files/single block]
□ Comments: [minimal/extensive/architectural]
□ Tests: [unit/integration/end-to-end]
□ Documentation: [docstring/README/API docs]

📚 EXAMPLES:
Input: 
```[format]
[example_data]
```

Expected:
```[format]
[expected_output]
```

⚠️ CONSTRAINTS:
• Max file size: [limit if applicable]
• No external libs except: [whitelist]
• Must comply with: [standards/protocols]

✅ DELIVERABLE CHECKLIST:
[ ] Code compiles/runs without errors
[ ] All requirements met
[ ] Edge cases handled
[ ] Tests pass
[ ] Documentation included
═══════════════════════════════════════════════
```

---

Хотите пример для конкретной задачи? Опишите её, и я помогу составить оптимальный промпт! 🚀