---
layout: single
title: "[BOJ] 11866. 요세푸스 문제 0"
category: ["baekjoon", "silver"]
date: "2023-05-18 00:00:00 +0900"
last_modified_at: "2023-05-20 00:00:00 +0900"
---

## **_[11866. 요세푸스 문제 0](https://icpc.me/11866)_**

## 풀이

## 구조체 선언
```c
typedef struct _Node {
    int data;
    struct _Node* next;
} Node;

typedef struct _Queue {
    Node* front;
    Node* rear;
    int size;
} Queue;
```

* ### Node
    * #### data: 정보 저장
    * #### next: 다음 노드의 포인터
* ### Queue
    * #### front: 첫 번째 노드의 포인터
    * #### rear: 마지막 노드의 포인터
    * #### size: 노드들의 길이(배열의 길이)

## Queue 초기화 함수
```c
void init(Queue* q) {
    q->front = NULL;
    q->rear = NULL;
    q->size = 0;
}
```

## Node 생성 후 원형 큐로 작업
```c
void push(Queue* head, int data) {
    Node* tmpNode = (Node*)malloc(sizeof(Node));
    tmpNode->data = data;
    tmpNode->next = NULL;
    // 큐가 비어있을 때
    if (head->rear == NULL) {
        head->front = tmpNode;
        head->rear = tmpNode;
        // 원형 큐 구조
        tmpNode->next = tmpNode;
    }
    else {
        head->rear->next = tmpNode;
        head->rear = tmpNode;
        // 새 노드 삽입 시 원형 큐 구조 유지
        tmpNode->next = head->front;
    }
    head->size++;
}
```

## main 함수
```c
int main() {
    Queue josephus;     // 큐 생성
    init(&josephus);    // 초기화

    int n, k, t;
    scanf("%d %d", &n, &k);

    // 입력받은 n의 크기에 따라 원형 큐 생성
    // (1, 2, 3, ... , n)
    for (int i = 0; i < n; i++) {
        push(&josephus, (i + 1));
    }
    
    // 커서 선언
    Queue cur;

    // 커서.front = 원형 큐.rear;
    cur.front = josephus.rear;
    // 이래야 입력 받은 k에 따라 커서를 움직일 수 있음

    printf("<");

    while (n != 0) {
        // 임시변수 t
        t = k;
        while (t != 0) {
            // 입력한 k만큼 커서를 옮김
            cur.front = cur.front->next;

            // 값이 0이 아닐때만 t감소
            if (cur.front->data != 0)
                t--;
        }
```
## 출력 구문
```c
        // 출력 형식에 따름
        if (n != 1)
            printf("%d, ", cur.front->data);
        else
            printf("%d>", cur.front->data);
        // 해당하는 노드 data를 0으로 만들어서
        // 건너뛸 수 있도록 함
        cur.front->data = 0;
        n--;
```

## 전체 소스코드
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

typedef struct _Node {
    int data;
    struct _Node* next;
} Node;

typedef struct _Queue {
    Node* front;
    Node* rear;
    int size;
} Queue;

void init(Queue* q) {
    q->front = NULL;
    q->rear = NULL;
    q->size = 0;
}

void push(Queue* head, int data) {
    Node* tmpNode = (Node*)malloc(sizeof(Node));
    tmpNode->data = data;
    tmpNode->next = NULL;
    if (head->rear == NULL) {
        head->front = tmpNode;
        head->rear = tmpNode;
        tmpNode->next = tmpNode;
    }
    else {
        head->rear->next = tmpNode;
        head->rear = tmpNode;
        tmpNode->next = head->front;
    }
    head->size++;
}

int main() {
    Queue josephus;
    init(&josephus);

    int n, k, t;
    scanf("%d %d", &n, &k);

    for (int i = 0; i < n; i++) {
        push(&josephus, (i + 1));
    }
    Queue cur;
    cur.front = josephus.rear;

    printf("<");

    while (n != 0) {
        t = k;
        while (t != 0) {
            cur.front = cur.front->next;
            if (cur.front->data != 0)
                t--;
        }
        if (n != 1)
            printf("%d, ", cur.front->data);
        else
            printf("%d>", cur.front->data);
        cur.front->data = 0;
        n--;
    }
}
```