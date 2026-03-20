# Tools - 자동화 스크립트

Python 기반 자동화 도구 모음입니다.

## 설치

```bash
cp ../.env.template ../.env
# .env 편집하여 API 키 입력
```

## 도구 목록

### competitor-review-analyzer
다나와 상품 리뷰 크롤링 + AI 분석
```bash
python tools/competitor-review-analyzer/scripts/danawa-reviews.py --pcode [상품코드]
```

### dashboard-data-pipeline
매출 데이터 수집 및 가공
```bash
python tools/dashboard-data-pipeline/scripts/pipeline.py
```

### notion-handler
Notion 페이지/DB 관리 (NOTION_TOKEN 필요)
```bash
python tools/notion-handler/scripts/notion_api.py
```

### transcript-organizer
녹음 텍스트 자동 정리 (회의록/강의/인터뷰)
- 템플릿: `templates/meeting-template.md`, `lecture-template.md`, `interview-template.md`
- 인코딩 자동 감지: `utils/encoding-detector.sh`
