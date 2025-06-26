---
title: "DI (Dependency Injection) 패턴 적용 사례"
excerpt: "Momentjs → Dayjs 마이그레이션"

categories:
  - Web
tags:
  - [Web, Boostcourse]

permalink: /blog/web/di-pattern-case

toc: true
toc_sticky: true

date: 2025-06-26
last_modified_at: 2025-06-26
---

# 문제 상황

회사 프로젝트에서 moment 라이브러리로 사용 중이던 레거시 코드를 dayjs로 교체해야 하는 상황이 발생했었습니다.

<br/>

## 왜 교체해야 했는가 ?

1. 번들 크키 문제

- Moment.js는 gzipped 기준 약 18KB로 번들 크기가 커서, 웹 로딩 속도를 느리게 하고 사용자 경험을 떨어뜨릴 수 있었습니다.

2. 공식 지원 중단

- Moment.js는 더 이상 활발한 개발 및 업데이트 지원이 이루어지지 않는 Deprecated 상태였습니다.
- [Moment.js 공식 지원 중단 공지](https://momentjs.com/docs/#/-project-status/)

위와 같은 이유로 장기적인 관점에서 유지보수 비용을 증가시키고, 최적화된 성능을 기대하기 어렵게 만든다고 생각했습니다.

## day.js 로 교체를 진행하면서 발생한 문제

기존 레거시 코드에서는 Moment.js 가 여러 파일에 걸쳐 산발적으로 사용되고 있었습니다. 기능 변경이나 수정 시, Moment.js 를 사용하는 모든 파일을 일일이 찾아 수정해야 하는 비효율적인 작업이 반복되고 있었습니다. 라이브러리 교체나 버전 업그레이드 시, 광범위한 코드 수정이 필요하여 개발 효율성이 크게 떨어졌습니다.

# 해결 방안

DI (Dependency Injection) 패턴을 적용하여 라이브러리 교체 과정에서 발생하는 문제를 해결했습니다.
날짜 라이브러리를 한 곳에서만 정의하고 주입하는 구조로 통일하였습니다.

# DI 패턴 구현 방법

## DI 패턴은 무엇이고 어떻게 구현할까요 ?

DI 패턴이란 객체가 직접 사용할 의존성을 생성하지 않고, 외부에서 <mark>주입</mark> 받는 설계 방식입니다. <br/>
DI 패턴을 사용하면 객체 간의 결합도를 낮추고, 유연한 확장성을 제공할 수 있습니다. <br/>

<br/>

## 구현 방법

실제 코드를 보면서 구현 방법을 알아보겠습니다.

1. 인터페이스 정의

```typescript
export interface BaseImmutableDate {
  format(fmt: string): string;
}
```

쉬운 이해를 위해 메서드는 하나만 정의했습니다.
BaseImmutableDate 라는 공통 인터페이스를 정의합니다.
해당 인터페이스의 규칙을 따라 라이브러리별 클래스를 작성할 예정입니다.

2. 라이브러리별 클래스 작성

```typescript
import moment from "moment";

export class MomentImmutableDate implements BaseImmutableDate {
  private readonly date: moment.Moment;

  constructor(date: string | Date) {
    this.date = moment(date);
  }

  format(fmt: string): string {
    return this.date.format(fmt);
  }
}
```

```typescript
import dayjs, { Dayjs } from "dayjs";

export class DayjsImmutableDate implements BaseImmutableDate {
  private readonly date: Dayjs;

  constructor(date: string | Date) {
    this.date = dayjs(date);
  }

  format(fmt: string): string {
    return this.date.format(fmt);
  }
}
```

```typescript
import { format as fnsFormat } from "date-fns";
import { BaseImmutableDate } from "./baseImmutableDate";

export class FnsImmutableDate implements BaseImmutableDate {
  private readonly date: Date;

  constructor(date: string | Date) {
    this.date = new Date(date);
  }

  format(fmt: string): string {
    return fnsFormat(this.date, fmt);
  }
}
```

BaseImmutableDate 인터페이스를 구현한 라이브러리별 클래스를 작성했습니다.
사용자는 라이브러리별 클래스를 사용하여 날짜 포맷팅을 할 수 있습니다.
data-fns 라이브러리의 경우, 함수형 스타일을 지향하더라도 라이브러리의 문법척 차이를 내부적으로 처리하고, 외부에는 통일된 인터페이스를 제공 받을 수 있습니다.

3. 최종 DI 객체 구현 및 사용

만약 내가 사용하는 라이브러리가 dayjs 라면, 아래와 같이 사용할 수 있습니다.

```typescript
import { BaseImmutableDate } from "./baseImmutableDate";
import { DayjsImmutableDate } from "./dayjsImmutableDate";

export class ImmutableDate
  extends DayjsImmutableDate
  implements BaseImmutableDate {}
```

fns 라이브러리라면 아래처럼 사용 할 수 있겠죠

```typescript
import { BaseImmutableDate } from "./baseImmutableDate";
import { FnsImmutableDate } from "./fnsImmutableDate";

export class ImmutableDate
  extends FnsImmutableDate
  implements BaseImmutableDate {}
```

그럼 최종적으로는 ImmutableDate 클래스를 사용하면 되고, 내부적으로는 라이브러리별 클래스를 사용하여 날짜 포맷팅을 할 수 있습니다.

```typescript
import { ImmutableDate } from "./immutableDate";

const date1 = new ImmutableDate("2024-06-01T00:00:00Z");
const date2 = new ImmutableDate("2024-07-01T00:00:00Z");

console.log(date1.format("YYYY-MM-DD")); // 출력: 2024-06-01
console.log(date2.format("YYYY-MM-DD")); // 출력: 2024-07-01
```

## 그럼 왜 함수가 아니라 클래스로 날짜 래퍼를 만들었을까?

1. 불변성 (Immutable)

날짜 값을 한 번 할당하면 내부적으로 변경되지 않는 값을 사용할 수 있습니다. 클래스 인스턴스를 통해 내부 상태(date)를 private 으로 선언하여 외부에서 접근 할 수 없도록 합니다.

2.  interface/implements 패턴

인터페이스만 지키면, dayjs든 moment 등 어떤 라이브러리로도 손쉽게 교체 가능하고, 유연하고 확장 가능한 구조를 만들 수 있습니다. 여러 구현체를 한 타입으로 사용이 가능합니다.

3. 메서드 체이닝/메서드 관리에 용이

상태와 메서드를 한 인스턴스에 묶어서 관리하기 때문에 메서드 체이닝이 용이합니다.

```typescript
const date = new ImmutableDate("2024-06-01T00:00:00Z");
date.format("YYYY-MM-DD").add(1, "day").format("YYYY-MM-DD");
```

# 결론

## Moment.js 에서 Day.js 에서 교체하면서 얼마나 번들 크기가 감소했을까?

![Image](/assets/images/posts_img/web/2025-06-26.png)

18.32KB -> 2.88KB 로 16KB 정도가 감소했습니다.
84% 감소인데, LCP 기준 번들 크기 20KB 감소 시, LCP가 100~300ms 빨라질 수 있다고 한다.

- [위 실험을 봐도 번들 크기 감소 시, LCP가 100~300ms 빨라 지는 걸 볼 수 있습니다.](https://sophiabits.com/blog/css-in-js-to-tailwind-better-web-vitals)

## DI 패턴을 통한 유연한 라이브러리 관리가 가능해짐

날짜 라이브러리에 대한 직접적인 종속성을 제거하고 DI 패턴을 적용하여, 향후 다른 라이브러리로의 교체가 용이해지고 전반적인 코드 유지보수성이 향상되었습니다.
