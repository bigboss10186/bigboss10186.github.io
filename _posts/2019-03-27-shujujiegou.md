---
layout:     post
title:      数据结构线性结构
subtitle:   数据结构
date:       2019-03-27
author:     BY 孟超
header-img: img/home-bg-o.jpg
catalog: 	 true
tags:
    - 数据结构
---

## 一、线性表

下面是我自己实现的代码，编译器通过了，结果也是对的，但是没准也会有错误存在

```c++
#include <stdio.h>
#include <stdlib.h>
#define Maxsize 10
//建立线性表

//将城市的各个参数进行数据封装
typedef struct city
{
    int number;
    char name[10];
}element;

//数据结构
typedef struct sqlsit
{
    element data[Maxsize];
    int length;
}sqlist;

//函数声明
void sqlist_init(sqlist *&L,element a[],int j);
int display(sqlist *L);
void sqlist_insert(sqlist*&L,element b,int n);
void sqlist_free(sqlist*&L);

int main()
{
    sqlist *L;
    element a[5] ={{1,"beijing"},{2,"shanghai"},{3,"shenzhen"},{4,"guangzhou"},{5,"changchun"}};
    sqlist_init(L,a,5);
    display(L);
    element b={6,"xi'an"};
    sqlist_insert(L,b,2);
    display(L);
    sqlist_free(L);
    return 0;
}

//建立线性表的代码
void sqlist_init(sqlist *&L,element a[],int j)
{
    int i=0;
   // L=(sqlist *)malloc(sizeof(element)+4);
   L=(sqlist *)malloc(sizeof(sqlist));
    for(i=0;i<j;i++)
    {
        L->data[i]=a[i];
    }
    L->length=j;
}

//打印线性表内容
int display(sqlist*L)
{
    if(L->length==0)
        return -1;
    else
        for(int i=0;i<L->length;i++)
        {
            printf("%d  ",L->data[i].number);
            printf("%s\n",L->data[i].name);
        }
        return 0;
}

//向线性表中插入元素
void sqlist_insert(sqlist*&L,element b,int n)//n是插在第几个位置(从0开始)，b是要插入的元素
{
    int j=L->length;
    for(int i=0;i<L->length-n+1;i++,j--)
    {
        L->data[j]= L->data[j-1];
    }
    L->data[n]=b;
    L->length= L->length+1;

}

//释放空间
void sqlist_free(sqlist*&L)
{
    free(L);
}
```

## 二、单链表

```c++
#include <stdio.h>
#include <stdlib.h>

//数据结构
typedef struct node
{
    int data;
    struct node *next;
}node;

void linklist_head_init(node *&L,int a[],int n);
void linklist_foot_init(node *&L,int a[],int n);
void linklist_destory(node *&L);
void linklist_display(node *&L);
int linklist_insert(node *&L,int i,int e);

int main()
{
    int a[10]={0,1,2,3,4,5,6,7,8,9};
    node *L;
    linklist_head_init(L,a,9);
    linklist_insert(L,2,12);
    //linklist_foot_init(L,a,10);
    linklist_display(L);
    linklist_destory(L);
    return 0;
}

//头插法建立单链表
void linklist_head_init(node *&L,int a[],int n)
{
    node *s;
    L=(node*)malloc(sizeof(node));//创建头结点
    L->next=NULL;
    for(int i=0;i<n;i++)
    {
        s=(node*)malloc(sizeof(node));
        s->data=a[i];
        s->next=L->next;
        L->next=s;
    }
}

//尾插法建立单链表
void linklist_foot_init(node *&L,int a[],int n)
{
    node *s,*r;                   //r为尾指针始终指向链表的末尾
    L=(node*)malloc(sizeof(node));//创建头结点
    L->next=NULL;
    r=L;
    for(int i=0;i<n;i++)
    {
        s=(node*)malloc(sizeof(node));
        s->data=a[i];
        r->next = s;
        r=s;
    }
    r->next =NULL;
}
//销毁单链表
void linklist_destory(node *&L)
{
    node *pre=L,*p=L->next;
    while(p!=NULL)
    {
        free(pre);
        pre=p;
        p=pre->next;
    }
    free(p);
}

//打印单链表中的内容
void linklist_display(node *&L)
{
    while(L->next!=NULL)
    {
        printf("%d\n",L->next->data);
        L=L->next;
    }
}

//插入元素,在单链表中找到第i-1个节点*p，若存在这样的节点，将e插在其后方
int linklist_insert(node *&L,int i,int e)
{
    int j=0;
    node *p=L,*s;
    while(j<i-1&&p!=NULL)
    {
        j++;
        p=p->next;
    }
    if(p==NULL)
    {
        return -1;
    }
    else
    {
        s=(node *)malloc(sizeof(node));
        s->data=e;
        s->next=p->next;
        p->next=s;
        return 0;
    }
}
```

