# Домашнее задание к работе 16
## Условие задачи
Массив d0, d1, d2, ..., dh составленный из последовательности из массивов
а0, а1, а2, ..., an-1, b0, b1, b2 ..., bm-1, с0, с1,с2, ..., сl-1.исключением минимальных
значений в каждом из массивов
## 1. Алгоритм и блок-схема
## Алгоритм
1. Начало.
2. Установить локаль и инициализировать генератор случайных чисел:
   srand(time(NULL));
3. Сгенерировать размеры трёх массивов случайно от 10 до 50:
   size_a = rand() % 41 + 10;
   size_b = rand() % 41 + 10;
   size_c = rand() % 41 + 10;
4. Выделить динамическую память для массивов a, b, c:
   a = malloc(size_a * sizeof(double));
   b = malloc(size_b * sizeof(double));
   c = malloc(size_c * sizeof(double));
5. Заполнить массивы случайными числами:
   full_elements(a, size_a);
   full_elements(b, size_b);
   full_elements(c, size_c);
6. Вывести массивы a, b, c на экран с указанием размеров:
   put_elements(a, size_a);
   put_elements(b, size_b);
   put_elements(c, size_c);
7. Сформировать массив D по заданному варианту:
   d = build_d(a, size_a, b, size_b, c, size_c, &size_d);
8. Проверить успешность выделения памяти для D.
9. Вывести массив D на экран:
   put_elements(d, size_d);
10. Освободить память для всех массивов:
    free(a); free(b); free(c); free(d);
11. Конец.
### Блок-схема
<img width="609" height="1892" alt="image" src="https://github.com/user-attachments/assets/dd5352a7-a2c8-42af-bc3a-61095653b6dc" />





## 2. Реализация программы

```
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <locale.h>

double* full_elements(double* ptr_array, int n) {
    for (int i = 0; i < n; i++) {
        ptr_array[i] = (double)rand() / RAND_MAX * 100.0;
    }
    return ptr_array;
}

int put_elements(double* ptr_array, int size) {
    for (int i = 0; i < size; i++) {
        printf("%.2lf ", ptr_array[i]);
    }
    printf("\n");
    return 0;
}

double* build_d(double* a, int size_a, double* b, int size_b, double* c, int size_c, int* size_d) {
    double min_a = a[0], min_b = b[0], min_c = c[0];

    for (int i = 1; i < size_a; i++) 
        if (a[i] < min_a) min_a = a[i];
    for (int i = 1; i < size_b; i++) 
        if (b[i] < min_b) min_b = b[i];
    for (int i = 1; i < size_c; i++) 
        if (c[i] < min_c) min_c = c[i];

    int count = 0;
    for (int i = 0; i < size_a; i++) 
        if (a[i] != min_a) count++;
    for (int i = 0; i < size_b; i++) 
        if (b[i] != min_b) count++;
    for (int i = 0; i < size_c; i++) 
        if (c[i] != min_c) count++;

    *size_d = count;
    double* d = (double*)malloc((*size_d) * sizeof(double));
    if (!d) return NULL;

    int idx = 0;
    for (int i = 0; i < size_a; i++) 
        if (a[i] != min_a) d[idx++] = a[i];
    for (int i = 0; i < size_b; i++) 
        if (b[i] != min_b) d[idx++] = b[i];
    for (int i = 0; i < size_c; i++) if (c[i] != min_c) d[idx++] = c[i];

    return d;
}

int main() {
    setlocale(LC_ALL, "RUS");
    srand(time(NULL));

    int size_a = rand() % 41 + 10;
    int size_b = rand() % 41 + 10;
    int size_c = rand() % 41 + 10;

    double* a = (double*)malloc(size_a * sizeof(double));
    double* b = (double*)malloc(size_b * sizeof(double));
    double* c = (double*)malloc(size_c * sizeof(double));

    if (!a || !b || !c) {
        puts("Ошибка выделения памяти");
        return -1;
    }

    full_elements(a, size_a);
    full_elements(b, size_b);
    full_elements(c, size_c);

    printf("Массив A (size=%d):\n", size_a);
    put_elements(a, size_a);

    printf("\nМассив B (size=%d):\n", size_b);
    put_elements(b, size_b);

    printf("\nМассив C (size=%d):\n", size_c);
    put_elements(c, size_c);

    int size_d;
    double* d = build_d(a, size_a, b, size_b, c, size_c, &size_d);
    if (!d) {
        free(a); free(b); free(c);
        puts("Ошибка выделения памяти для массива D");
        return -1;
    }

    printf("\nМассив D (все элементы кроме минимальных в каждом массиве):\n");
    put_elements(d, size_d);

    free(a);
    free(b);
    free(c);
    free(d);

    return 0;
}

```


## 3. Результаты работы программы
<img width="1474" height="467" alt="image" src="https://github.com/user-attachments/assets/ed68216d-efad-4780-bd1e-500de3f4a07c" />



