# 하원식 · 김경민 모바일 청첩장

정적 HTML/CSS/JavaScript로 만든 GitHub Pages 청첩장입니다.

- 청첩장: <https://wonsik-ha.github.io/wedding-invitation/>
- 개발자 페이지: <https://wonsik-ha.github.io/wedding-invitation/developer.html>
- 기본 페이지의 `WEDDING INVITATION` 문구를 연속으로 다섯 번 눌러도 개발자 페이지가 열립니다.

## 로컬 실행

실제 개인정보가 담긴 `static/invitation.conf`는 Git에 포함되지 않습니다.

```bash
cd static
cp invitation.conf.example invitation.conf
./build.sh
./startup.sh
```

`http://localhost:8080`에서 결과를 확인할 수 있습니다. 빌드 결과는 `static/dist/`에 생성됩니다.

## 배포

`main` 브랜치에 push하면 GitHub Actions가 다음 순서로 배포합니다.

1. 저장소 Secret `INVITATION_CONFIG`를 `static/invitation.conf`로 복원
2. `static/build.sh` 실행
3. `static/dist/`를 GitHub Pages에 배포

사진을 추가하거나 예식 정보를 바꾸면 로컬 `static/invitation.conf`를 수정한 다음, 같은 전체 내용을 GitHub Secret `INVITATION_CONFIG`에도 다시 저장해야 합니다.

## 사진 추가

사진 파일은 `static/src/photos/`에 넣고 `static/invitation.conf`의 `PHOTO_MAIN`, `PHOTO_GROOM`, `PHOTO_BRIDE`, `GALLERY` 등에 파일명을 지정합니다. 실제 사진은 실수로 공개되지 않도록 기본적으로 `.gitignore` 대상입니다. GitHub Pages에 올려도 되는 사진인지 확인한 뒤 필요한 파일만 `git add -f static/src/photos/<파일명>`으로 명시적으로 추가해야 합니다.

현재 사진은 모두 `TBU`로 표시됩니다.

## 사용자가 직접 해야 하는 외부 콘솔 설정

커스텀 도메인과 DNS 설정은 사용하지 않습니다. 아래 두 서비스에 GitHub Pages의 origin만 등록하면 됩니다.

### Kakao Developers

1. Kakao Developers에서 해당 애플리케이션을 엽니다.
2. JavaScript SDK 도메인에 `https://wonsik-ha.github.io`를 등록합니다.
3. 카카오톡 공유 제품 설정의 웹 도메인에도 같은 값을 등록합니다.

JavaScript 키는 브라우저에서 사용되는 공개 식별자입니다. 서버 비밀키처럼 숨길 수는 없지만, 허용 도메인을 정확히 제한해야 합니다.

### NAVER Cloud Platform Maps

1. NAVER Cloud Platform 콘솔에서 해당 Maps 애플리케이션을 엽니다.
2. Web Dynamic Map 사용을 활성화합니다.
3. Web Service URL에 `https://wonsik-ha.github.io`를 등록합니다.

클라이언트 ID(`ncpKeyId`)만 브라우저에 사용하며 Client Secret은 정적 페이지에 넣지 않습니다.

## 보안 주의

- `static/invitation.conf`와 실제 사진은 커밋하지 않습니다.
- 계좌번호는 빌드 결과에서 난독화되지만 암호화된 비밀은 아닙니다.
- 제공된 클라이언트 키 값은 문서나 소스에 직접 기록하지 않습니다.

세부 빌드 구조는 [static/README.md](static/README.md)를 참고해 주세요.
