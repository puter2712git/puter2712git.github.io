---
title: "List"
excerpt: "자료구조 List에 대한 포스트"

categories:
  - Data Structure
tags:
  - Computer Science
  - Data Structure

toc: true
toc_sticky: true

date: 2022-12-28
last_modified_at: 2022-12-28

use_math: true
---

## 1. List란?

데이터를 담은 `node`들끼리 서로 연결되어 있는 자료구조

각 `node`들은 양방향 연결되어 있을 수 있고, 단방향일 수도 있다.
- **Singly Linked List** : 단방향 연결되어 있는 리스트
- **Doubly Linked List** : 양방향 연결되어 있는 리스트

<br>

## 2. Doubly Linked List 구현 (C)

문자열을 담은 노드끼리 연결되어 있는 **List**를 구현할 것이다.

- `void initialize(struct node **_head);` : `_head` 노드를 초기화 (초기 실행)
- `void finalize(struct node **_head);` : 리스트의 노드 전체 해제 (마무리 단계에서 실행)
- `bool is_empty(struct node *_head);` : 리스트가 비어있는지 확인
- `void add_list_head(struct node *_head, char *_item);` : 리스트의 앞에 `_item` 원소를 지닌 노드를 추가
- `void add_list_tail(struct node *_head, char *_item);` : 리스트의 뒤에 `_item` 원소를 지닌 노드를 추가
- `void del_list_head(struct node *_head, char *_buffer);` : 리스트의 앞 노드를 삭제
- `void del_list_tail(struct node *_head, char *_buffer);` : 리스트의 뒤 노드를 삭제
- `void dump_list(struct node *_head);` : 리스트의 모든 노드들의 데이터를 출력

위 함수들을 `list.h` 헤더파일에 따로 선언해 주었다.

```c
#ifndef __LIST_H_
#define __LIST_H_

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <stdbool.h>
#include <assert.h>

#define MAX_BUFFER_SIZE 100

/* Node data structure of list which store string */
struct node {
	char *data;
	struct node *left;
	struct node *right;
};


/* Initialize node */
void initialize(struct node **_head);


/* Finalize node */
void finalize(struct node **_head);


/* Check the list is empty */
bool is_empty(struct node *_head);


/* Add the @_item into the head of the list */
void add_list_head(struct node *_head, char *_item);


/* Add the @_item into the tail of the list */
void add_list_tail(struct node *_head, char *_item);


/* Delete the data of the head of the list */
void del_list_head(struct node *_head, char *_buffer);


/* Delete the data of the tail of the list */
void del_list_tail(struct node *_tail, char *_buffer);


/* Show data of the list */
void dump_list(struct node *_head);

#endif
```

<br>

`initialize()` 함수에서는 head가 되는 `node`를 동적할당 해주고, 멤버 변수들을 적절하게 초기화 시켜준다.

```c
void initialize(struct node **_head)
{
	*_head = (struct node*)malloc(sizeof(struct node));
	(*_head)->data = NULL;
	(*_head)->left = NULL;
	(*_head)->right = NULL;

	return;
}
```

<br>

`finalize()` 함수에서는 list의 노드들의 메모리를 해제시켜준다.

```c
void finalize(struct node **_head)
{
	/* If the list is empty */
	if (is_empty(*_head)) {
		free(*_head);

		return;
	}

	struct node *temp = (*_head)->right;

	while (temp->data) {
		struct node *del = temp;
		temp = temp->right;

		free(del);
	}

	free(*_head);

	return;
}
```

<br>

`is_empty()` 함수에서는 `list`가 비어있다면 `true`, 아니라면 `false`를 반환한다.

```c
bool is_empty(struct node *_head)
{
	return !_head->left && !_head->right;
}
```

<br>

`add_list_head()` 함수에서는 리스트의 앞에 노드를 삽입한다.

```c
void add_list_head(struct node *_head, char *_item)
{
	struct node *new = (struct node*)malloc(sizeof(struct node));
	new->data = _item;

	/* If the list is empty */
	if (is_empty(_head)) {
		_head->right = new;
		_head->left = new;

		new->left = _head;
		new->right = _head;

		return;
	}
	
	new->left = _head;
	new->right = _head->right;

	_head->right = new;

	new->right->left = new;

	return;
}
```

<br>

