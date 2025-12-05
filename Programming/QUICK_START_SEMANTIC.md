# Быстрый старт: Семантический анализ

## Что делать дальше?

Вы завершили лексер и парсер. Теперь пора добавить семантический анализ!

## План действий (пошагово)

### Шаг 1: Понять теорию (5 минут) ✓
Прочитайте `docs/SEMANTIC_ANALYSIS.md`

### Шаг 2: Скомпилировать код (СЕЙЧАС)

```bash
cd /home/bread/dev/projects/bread-crumbs
make clean
make build
```

### Шаг 3: Написать простой тест

Создайте файл `tests/test_semantic.c`:

```c
#include <stdio.h>
#include "compiler/semantic.h"
#include "compiler/parser.h"

int main(void) {
    // Инициализация
    init_type_system();
    
    // Простой тест: type inference
    const char* code = "var x = 42";
    struct lexer* lex = new_lexer(code);
    struct parser* pars = new_parser(lex);
    struct ast_node* ast = parse_expr(pars);
    
    // Семантический анализ
    struct semantic_context* ctx = new_semantic_context();
    bool success = analyze_ast(ctx, ast);
    
    printf("Semantic analysis: %s\\n", success ? "PASS" : "FAIL");
    printf("Errors: %zu\\n", ctx->error_count);
    
    // Cleanup
    free_semantic_context(ctx);
    free_ast(ast);
    free_parser(pars);
    free_type_system();
    
    return success ? 0 : 1;
}
```

### Шаг 4: Обновить Makefile

Добавьте в Makefile:

```makefile
SEMTEST = $(BIN_DIR)/semtest
SRCS_SEMTEST = tests/test_semantic.c \\
               $(SRC_DIR)/compiler/semantic/types.c \\
               $(SRC_DIR)/compiler/semantic/symbol.c \\
               $(SRC_DIR)/compiler/semantic/checker.c \\
               $(SRC_DIR)/compiler/parser.c \\
               $(SRC_DIR)/compiler/lexer.c \\
               $(SRC_DIR)/compiler/lexer/tokenizer.c \\
               $(SRC_DIR)/compiler/parser/ast.c \\
               $(SRC_DIR)/compiler/errors.c

$(SEMTEST): | $(BIN_DIR)
\t$(CC) $(CFLAGS) $(SRCS_SEMTEST) -o $@
```

### Шаг 5: Постепенная реализация checker.c

checker.c - это ~500+ строк кода. Начните с базовых функций:

#### 5.1. Создайте контекст (ГОТОВО ✓)
```c
struct semantic_context* new_semantic_context(void);
void free_semantic_context(struct semantic_context* ctx);
```

#### 5.2. Реализуйте check_variable (СЛЕДУЮЩИЙ ШАГ)
```c
bool check_variable(struct semantic_context* ctx, struct ast_node* node) {
    // 1. Проверить, что переменная не объявлена дважды
    // 2. Вывести тип (type inference)
    // 3. Добавить в symbol table
    // 4. Проверить совместимость типов при инициализации
}
```

#### 5.3. Реализуйте check_var_ref
```c
bool check_var_ref(struct semantic_context* ctx, struct ast_node* node) {
    // 1. Найти переменную в symbol table
    // 2. Если не найдена - ошибка
    // 3. Пометить как использованную
}
```

#### 5.4. Реализуйте check_binary_op
```c
bool check_binary_op(struct semantic_context* ctx, struct ast_node* node) {
    // 1. Проверить левый и правый операнды
    // 2. Проверить совместимость типов
    // 3. Вычислить результирующий тип
}
```

#### 5.5. Реализуйте check_function
```c
bool check_function(struct semantic_context* ctx, struct ast_node* node) {
    // 1. Создать scope для функции
    // 2. Добавить параметры в scope
    // 3. Проверить тело функции
    // 4. Проверить, что все пути возвращают значение
}
```

## Примеры для тестирования

### Тест 1: Type Inference
```c
var x = 42;           // должно вывести: int
var y = 3.14;         // должно вывести: float
var z = "hello";      // должно вывести: str
```

### Тест 2: Type Mismatch
```c
var x: int = "hello"; // должна быть ошибка
```

### Тест 3: Undefined Variable
```c
var x = y + 5;        // должна быть ошибка: y не определена
```

### Тест 4: Redefinition
```c
var x = 42;
var x = 10;           // должна быть ошибка: x уже определена
```

## Чеклист прогресса

- [x] types.h/c - система типов
- [x] symbol.h/c - таблица символов
- [x] checker.h - интерфейс проверщика
- [ ] checker.c - реализация проверщика (В ПРОЦЕССЕ)
  - [ ] new_semantic_context
  - [ ] analyze_ast
  - [ ] check_variable
  - [ ] check_var_ref  
  - [ ] check_binary_op
  - [ ] check_function
  - [ ] check_if/while/for
  - [ ] type inference
- [ ] Тесты
- [ ] Интеграция с main.c

## Полезные команды

```bash
# Скомпилировать
make build

# Запустить парсер тест
./bin/parstest

# Запустить семантический тест (когда создадите)
./bin/semtest

# Проверить утечки памяти
valgrind --leak-check=full ./bin/semtest
```

## Следующие шаги

1. **Сегодня**: Реализовать базовый checker.c (100-200 строк)
   - new_semantic_context
   - analyze_ast (dispatcher)
   - check_variable
   - check_var_ref

2. **Завтра**: Добавить проверку операторов
   - check_binary_op
   - check_unary_op
   - type inference

3. **Послезавтра**: Проверка функций и control flow
   - check_function
   - check_return
   - all_paths_return

## Помощь

Если застряли, посмотрите примеры в:
- **Clang**: `clang/lib/Sema/Sema.cpp`
- **Rust**: `rustc_typeck/src/check/`
- **GCC**: `gcc/c/c-typeck.c`

Или спросите меня! Я помогу реализовать конкретную функцию.
