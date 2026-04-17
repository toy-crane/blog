---
title: "toycrane.xyz Personal Site"
date: 2026-04-09
tags:
  - dev/web
  - business/branding
  - productivity
aliases:
  - "개인 사이트"
  - "toycrane.xyz"
  - "Personal Blog Site"
description: "Notion 소개 페이지를 대체할 Astro 기반 미니멀 개인 사이트 기획"
---

# toycrane.xyz

## Problem Statement

Notion 소개 페이지를 벗어나, Product Maker로서의 정체성을 간결하고 세련되게 보여주는 개인 사이트가 필요하다.

## Recommended Direction

Astro + Vercel로 mitchellh.com 스타일의 미니멀 사이트를 빌드한다. 콘텐츠는 극도로 절제하고, 타이포그래피와 여백으로 퀄리티를 잡는다. 블로그 글은 새로 작성하며, Content Collections로 관리한다.

## Site Structure

```
/                  -> Home (2-3줄 소개 + 대표 프로젝트 링크)
/projects          -> Dearly, Go Parkgolf 각각 한 단락 소개
/writing           -> 새로 쓰는 글 목록 (Content Collections)
/writing/[slug]    -> 개별 글 페이지
```

Footer: 소셜 링크 (GitHub, Threads, X, LinkedIn)

## Key Assumptions to Validate

- [ ] toycrane.xyz DNS 설정이 Vercel에 연결 가능한지 확인
- [ ] Astro + MDX + React 컴포넌트 조합이 원하는 디자인을 표현할 수 있는지 -- 프로토타입으로 검증
- [ ] 글을 정기적으로 쓸 수 있는지 -- 첫 글 1개를 런칭과 함께 올려서 시작

## Tech Stack

- **Framework**: Astro (Content Collections, MDX)
- **Deploy**: Vercel
- **Domain**: toycrane.xyz
- **Styling**: TBD (Tailwind CSS 유력)
- **Dark Mode**: 기본 지원 (시스템 설정 연동 + 토글)

## MVP Scope (1-2주)

- Astro 프로젝트 세팅 + Vercel 배포
- 홈페이지 (소개 + 프로덕트 링크)
- /projects 페이지
- /writing 목록 + 개별 글 페이지 (Content Collections)
- 소셜 링크
- 다크모드 (시스템 설정 연동 + 수동 토글)
- 커스텀 도메인 연결 (toycrane.xyz)
- RSS 피드

## Not Doing (and Why)

- **Medium 글 이전** -- 새 글로 시작하는 게 더 깔끔. Medium은 그대로 둠
- **댓글 기능** -- 소셜에서 대화하면 됨. 사이트는 일방향 발행
- **애널리틱스** -- 런칭 후 트래픽 생기면 그때 추가
- **i18n** -- 한국어 단일 언어로 시작
- **검색 기능** -- 글이 쌓이면 그때 추가

## Design References

- [leerob.com](https://leerob.com/) -- 극강의 미니멀. 단일 컬럼, 장식 없음, 타이포그래피만으로 완성. **Primary reference**
- [mitchellh.com](https://mitchellh.com/) -- 콘텐츠 중심, 텍스트만으로 구성
- [flavienbonvin.com](https://flavienbonvin.com/) -- Astro 빌드 참고용. 마이그레이션 과정 공개

## Open Questions

- 첫 번째 글의 주제는?
