---
title: "Profiling Tool & APM Tool"
last_modified_at: 2021-04-27T00:05
categories: 
  - java-performance-tuning-story
tags: 
  - 'Profiling Tool' 
  - '성능 측정' 
  - '자바 성능 튜닝 이야기' 
  - '튜닝' 
  - '프로파일링 툴'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[자바 성능 튜닝 이야기[ProgrammingInsight-이상민]](http://www.yes24.com/Product/Goods/11261731)

<br>


# Profiling Tool

**시스템의 성능이 느릴 때 가장 먼저 해야 하는 작업은 병목 지점을 파악하는 것**이다.

**Java 기반 시스템에 대해 응답 속도나 각종 데이터 측정 프로그램**은 많은데, **Application의 속도에 문제가 있을 경우 분석하기 위한 툴로 `Profiling Tool`이나 `APM Tool`을 사용**한다.


<br>

## APM Tool

**요즘 성능 측정에 많이 사용하는 Tool로는 `APM(Application Performance Monitoring, Management)`**이 있다.

국산 `APM`으로는 미소 정보 사의 `WebTune`, 케이와이즈 사의 `Pharos`가 있다.

외산 `APM`으로는 CA Wily 사의 `Introscope`, `Compuware`, `dynaTrace`라는 Tool이 있고, **이 Tool들은 운영용 Server를 진단 및 모니터링 하기 위해 사용**한다.

**이 Tool들 중 미소 정보 사의 [WebTune](http://www.misoinfo.co.kr/#/misoinfo/solutionPerformanceWebtune.do)은 개발 장비에서 사용하는 License가 무료**이다.

<br>

`APM Tool`을 `Profiling Tool`과 비교하면 **`APM Tool`은 운영 환경 용 Tool**이고,

**`Profiling Tool`은 개발자용 Tool**이다.

둘의 비교 특징은 아래 표와 같다.

|구분|특징|
|:--:|:--:|
|Profiling Tool|소스 레벨 분석을 위한 툴<br>애플리케이션 세부 응답 시간 까지 분석 가능<br>메모리 사용량을 객체, 클래스, 소스 라인 단위 분석 가능<br>APM 툴에 비해 가격이 저렴<br>사용자 수 기반으로 보통 가격 책정<br>자바 기반 클라이언트 프로그램 분석 가능|
|APM Tool|애플리케이션 장애 상황에 대한 모니터링 및 문제점 진단이 주 목적<br>서버의 사용자 수나 리소스에 대한 모니터링 가능<br>실시간 모니터링을 위한 툴<br>프로파일링 툴에 비해 가격이 비쌈<br>CPU 수를 기반으로 가격이 책정<br>자바 기반 클라이언트 프로그램 분석 불가능|
