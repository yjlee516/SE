# 블로그 실습 DB 명세

`public.posts` 테이블

| 컬럼 | 자료형 | 필수 여부 | 기본값·설명 |
| --- | --- | --- | --- |
| id | uuid | 필수 | gen_random_uuid() [기본키] |
| title | text | 필수 | 없음 |
| summary | text | 필수 | 없음 |
| content | text | 선택 | 없음 |
| category | text | 필수 | 없음 |
| primary_tag | text | 필수 | 없음 |
| secondary_tag | text | 필수 | 없음 |
| thumbnail_theme | text | 선택 | 'cyber' |
| author_name | text | 필수 | 없음 |
| author_avatar | text | 선택 | 없음 |
| author_id | uuid | 선택 | 없음 [auth.users(id) 참조] |
| read_time | text | 필수 | '5 min' |
| likes_count | integer | 필수 | 0 |
| comments_count | integer | 필수 | 0 |
| created_at | timestamptz | 필수 | now() |
| updated_at | timestamptz | 필수 | now() |

필수 여부는 DB의 NOT NULL 기준이다. 새 글의 본문과 로그인 사용자 ID는 아래 구현 조건에 따라 저장한다.

- .env.local의 Supabase에 연결한다. 테이블·컬럼·RLS 변경 및 시드 실행 금지.
- 홈은 실제 posts를 조회하고 id로 상세에 연결한다. 목 데이터로 대체하지 않는다.
- 상세는 해당 id의 content를 마크다운으로 표시한다. 빈 본문·없는 글·조회 오류를 구분한다.
- 작성은 로그인 사용자만 허용하고 author_id=user.id, content=입력 본문으로 저장한다. 기본값 없는 필수 컬럼도 채운다.
- 권한 오류는 알리고 공용 DB 정책을 임의로 바꾸지 않는다.
