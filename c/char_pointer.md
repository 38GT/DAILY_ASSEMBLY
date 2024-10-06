<h1 style="font-size: 48px;">char*</h1>


1. char*는 문자(character)를 가리키는 포인터(pointer)를 의미합니다. 
2. 즉, 문자열을 메모리에서 가리키는 역할을 합니다.

```c
char* str = "Hello";
```
3. null 문자: 문자열의 끝을 나타내기 위해 null 문자(\0)가 반드시 포함되어야 합니다.
4. 가변적 크기: char*로 선언된 문자열은 길이가 고정되지 않으며, 다른 메모리 주소를 가리키게 할 수 있습니다.

```c
#include <stdio.h>

void main() {
    char str2[] = "Hello";
    char* str = "Hello";
    printf("%s\n",str2);
    printf("%s\n",str);
}
```
5. 위처럼 string을 표현하는 두 가지 방법 1. char* 2.char 배열