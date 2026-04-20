# Index

위키 전체 페이지를 카테고리별로 정리한 목록입니다. Dataview가 자동으로 갱신합니다.

## Concepts (개념)

```dataview
TABLE tags AS "Tags", updated AS "Updated"
FROM "wiki/concepts"
SORT updated DESC
```

## Entities (엔티티)

```dataview
TABLE tags AS "Tags", updated AS "Updated"
FROM "wiki/entities"
SORT updated DESC
```

## Summaries (요약)

```dataview
TABLE sources AS "Sources", updated AS "Updated"
FROM "wiki/summaries"
SORT updated DESC
```

## Relationships (관계)

```dataview
TABLE tags AS "Tags", updated AS "Updated"
FROM "wiki/relationships"
SORT updated DESC
```

## 최근 활동

```dataview
TABLE type AS "Type", updated AS "Updated"
FROM "wiki"
SORT updated DESC
LIMIT 10
```
