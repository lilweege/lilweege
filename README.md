<h3 align="center">Hi, I'm Luigi</h3>
<p align="center">
<a href="https://www.linkedin.com/in/luigi-q/">linkedin</a> | <a href="https://luigi.codes">website</a> | <a href="mailto:luigi@quattrociocchi.net">email</a>
</p>

---

MTS at Cerebras Systems

<br>
<details>
<summary>Interesting snippets</summary>
<br>


Stack UB (-O0)
```c
void A() { int x = 42; }
int B() { int x; return x; }

int main() {
    A();
    printf("%d\n", B());
}
```

Register UB (-O0)
```c
void A(int x) {}
int B() { int x; return x; }

int main() {
    A(42);
    printf("%d\n", B());
}
```

Computed GOTO
```c
void jump(void* p) { goto* p; }

int main() {
    void* p = &&foo;
    jump(p);

    printf("A\n");
foo:
    printf("B\n");
}
```

Faster switch
```c++
#include <cstdio>
#include <cstdint>

enum Instruction : uint8_t { Done, A, B, C };

void execute(const Instruction* list) {
    constexpr void* labels[] = {
        [Instruction::Done] = &&Done,
        [Instruction::A] = &&A,
        [Instruction::B] = &&B,
        [Instruction::C] = &&C,
    };
    const auto next = [&] { return labels[*list++]; };
    goto* next();

Done: return;
A:  printf("A\n"); goto* next();
B:  printf("B\n"); goto* next();
C:  printf("C\n"); goto* next();
}

int main() {
    constexpr Instruction list[] = {C, B, A, Done};
    execute(list);
}
```

Duff's device
```c
#include <stddef.h>
#include <stdio.h>

void better_memcpy(void* restrict dest, void* restrict src, size_t n) {
    unsigned char* to = dest;
    unsigned char* from = src;
    int remain = (n + 7) / 8;
    switch (n % 8) {
        case 0: do { *to++ = *from++;
        case 7:      *to++ = *from++;
        case 6:      *to++ = *from++;
        case 5:      *to++ = *from++;
        case 4:      *to++ = *from++;
        case 3:      *to++ = *from++;
        case 2:      *to++ = *from++;
        case 1:      *to++ = *from++;
        } while (--remain > 0);
    }
}

int main() {
    int a[] = { 0, 1, 2, 3, 4, 5, 6, 7, 8 };
    int b[sizeof(a) / sizeof(a[0])];
    better_memcpy(b, a, sizeof(b));
    printf("%d\n", b[8]);
}
```

Hello, World!
```c
#define print(...) puts(#__VA_ARGS__)
int main() { print(Hello, World!); }
```
```c
int main() {
    __uint128_t x = ((__uint128_t)0x290E4D9CB7 << 64) | 0xD740B37ECD996400;
    while (x >>= 7) putchar(x & 0x7F);
}
```

</details>