## 三、栈

线性栈

```c++
#include <stdio.h>
#include <stdlib.h>
#define Maxsize 11

//数据结构
typedef struct
{
    int data[Maxsize];
    int top;    //记录栈顶
}sqstack;


//函数声明
void sqstack_init(sqstack *&s);
void sqstack_create(sqstack *&s,int a[],int n);
void sqstack_distory(sqstack *&s);
int sqstack_push(sqstack *&s,int &e);
int sqstack_pop(sqstack *&s,int &e);
void sqstack_display(sqstack *&s);


int main()
{
    int e=11;
    sqstack *s;
    int a[10]={1,2,3,4,5,6,7,8,9,10};

    sqstack_create(s,a,10);
    sqstack_push(s,e);
    sqstack_display(s);
    sqstack_distory(s);
    return 0;
}

//初始化栈
void sqstack_init(sqstack *&s)
{
    s=(sqstack*)malloc(sizeof(sqstack));
    s->top=-1;  //空栈
}

//创建一个栈
void sqstack_create(sqstack *&s,int a[],int n)
{
    s=(sqstack*)malloc(sizeof(sqstack));
    for(int i=0;i<n;i++)
    {
        s->data[i]=a[i];
    }
    s->top=n-1;
}

//销毁栈
void sqstack_distory(sqstack *&s)
{
    free(s);
}

//进栈
int sqstack_push(sqstack *&s,int &e)
{
    if(s->top==Maxsize -1)  //栈上溢出
        return -1;
    s->top++;
    s->data[s->top]=e;
    return 0;
}

//出栈
int sqstack_pop(sqstack *&s,int &e)
{
    if(s->top==-1)
        return -1;
    e=s->data[s->top];
    s->top--;
    return 0;
}

//打印
void sqstack_display(sqstack *&s)
{
    int i=0;
    while(s->top!=-1)
    {
        printf("%d\n",s->data[i]);
        s->top--;
        i++;
    }

}

```

## 链栈

**链栈中栈顶就是与头结点挨着的那个节点。**

```c++
#include <stdio.h>
#include <stdlib.h>

typedef struct listack
{
    int data;
    struct listack *next;
}listack;

//初始化栈
void sqstack_init(listack *&s)
{
    s=(listack*)malloc(sizeof(listack));
}

//销毁栈
void listack_destory( listack*&L)
{
    listack *pre=L,*p=L->next;
    while(p!=NULL)
    {
        free(pre);
        pre=p;
        p=pre->next;
    }
    free(p);
}

//创建一个栈
void listack_create( listack*&s,int a[],int n)
{
    s=(listack*)malloc(sizeof(listack));
    s->next=NULL;           //很必要
	listack *p;
    for(int i=0;i<n;i++)
    {
        p=(listack*)malloc(sizeof(listack));
        p->data=a[i];
        p->next=s->next;
        s->next=p;
    }
}

//进栈
int listack_push(listack *&s,int &e)
{
    listack *p;
    p=(listack*)malloc(sizeof(listack));
    p->data=e;
    p->next=s->next;
    s->next= p;
    return 0;
}

//出栈
int listack_pop(listack *&s,int &e)
{
    e=s->next->data;
    s->next=s->next->next;
    return 0;
}

//打印
void listack_display(listack *&s)
{
    while(s->next!=NULL)
    {
        printf("%d\n",s->next->data);//这里是这句话，不然会把头结点都会打印出来
        s=s->next;
    }

}

int main()
{
    listack *s;
    int a[10]={1,2,3,4,5,6,7,8,9,10};
    int e=10;
    listack_create(s,a,9);
    listack_push(s,e);
    listack_display(s);
    listack_destory(s);
    printf("hello world\n");
    return 0;
}


```
