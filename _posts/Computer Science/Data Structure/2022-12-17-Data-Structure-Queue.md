---
title: "Queue"
excerpt: "자료구조 Queue에 대한 포스트"

categories:
  - Data Structure
tags:
  - Computer Science
  - Data Structure

toc: true
toc_sticky: true

date: 2022-12-17
last_modified_at: 2022-12-17
---

## 1. Queue란?

`rear`와 그 반대편의 `front` 두 개의 데이터를 이용하는 자료구조

$Q = {(a_{0}, ..., a_{n-1})}$일 때, <br>
- $a_{0}$은 `front element`
- $a_{n-1}$은 `rear element`
- $a_{i}$는 $a_{i-1}$ 뒤의 노드이다. ($0<i<n$)

**FIFO**(FIRST-IN-FIRST-OUT) 형태의 자료구조로,<br>
`rear`로 새로운 **element**가 추가되고, `front`로 **element**를 삭제 및 **get**할 수 있다.

<br>

## 2. Queue 구현 (C)

구현할 함수들은 다음과 같다.

- `void initialize(struct queue **_head);` : `queue`를 초기화 (초기 실행)
- `void finalize(struct queue **_head);` : `queue` 종료 (마무리 단계에서 실행)
- `bool is_empty(struct queue *_head);` : `queue`가 비어있는지 확인
- `void enqueue(struct queue *_head, element _item);` : `_queue`에 `_item` 원소를 가진 노드를 추가
- `element dequeue(struct queue *_head);` : `queue`의 `front` 원소를 삭제
- `void dump_queue(struct queue *_head);` : `queue`의 모든 원소들을 출력

위 함수들을 `queue.h` 헤더파일에 따로 선언해 주었다.

```c
#ifndef __QUEUE_H__
#define __QUEUE_H__

#include <stdbool.h>

typedef int element;

/* Node data structure which store element */
struct node {
	element data;
	struct node *next;
};

/* Queue data structure which connects nodes */
struct queue {
	struct node *front;
	struct node *rear;
};


/* Initialize queue */
void initialize(struct queue **_head);


/* Finalize queue */
void finalize(struct queue **_head);


/* Check the queue is empty */
bool is_empty(struct queue *_head);


/**
 * Check the queue is full
 * 
 * Because this queue is made by dynamic allocation,
 * there will be no full in queue data structure.
*/
// bool is_full(struct queue *_head);


/* Add the @item into the queue */
void enqueue(struct queue *_head, element _item);


/* Delete the item of the queue */
element dequeue(struct queue *_head);


/* Show data of the queue */
void dump_queue(struct queue *_head);

#endif
```

<br>

`initialize()` 함수에서는 `queue`을 동적할당 해주고, 멤버 변수들을 적절하게 초기화 시켜준다.

```c
void initialize(struct queue **_head)
{
	*_head = (struct queue*)malloc(sizeof(struct queue));
	(*_head)->front = NULL;
	(*_head)->rear = NULL;

	return;
}
```

<br>

`finalize()` 함수에서는 `queue`의 메모리를 해제시켜준다.

```c
void finalize(struct queue **_head)
{
	free(*_head);

	return;
}
```

<br>

`is_empty()` 함수에서는 `queue`가 비어있다면 `true`, 아니라면 `false`를 반환한다.

```c
bool is_empty(struct queue *_head)
{
	return !_head->front && !_head->rear;
}
```

<br>

`enqueue()` 함수에서는 `_item`이 담긴 노드를 삽입한다.

```c
void enqueue(struct queue *_head, element _item)
{
	struct node *new = (struct node*)malloc(sizeof(struct node));
	new->data = _item;
	new->next = NULL;

	if (is_empty(_head)) {
		_head->front = new;
	}
	else {
		_head->rear->next = new;
	}
	_head->rear = new;

	return;
}
```

<br>

`dequeue()` 함수에서는 먼저 `queue`가 비어있는지를 확인하고, 노드가 존재하면 `front` 노드를 삭제한 뒤 그 노드의 `data`를 반환한다.<br>
만약 `queue`가 비어있다면 프로그램을 강제종료한다.

```c
element dequeue(struct queue *_head)
{
	assert(!is_empty(_head));

	struct node *del = _head->front;
	element removed_item = del->data;
	_head->front = del->next;
	free(del);

	return removed_item;
}
```

<br>

`dump_queue()` 함수에서는 먼저 `queue`가 비어있는지를 확인하고, 원소가 존재하면 `queue` 안의 모든 노드들의 `data`를 출력한다.<br>
만약 `queue`가 비어있다면 프로그램을 강제종료한다.

```c
void dump_queue(struct queue *_head)
{
	assert(!is_empty(_head));

	struct node *temp = _head->front;
	int index = 1;

	printf("==============================\n");
	printf("Print the elements of the queue.\n");
  
	while (temp) {
		printf("[%2d] : %d\n", index, temp->data);

		temp = temp->next;
		index++;
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

#include "queue.h"

void initialize(struct queue **_head)
{
	*_head = (struct queue*)malloc(sizeof(struct queue));
	(*_head)->front = NULL;
	(*_head)->rear = NULL;

	return;
}


void finalize(struct queue **_head)
{
	free(*_head);

	return;
}


bool is_empty(struct queue *_head)
{
	return !_head->front && !_head->rear;
}


void enqueue(struct queue *_head, element _item)
{
	struct node *new = (struct node*)malloc(sizeof(struct node));
	new->data = _item;
	new->next = NULL;

	if (is_empty(_head)) {
		_head->front = new;
	}
	else {
		_head->rear->next = new;
	}
	_head->rear = new;

	return;
}


element dequeue(struct queue *_head)
{
	assert(!is_empty(_head));

	struct node *del = _head->front;
	element removed_item = del->data;
	_head->front = del->next;
	free(del);

	return removed_item;
}


void dump_queue(struct queue *_head)
{
	assert(!is_empty(_head));

	struct node *temp = _head->front;
	int index = 1;

	printf("==============================\n");
	printf("Print the elements of the queue.\n");
  
	while (temp) {
		printf("[%2d] : %d\n", index, temp->data);

		temp = temp->next;
		index++;
	}

	printf("==============================\n");

  return;
}
```

## 3. GitHub Link

[Queue source code](https://github.com/puter2712git/Queue){:target="_blank"}