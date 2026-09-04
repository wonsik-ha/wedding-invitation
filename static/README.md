# 정적 청첩장 빌드

`invitation.conf`의 예식 정보를 HTML/CSS/JavaScript 정적 파일로 변환합니다. 서버나 데이터베이스는 필요하지 않습니다.

## 빌드와 미리보기

```bash
cp invitation.conf.example invitation.conf
./build.sh
./startup.sh
```

기본 주소는 `http://localhost:8080`이며 결과물은 `dist/`에 생성됩니다.

```bash
./build.sh                       # invitation.conf -> dist/
./build.sh -c other.conf         # 다른 설정 파일 사용
./build.sh -o /tmp/invitation    # 다른 출력 경로 사용
./startup.sh 3000                # 미리보기 포트 지정
```

## 생성되는 페이지

- `index.html`, `main.html`: 기본 청첩장
- `developer.html`: 개발자 콘셉트 청첩장

기본 페이지의 `WEDDING INVITATION` 문구를 연속으로 다섯 번 누르면 `developer.html`로 이동합니다. release, terminal, 방명록 페이지는 빌드하지 않습니다.

## 설정

`invitation.conf.example`을 복사해 만든 `invitation.conf`에서 이름, 예식 일시, 장소, 교통, 사진, 계좌 및 클라이언트 키를 설정합니다. 이 파일은 개인정보를 포함하므로 Git에 커밋되지 않습니다.

`WEDDING_AT`은 시간대 오프셋을 포함한 ISO 8601 형식이어야 합니다.

```ini
WEDDING_AT="2026-11-08T13:00:00+09:00"
```

사진 파일은 `src/photos/`에 넣고 설정 파일의 `PHOTO_*`, `GALLERY` 항목에 파일명을 지정합니다. 항목을 비우면 `TBU` 자리 표시가 나타나며, 설정되지 않은 데모 사진은 결과물에 복사되지 않습니다.

## 빌드 안전장치

- 필수 설정 및 날짜 형식 검사
- 치환되지 않은 템플릿 토큰 경고
- 생성된 JSON 유효성 검사
- 계좌번호 평문이 결과물에 남으면 빌드 실패
- 설정한 사진만 결과물에 복사

계좌번호 난독화는 검색 노출과 단순 수집을 줄이기 위한 것이며 암호화가 아닙니다. 브라우저가 표시할 수 있는 값은 사용자가 결국 복원할 수 있습니다.

## GitHub Pages

저장소 루트의 GitHub Actions workflow가 Secret `INVITATION_CONFIG`를 설정 파일로 복원하고 `dist/`를 Pages에 배포합니다. Kakao 및 NAVER 클라이언트 키의 도메인 등록 방법은 루트 [README](../README.md)에 정리되어 있습니다.
