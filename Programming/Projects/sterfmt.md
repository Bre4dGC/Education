# sterfmt

SterFmt combines the familiarity of standard printf formatting with the power of ANSI escape codes and modern formatting features. Create stunning CLI output with minimal effort!
```c
sfprint("Name: <fg=blue>{s:italic}</> Age: <bold>{d}</>", name, age);
```
---
## Features Showcase

1. Rich Text Formatting
```c
sfprint("<fg=blue, bg=yellow, bold, underline>Hello World!</>");
```

2. Nested Styles
```c
sfprint("Normal <bold>Bold <italic>Bold+Italic</italic> Back to Bold</bold> Normal");
```

3. Conditional Formatting
```c
sfprint("Status: <if={d} == 0><fg=green>OK<else><fg=red>ERROR</if>", status);
```

4. Value-Dependent Colors
```c
sfprint("Temp: <fg={d}<30 ? blue : red>{d}°C", temperature);
```

5. Progress Bars
```c
sfprint("Download: [<progress:{d} / 100 : width=20:fg=green>] {d}%", progress);
```

6. Tabular Data
```c
sfprint("<table:border=1> \
  <row><cell:align=center>Name</><cell>Age</><cell>Score</> \
  <row><cell>Alice</><cell:fg=red>24</><cell>95.5</> \
  <row><cell>Bob</><cell>31</><cell>87.2</> \
</table>");
```
```
┌───────┬─────┬───────┐
│ Name  │ Age │ Score │
├───────┼─────┼───────┤
│ Alice │ 24  │ 95.5  │
│ Bob   │ 31  │ 87.2  │
└───────┴─────┴───────┘
```

7. RGB and Hex Color Support
```c
sfprint("<fg=rgb(255,100,50)>RGB orange text</>");
sfprint("<bg=hex(FFFFFF)>Hex white background</>");
```

8. Text Animations
```c
sfprint("<blink>Alert!</> <marquee>Important message...</>");
```

9. Format String Reuse
```c
sterfmt_t* fmt = sfcompile("<fg=blue>{s}</>");
sfprint(fmt, "Hello"); // Blue text
sfprint(fmt, "World"); // Also blue text
```
---
## Installation

#### Clone the repository
```bash
git clone https://github.com/Bre4dGC/sterfmt.git
cd sterfmt
```
#### Build and install
```bash
mkdir build && cd build
cmake ..
make
sudo make install
```
---
## Basic Usage
```c
#include <sterfmt.h>

int main(void)
{
    char* name = "Alice";
    int age = 25;
    double temp = 28.5;
    
    sfprint("Hello! <fg=green, bold>{s}</> You're <fg={d} < 30 ? blue : red>{f}</> years old", name, age);
    sfprint("Current temp: <fg=rgb(255,100,50)>{.1f}°C</>", temp);
    
    return 0;
}
```
---
## SterFmt is optimized for speed:
- Format strings are pre-compiled to AST
- Minimal memory allocations during rendering
- Efficient ANSI code generation

---
## Roadmap
- [ ] Core formatting engine
- [ ] ANSI color support
- [ ] Formatting input
- [ ] Windows console support
- [ ] VGA mode support
- [ ] More text animations
- [ ] Custom formatter registration

---
## Contributing
We welcome contributions! Please see our Contribution Guidelines.

## License
MIT License