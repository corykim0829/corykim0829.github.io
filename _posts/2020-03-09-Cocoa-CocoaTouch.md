---
layout: post
title: "[iOS] Cocoa, Cocoa Touch"
header:
  teaser: /assets/images/ios-posts2.jpeg
  overlay_image: /assets/images/ios-posts2.jpeg
  overlay_filter: 0.2
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
categories:
  - iOS
tags:
  - Cocoa
  - Cocoa Touch
date: 2020-03-09 12:00:00
lastmod: 2020-03-09 12:00:00
sitemap :
  changefreq : daily
  priority : 1.0
---

Apple의 Cocoa 그리고 Cocoa Touch에 대해서 알아보자

<br>

#### 들어가기 전에

> Framework

라이브러리는 실행 가능한 코드라고 한다면, 프레임워크는 공유 라이브러리와 헤더 및 다른 리소스의 하위 디렉토리를 포함하는 번들(디렉토리 구조)입니다.

<br>

## Cocoa

Cocoa는 OS X 운영체제, 그리고 iPhone, iPad, iPod touch와 같은 멀티터치를 사용하는 운영체제인 iOS를 위한 **어플리케이션 환경**이다. Cocoa에는 객체 지향 소프트웨어 라이브러리, 런타임 시스템 및 통합 개발 환경으로 구성되어있다.

'Cocoa'라는 단어는 Objective-C 런타임을 기반으로하고, NSObject를 상속받는 모든 클래스 또는 객체를 가리킬 때 사용한다.

Cocoa라는 이름은 Cocoa가 만들어지기 몇년 전에 Apple이 어린이를 위한 멀티미디어 프로젝트 디자인 어플리케이션의 이름으로 시작되었다. Apple의 Cocoa를 개발하기 위해 고용된 Peter Jensen이 만든 이 이름은, 웹 페이지에서 "어린이를 위한 자바(Java for kids)"를 위해서 의도되었다고 한다.

<br>

## Cocoa Touch

iPhone, iPad 및 iPod Touch용 API이다. 다른 말로 Cocoa Touch 계층이라고도 한다. 그래픽 UI를 구현하는, 이벤트-구동(event-driven) 기법을 쓰는 iOS용 응용 소프트웨어는 보통 코코아 터치 계층에 기반하여 작성된다. 또한, 사용자 전화번호부(user contacts)와 같은 기기의 핵심 기능을 접근하기 위해서는 Cocoa Touch Layer를 이용하여야 한다. Cocoa Touch는 iOS의 소프트웨어 계층 중 가장 상위 계층이다. 

<br>

## iOS frameworks 계층 구조

iOS 을 그림으로 본다면 다음과 같다.

<br>

<p align="center"><img src="https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/Art/architecture_stack.jpg"></p>
<p align="center"><font color="#777777" weight="bold">iOS 구조에서의 Cocoa</font></p>

보통 궁극적으로 UIKit를 지원하는 iOS의 시스템 라이브러리나 프레임워크는 OS X의 라이브러리와 프레임워크의 서브셋이다. 예를 들면, iOS에는 Carbon 어플리케이션 환경이 없고, command-line access가 없고, 출력 프레임워크와 서비스가 없고, QuickTime은 플랫폼에 없다. 하지만 iOS에서 지원하는 기기의 특성상, iOS 전용의 public 및 private framework가 있다.

다음은 iOS 각 계층의 프레임워크의 요약이다.

> Core OS

이 수준에는 커널, 파일 시스템, 네트워킹 인프라, 보안, 전원 관리 및 여러 장치 드라이버가 포함된다. 또한 POSIX/BSD 4.4/C99 API 스펙을 지원하고 많은 서비스를 위한 시스템 레벨 API를 포함하는 libSystem 라이브러리도 있다.

> Core Service

이 계층의 프레임워크는 문자열 조작(string manipulation), 컬렉션 관리, 네트워킹, URL 유틸리티, 연락처 관리, 설정과 같은 **핵심 서비스**를 제공한다. 또한 기기의 하드웨어 기능을 기반으로 서비스를 제공하는데, GPS, 나침반, 가속도계, 자이로스코프 같은 것들이 있다. 이 계층의 프레임워크의 예시로는 Core Location, Core Motion, 그리고 System Configuration이 있다.

이 계층은 String이나 collection과 같은 공통의 데이터 타입을 위한 추상화를 제공하는 프레임 워크인, **Foundation**과 **Core Foundation**을 포함한다. Core Framework 계층은 또한 Core Data를 포함하며, 객체 그래프 관리와 객체 지속성을 포함한다.

> Media

이 계층의 프레임워크들과 서비스는 Core Services 계층에 의존하고 그래픽과 멀티미디어 서비스를 Cocoa Touch 계층에 제공한다. 여기에는 Core Graphics, Core Text, OpenGL ES, Core Animation, AVFoundation, Core Audio와 video playback와 같은 프레임워크가 포함되어있다. 

> **Cocoa Touch**

이 계층의 프레임워크들은 직접적으로 iOS 기반의 어플리케이션을 지원한다. Game Kit, Map Kit, iAd와 같은 프레임워크를 포함한다.

Cocoa Touch 계층과 Core Services 계층은 각 iOS 어플리케이션 개발에 특별히 중요한 **Objective-C 프레임워크**를 가지고 있다. 이들은 다음의 iOS의 core Cocoa 프레임워크이다. 

> **UIKit**

이 프레임워크는 어플리케이션이 사용자 인터페이스에 표시하는 객체를 제공하고 이벤트 처리 및 그리기를 포함하여 어플리케이션 동작의 구조를 정의한다. 

> **Foundation**

이 프레임워크는 객체의 기본 동작을 정의하고 관리를 위한 메커니즘을 설정하며 기본 데이터 타입, 컬렉션과 운영체제 서비스를 위한 객체를 제공한다. Foundation은 기본적으로 Core Foundation 프레임워크의 객체 지향 버전이다.

<br>

#### The Original Document

- [Cocoa Fundamentals Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/CocoaFundamentals/WhatIsCocoa/WhatIsCocoa.html)