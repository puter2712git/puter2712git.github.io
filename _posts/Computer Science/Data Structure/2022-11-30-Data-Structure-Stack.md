---
title: "Stack"
excerpt: "자료구조 Stack에 대한 포스트"

categories:
  - Data Structure
tags:
  - Computer Science
  - Data Structure

toc: true
toc_sticky: true

date: 2022-11-30
last_modified_at: 2022-11-30

use_math: true
---

## 1. Stack이란?

`top`의 자료로 삽입과 삭제를 처리하는 자료구조

$S = {a_{0}, \;a_{1}, \;..., \;a_{n-1}}$으로 stack이 주어질 때,
- $a_{0}$은 가장 아래쪽의 원소
- $a_{n-1}$은 가장 위쪽의 원소(`top` element)이다.

**LIFO**(LAST-IN-FIRST-OUT) 형태의 자료구조로,<br>
가장 나중에 들어온 원소가 처음으로 나가는 형태이다.

<br>

## 2. Stack 구현 (C)

구현할 함수들은 다음과 같다.

- `void initialize(struct stack **_stack);` : `_stack`을 초기화 (초기 실행)
- `void finalize(struct stack **_stack);` : `_stack` 종료 (마무리 단계에서 실행)
- `bool is_empty(struct stack *_stack);` : `_stack`이 비어있는지 확인
- `bool is_full(struct stack *_stack);` : `_stack`이 다 찼는지 확인
- `void push(struct stack *_stack, element _item);` : `_stack`에 `_item` 원소를 삽입 (push 명령)
- `element pop(struct stack *_stack);` : `_stack`의 top 원소를 삭제 (pop 명령)
- `void dump_stack(struct stack *_stack);` : `_stack`의 모든 원소들을 출력

위 함수들을 `stack.h` 헤더파일에 따로 선언해 주었다.

```c
#ifndef __STACK_H__
#define __STACK_H__

#include <stdbool.h>

typedef int element;


/* Stack data structure which store string elements */
struct stack {
  element *data;
  int top;
  int size;
};

/* Initialize the @_stack */
void initialize(struct stack **_stack);

/* Finalize the @_stack */
void finalize(struct stack **_stack);

/* Check if the @_stack is empty */
bool is_empty(struct stack *_stack);

/* Check if the @_stack is full */
bool is_full(struct stack *_stack);

/* Push the @_item into the @_stack */
void push(struct stack *_stack, element _item);

/* Pop the top of the element of the @_stack */
element pop(struct stack *_stack);

/* Print all elements of the @_stack */
void dump_stack(struct stack *_stack);

#endif
```

<br>

`initialize()` 함수에서는 `_stack`을 동적할당 해주고, 멤버 변수들을 적절하게 초기화 시켜준다.

```c
void initialize(struct stack **_stack) {
  *_stack = (struct stack*)malloc(sizeof(struct stack));
  (*_stack)->data = NULL;
  (*_stack)->top = -1;
  (*_stack)->size = 0;

  return;
}
```

<br>

`finalize()` 함수에서는 `_stack`의 메모리를 해제시켜준다.

```c
void finalize(struct stack **_stack) {
  free(*_stack);

  return;
}
```

<br>

`is_empty()` 함수에서는 `_stack`이 비어있다면 `true`, 아니라면 `false`를 반환한다.

```c
bool is_empty(struct stack *_stack) {
  return _stack->size == 0;
}
```

<br>

`is_full()` 함수에서는 `_stack`이 꽉 차있다면 `true`, 아니라면 `false`를 반환한다.

```c
bool is_full(struct stack *_stack) {
  return _stack->top + 1 == _stack->size;
}
```

<br>

`push()` 함수에서는 먼저 `_stack`이 꽉 차있는지를 확인하고, 꽉 차있다면 메모리 주소를 더 할당해준 뒤 `_item`을 삽입한다.

```c
void push(struct stack *_stack, element _item) {
  if (is_full(_stack)) {
    _stack->size++;
    _stack->data = (element*)realloc(_stack->data, _stack->size * sizeof(element));
  }

  _stack->data[++(_stack->top)] = _item;
  return;
}
```

<br>

`pop()` 함수에서는 먼저 `_stack`이 비어있는지를 확인하고, 원소가 존재하면 top 원소를 삭제한 뒤 그 값을 반환한다.<br>
만약 `_stack`이 비어있다면 프로그램을 강제종료한다.

```c
element pop(struct stack *_stack) {
  assert(!is_empty(_stack));

  int popped = _stack->data[_stack->top--];
  _stack->size--;
  _stack->data = (element*)realloc(_stack->data, sizeof(element) * _stack->size);

  return popped;
}
```

<br>

`dump_stack()` 함수에서는 먼저 `_stack`이 비어있는지를 확인하고, 원소가 존재하면 `_stack` 안의 모든 원소들을 출력한다.<br>
만약 `_stack`이 비어있다면 프로그램을 강제종료한다.

```c
void dump_stack(struct stack *_stack) {
  assert(!is_empty(_stack));

  printf("==============================\n");
  printf("Print the elements of the stack.\n");
  for (int i = 0; i < _stack->size; i++) {
    printf("[%2d] : %d\n", i, _stack->data[i]);
  }
  printf("==============================\n");

  return;
}
```

<br>

전체 코드는 다음과 같다.

```c
#include <stdio.h>
#include <stdlib.h>
#include <assert.h>

#include "stack.h"

void initialize(struct stack **_stack) {
  *_stack = (struct stack*)malloc(sizeof(struct stack));
  (*_stack)->data = NULL;
  (*_stack)->top = -1;
  (*_stack)->size = 0;

  return;
}

void finalize(struct stack **_stack) {
  free(*_stack);

  return;
}

bool is_empty(struct stack *_stack) {
  return _stack->size == 0;
}

bool is_full(struct stack *_stack) {
  return _stack->top + 1 == _stack->size;
}

void push(struct stack *_stack, element _item) {
  if (is_full(_stack)) {
    _stack->size++;
    _stack->data = (element*)realloc(_stack->data, _stack->size * sizeof(element));
  }

  _stack->data[++(_stack->top)] = _item;
  return;
}

element pop(struct stack *_stack) {
  assert(!is_empty(_stack));

  int popped = _stack->data[_stack->top--];
  _stack->size--;
  _stack->data = (element*)realloc(_stack->data, sizeof(element) * _stack->size);

  return popped;
}

void dump_stack(struct stack *_stack) {
  assert(!is_empty(_stack));

  printf("==============================\n");
  printf("Print the elements of the stack.\n");
  for (int i = 0; i < _stack->size; i++) {
    printf("[%2d] : %d\n", i, _stack->data[i]);
  }
  printf("==============================\n");

  return;
}
```

## 3. GitHub Link

[Stack source code](https://github.com/puter2712git/Stack){:target="_blank"}