`add_list_tail()` 함수에서는 리스트의 뒤에 노드를 삽입한다.

```c
void add_list_tail(struct node *_head, char *_item)
{
	struct node *new = (struct node*)malloc(sizeof(struct node));
	new->data = _item;

	/* If the list is empty */
	if (is_empty(_head)) {
		_head->right = new;
		_head->left = new;

		new->left = _head;
		new->right = _head;

		return;
	}

	new->right = _head;
	new->left = _head->left;

	_head->left = new;

	new->left->right = new;

	return;
}
```

<br>

`del_list_head()` 함수에서는 리스트 앞의 노드를 삭제한다.

```c
void del_list_head(struct node *_head, char *_buffer)
{
	assert(!is_empty(_head));

	struct node *del = _head->right;

	strcpy(_buffer, del->data);

	_head->right = del->right;
	del->right->left = _head;

	free(del);

	return;
}
```

<br>

`del_list_tail()` 함수에서는 리스트 뒤의 노드를 삭제한다.

```c
void del_list_tail(struct node *_head, char *_buffer)
{
	assert(!is_empty(_head));

	struct node *del = _head->left;

	strcpy(_buffer, del->data);

	_head->left = del->left;
	del->left->right = _head;

	free(del);

	return;
}
```

<br>

`dump_list()` 함수에서는 리스트의 모든 노드들의 데이터를 출력한다.

```c
void dump_list(struct node *_head)
{
	assert(!is_empty(_head));

	struct node *temp = _head->right;
	int index = 1;

	printf("==============================\n");
	printf("Print the data of the list.\n");
  
	while (temp->data) {
		printf("[%2d] : %s\n", index, temp->data);

		temp = temp->right;
		index++;
	}

	printf("==============================\n");

  return;
}
```

<br>

전체 코드는 다음과 같다.

```c
#include "list.h"

void initialize(struct node **_head)
{
	*_head = (struct node*)malloc(sizeof(struct node));
	(*_head)->data = NULL;
	(*_head)->left = NULL;
	(*_head)->right = NULL;

	return;
}


void finalize(struct node **_head)
{
	/* If the list is empty */
	if (is_empty(*_head)) {
		free(*_head);

		return;
	}

	struct node *temp = (*_head)->right;

	while (temp->data) {
		struct node *del = temp;
		temp = temp->right;

		free(del);
	}

	free(*_head);

	return;
}


bool is_empty(struct node *_head)
{
	return !_head->left && !_head->right;
}


void add_list_head(struct node *_head, char *_item)
{
	struct node *new = (struct node*)malloc(sizeof(struct node));
	new->data = _item;

	/* If the list is empty */
	if (is_empty(_head)) {
		_head->right = new;
		_head->left = new;

		new->left = _head;
		new->right = _head;

		return;
	}
	
	new->left = _head;
	new->right = _head->right;

	_head->right = new;

	new->right->left = new;

	return;
}


void add_list_tail(struct node *_head, char *_item)
{
	struct node *new = (struct node*)malloc(sizeof(struct node));
	new->data = _item;

	/* If the list is empty */
	if (is_empty(_head)) {
		_head->right = new;
		_head->left = new;

		new->left = _head;
		new->right = _head;

		return;
	}

	new->right = _head;
	new->left = _head->left;

	_head->left = new;

	new->left->right = new;

	return;
}


void del_list_head(struct node *_head, char *_buffer)
{
	assert(!is_empty(_head));

	struct node *del = _head->right;

	strcpy(_buffer, del->data);

	_head->right = del->right;
	del->right->left = _head;

	free(del);

	return;
}


void del_list_tail(struct node *_head, char *_buffer)
{
	assert(!is_empty(_head));

	struct node *del = _head->left;

	strcpy(_buffer, del->data);

	_head->left = del->left;
	del->left->right = _head;

	free(del);

	return;
}


void dump_list(struct node *_head)
{
	assert(!is_empty(_head));

	struct node *temp = _head->right;
	int index = 1;

	printf("==============================\n");
	printf("Print the data of the list.\n");
  
	while (temp->data) {
		printf("[%2d] : %s\n", index, temp->data);

		temp = temp->right;
		index++;
	}

	printf("==============================\n");

  return;
}
```

## 3. GitHub Link

[Link source code](https://github.com/puter2712git/List){:target="_blank"}