```rust
// Неизменяемые (по умолчанию)  
let x: u32 = 42;  

// Изменяемые  
mut y: i64 = -100;  

// Тип выводится  
let z = 3.14;  // f64  

if x > 0 {  
    print("Positive");  
} else if x == 0 {  
    print("Zero");  
} else {  
    print("Negative");  
}  

// Как выражение (возвращает значение)  
let abs = if x >= 0 { x } else { -x };

// Классический for  
for i in 0..10 {  
    print(i);  
}  

// Бесконечный цикл  
loop {  
    if condition { break; }  
}  

// while  
while x < 100 {  
    x *= 2;  
}  

fn add(a: u32, b: u32) -> u32 {  
    return a + b;  
}  

// Автовозврат последнего выражения  
fn sub(a: u32, b: u32) -> u32 {  
    a - b  
}  

fn factorial(n: u32) -> u32 {  
    if n <= 1 { 1 }  
    else { n * factorial(n - 1) }  
}  

let square = |x: u32| -> u32 { x * x };  
let result = square(5);  // 25  

// Структура + методы  
struct Vector {  
    x: f32,  
    y: f32,  
}  

impl Vector {  
    fn new(x: f32, y: f32) -> Vector {  
        Vector { x, y }  
    }  

    fn length(&self) -> f32 {  
        sqrt(self.x * self.x + self.y * self.y)  
    }  
}  

let v = Vector::new(3.0, 4.0);  
print(v.length());  // 5.0  

trait Drawable {  
    fn draw(&self);  
}  

impl Drawable for Vector {  
    fn draw(&self) {  
        print("Drawing vector at ({}, {})", self.x, self.y);  
    }  
}  

trait Drawable {  
    fn draw(&self);  
}  

impl Drawable for Vector {  
    fn draw(&self) {  
        print("Drawing vector at ({}, {})", self.x, self.y);  
    }  
}  

// Ручное управление  
let buf = alloc(1024) as ptr[u8];  
// ...  
free(buf);  

// Умные указатели (опционально)  
let smart_ptr = Box::new(42);  // освободится автоматически  

fn worker() {  
    print("Hello from thread!");  
}  

let thread = Thread::spawn(worker);  
thread.join();  

let counter = Atomic[u32]::new(0);  

counter.fetch_add(1);  // атомарный инкремент  

let mutex = Mutex::new(0);  

{  
    let guard = mutex.lock();  
    *guard += 1;  // автоматически разблокируется  
}  

fn read_file(path: str) -> Result<[u8], IOError> {  
    let file = open(path)?;  // ? – ранний возврат ошибки  
    Ok(file.read_all())  
}  

match read_file("config.txt") {  
    Ok(data) => print("Success!"),  
    Err(e) => print("Error: {}", e),  
}  

fn divide(a: f32, b: f32) -> f32 {  
    if b == 0.0 { panic!("Division by zero!"); }  
    a / b  
}  

macro print_hex($expr) {  
    print("Value: 0x{:X}", $expr);  
}  

print_hex!(255);  // "Value: 0xFF"  

// Генерация структур для аппаратных регистров  
define_register CR0 as u32 {  
    paging: 31,  
    cache: 30,  
    // ...  
}  

// Чтение порта ввода-вывода  
let status = inb(0x1F7);  

// Запись в MMIO  
let fb = map_mmio(0xA000_0000, 0x1000);  
fb[0] = 0xFF0000;  // RGB-пиксель  

fn cli() {  
    asm {  
        cli  // x86  
    }  
}  

@module(type = "driver")  
mod network {  
    fn send_packet(data: [u8]) { ... }  
}  

@syscall(number = 1)  
fn sys_write(fd: u32, data: ptr[u8], len: u32) -> i32 { ... }  
```