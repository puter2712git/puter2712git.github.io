---
title: "Linked List (C)"
permalink: "/data-structure/linked-list"

categories:
  - Data Structure
tags:
  - [Linked List, C]

toc: true
toc_sticky: true

date: 2024-01-03
last_modified_at: 2024-01-03
---

## Linked List

**Linked List** 자료구조는 데이터와 다른 노드를 가리키는 포인터를 가지고 있는 **Node**들끼리 연결되어 있는 구조입니다. <br>
**Linked List**에는 노드의 포인터가 연결되는 방식을 통해 여러 종류로 구현이 가능합니다.

아래에서는 **Circular Linked List**를 C로 구현하였습니다. <br>
이전에 구현했던 **Queue**와 구조가 비슷한 걸 볼 수 있는데, 이는 연결 리스트를 이용하여 **Stack**과 **Queue**를 사용할 수 있음을 알 수 있습니다.

## 구현 (C)

```c
typedef char* element;

typedef struct node {
	element data;
	struct node *prev;
	struct node *next;
} Node;
```

연결 리스트에 담을 데이터 자료형은 `char*`이고, `node` 구조체에는 `data`와 구조체 `node`를 가리키는 포인터를 양옆으로 만듭니다.

이후, **Linked List**의 기본 함수들을 정의합니다.

```c
void push_front(element item)
{
	Node *new = (Node*)malloc(sizeof(Node));
	new->data = (element)malloc(strlen(item) + 1);
	strcpy(new->data, item);

	if (list->next == NULL) {
		list->next = new;
		list->prev = new;

		new->next = list;
		new->prev = list;
	} else {
		new->next = list->next;
		new->prev = list;

		list->next->prev = new;
		list->next = new;
	}
}

void push_back(element item)
{
	Node *new = (Node*)malloc(sizeof(Node));
	new->data = (element)malloc(strlen(item) + 1);
	strcpy(new->data, item);

	if (list->prev == NULL) {
		list->prev = new;
		list->next = new;

		new->prev = list;
		new->next = list;
	} else {
		new->prev = list->prev;
		new->next = list;

		list->prev->next = new;
		list->prev = new;
	}
}

element pop_front()
{
	Node *del = list->next;
	element ret = del->data;

	del->prev->next = del->next;
	del->next->prev = del->prev;

	free(del);
	return ret;
}

element pop_back()
{
	Node *del = list->prev;
	element ret = del->data;

	del->next->prev = del->prev;
	del->prev->next = del->next;

	free(del);
	return ret;
}
```

- `push_front()`

  - 리스트의 앞에 새로운 노드를 만들어 삽입합니다.

- `push_back()`

  - 리스트의 뒤에 새로운 노드를 만들어 삽입합니다.

- `pop_front()`

  - 리스트의 앞에 있는 노드를 삭제합니다.

- `pop_back()`
  - 리스트의 뒤에 있는 노드를 삭제합니다.
