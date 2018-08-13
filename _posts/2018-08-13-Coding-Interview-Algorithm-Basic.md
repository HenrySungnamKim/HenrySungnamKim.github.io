---
layout: post
title:  "Coding Interview 2.Algorithm Basic Concepts"
date:   2018-08-13
excerpt: "코딩인터뷰 2장 알고리즘 기본 개념"
tag:
- 코딩 인터뷰
- 알고리즘 기본 개념
- 내용 정리
comments: true
---
* auto-gen TOC:
{:toc}

# 알고리즘 기초 개념

> 도서 코딩 인터뷰 퀘스천을 읽고 정리한 내용입니다.

## 알고리즘이란?

주어진 문제를 처리하는 단계별 지침. 시간과 공간 사용의 효율성을 따진다.

## 성장률. Rate Of Growth

입력 함수에 따라 실행 시간이 증가하는 비율.

Time Complexity | Name | Example |
1 | Constant | Linked list 앞에 원소를 삽입할 때  |
logn | Logrithmic | Sorted Array의 원소를 찾을 때 |

n | Linear | Unsorted Array의 원소를 찾을 때 |

nlogn | Linear Logrithmic | n개의 item을 'divide-and-conquer' Mergesort로 정렬할 때

n^2 | Quadratic | 그래프의 두개의 노드 중 가장 빠른 길

n^3 | Cubic | Matrix Multiplication

2^n | Exponential | 하노이의 두개의 탑

## 알고리즘 분석

알고리즘이 어떤 입력에서 시간이 적게 걸리고(Best Case) 많이 걸리는 지(Worst Case)를 알아야 한다. 3가지의 분석 유형이 있다.

- Worst Case
  - 알고리즘이 엄청난 시간을 소요하게 하는 입력을 정의한다
  - 입력은 알고리즘을 느리게 동작하게 하는 것으로 선택한다
- Average Case
  - 알고리즘의 실행 시간에 대한 예측을 제공한다
  - 입력은 무작위로 가정한다
- Best Case
  - 알고리즘이 최단 시간을 소요하게 하는 입력을 정의한다
  - 입력은 알고리즘을 빠르게 동작하는 것으로 선택한다.

## 점근적 표기. Asymptotic Notation

세가지 분석 유형 모두 상한(Upper bounds), 하한(Lower bounds)을 정의해야 한다.

- Big-O 표기법 (Big-O Notation). 상한계(g(n)) 제공

  `f(n)=O(g(n))`으로 표기한다. 만약 f(n)이 n^4 + 100n^2+ 10n +30 이라면, g(n)은  n^4이 된다. n이 충분히 큰 수 일 때 g(n)은 f(n)에 대한 최대 성장률을 제공하게 된다.

- Omega-Ω 빅 오메가 표기법. 하한계(g(n)) 제공

  f(n) = Ω(g(n)으로 표기한다. 만약 f(n)이 n^4 + 100n^2+ 10n +30이라면, g(n)=Ω(g(n^2))이 된다.

- Theta-𝛉 빅 세타 표기법. 상한계, 하한계가 동일한지 아닌지 결정한다.
