---
title: "Hugo 로 github 블로그 시작하기 - 0. 개요"
date: 2019-05-31T11:06:16+09:00
categories:
  - Blogging
tags:
  - Blogging
  - Blog
  - 블로그
  - Hugo
  - Github
  - Github Pages
# thumbnailImagePosition: left
# thumbnailImage: //d1u9biwaxjngwg.cloudfront.net/highlighted-code-showcase/peak-140.jpg
# draft: true
---
Hosting Blog via Github Pages with Hugo on Mac, 맥에서 Hugo 로 Github Pages 에 블로그 호스팅하기
<!--toc-->
# 개요

## 뻘글

때로는 예상치 못한 곳에서 다른 분야의 지식을 사용하는 경우가 있다.

이번 블로그를 구성하고 준비하면서 가장 많이 느낀게 '이거 전에 안해봤으면 어쩔뻔했나..' 일 정도로  
뜬금없는 포인트에서 오류가 생겼고, 이전의 경험을 이용해 잠깐의 구글링만으로 상황을 모면한 경우가 많았다.  
(~~뽀록을 보기좋게 풀어 설명한 것임~~)

삽질은 많이 했어도 잘 해결하여 이 블로그도 만들고 이 글을 적고 있으니,  
가벼운 마음으로 적어나갈 예정이다.

## 내가 생각한 블로그의 조건

1. 마크다운 으로 작성해서 올릴 수 있어야함 (개발자의 문서는 역시 마크다운이지.)
2. 수정 내역을 볼 수 있어야 함 (문서 버전관리)
3. 위 두 과정을 효율적으로 (쉽고 빠르게) 수행할 수 있어야 함

## 블로그 구성 과정

1. 블로그 서비스/프레임워크 선택
2. Hugo 시작하기
3. 테마 선정 ~~삽질~~
4. Repository 생성 및 Subrepo 지정
5. github 호스팅
6. travis ci 연동 ~~삽질~~
7. Disqus 연동
8. Google analytics 연동 (& Google Search engine) - 진행중, 확인 필요
9. 이미지 호스팅 & CDN 주저리

위의 과정을 거치며 느낀 것은 github pages 를 운영중인 사람들이 존경스럽다?  
'블로그라면 이 정도는 기본이지' 했던 부분이 기본적으로 지원이 안돼서  
생각보다 손도 많이가고 삽질을 했던게 이유인 것 같다.

먼길을 달려온만큼 각 과정마다의 삽질 내역을 상세히 적을 것이므로  
여러 페이지에 나눠 작성하였다.

-----
다음은 블로그를 운영할 서비스/프레임워크 선택해볼 것이다.