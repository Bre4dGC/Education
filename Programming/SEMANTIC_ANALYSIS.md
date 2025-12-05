# Семантический анализ в Bread Crumbs

## Обзор

Семантический анализ - это третья фаза компиляции после лексинга и парсинга. Он проверяет **смысловую корректность** программы.

## Архитектура

```
Lexer → Parser → AST → Semantic Analyzer → Annotated AST + Errors
```

### Компоненты:

1. **Type System** (`types.h/c`)
   - Представление типов
   - Проверка совместимости типов
   - Type inference (вывод типов)

2. **Symbol Table** (`symbol.h/c`)
   - Hash-based таблица символов с O(1) lookup
   - Nested scopes (вложенные области видимости)
   - Name resolution (разрешение имен)

3. **Semantic Checker** (`checker.h/c`)
   - Проверка типов
   - Проверка потока управления
   - Валидация семантических правил

## Проверки

### 1. Type Checking (проверка типов)
```c
var x: int = "hello";  // ❌ Error: type mismatch
var y: int = 42;       // ✅ OK
```

### 2. Name Resolution
```c
var x = y + 5;  // ❌ Error: undefined variable 'y'
```

### 3. Type Inference
```c
var x = 42;         // ✅ Inferred as int
var y = 3.14;       // ✅ Inferred as float
var z = "hello";    // ✅ Inferred as str
```

### 4. Function Validation
```c
func foo() => int {
    // ❌ Error: missing return statement
}

func bar() => int {
    return 42;  // ✅ OK
}
```

### 5. Const Checking
```c
const PI = 3.14159;
PI = 3.14;  // ❌ Error: cannot assign to constant
```

### 6. Control Flow
```c
func test() {
    break;  // ❌ Error: break outside loop
}

while(true) {
    break;  // ✅ OK
}
```

## Использование

```c
#include "compiler/semantic.h"

// 1. Инициализировать систему типов
init_type_system();

// 2. Создать контекст
struct semantic_context* ctx = new_semantic_context();

// 3. Проанализировать AST
bool success = analyze_ast(ctx, root);

// 4. Проверить ошибки
if(!success || ctx->error_count > 0) {
    for(size_t i = 0; i < ctx->error_count; i++) {
        print_error(ctx->errors[i]);
    }
}

// 5. Очистка
free_semantic_context(ctx);
free_type_system();
```

## Примеры

### Пример 1: Type Inference
```c
// Input code:
var x = 42;
var y = x + 10;

// Semantic analyzer:
// - Infers x: int
// - Infers y: int (int + int = int)
// - ✅ No errors
```

### Пример 2: Type Mismatch
```c
// Input code:
var x: int = 42;
x = "hello";

// Semantic analyzer:
// - x declared as int
// - Trying to assign str to int
// - ❌ Error: type mismatch
```

### Пример 3: Undefined Variable
```c
// Input code:
var x = y + 5;

// Semantic analyzer:
// - Lookup 'y' in symbol table
// - Not found
// - ❌ Error: undefined variable 'y'
```

## Этапы анализа

1. **First Pass** - сбор деклараций
   - Функции
   - Struct/enum/union
   - Глобальные переменные

2. **Second Pass** - проверка тел функций
   - Type checking
   - Control flow validation
   - Name resolution

## Расширения (будущее)

- Generic types
- Type aliases
- Trait constraints
- Borrow checking (для безопасности памяти)
