# movie-lab-actions

웹서비스설계 과제: TMDB Vue 프로젝트 소스를 그대로 올리고, `main`에 push될 때마다 GitHub Actions가 `npm run build`를 실행해서 GitHub Pages로 자동 배포하는 레포입니다.

빌드 결과물(dist)은 이 레포에 커밋하지 않습니다. `.github/workflows/deploy.yml`이 push마다 빌드 → Pages 배포까지 처리합니다.

## 배포 주소

https://junyi11111.github.io/movie-lab-actions/

## 동작 방식

1. `main`에 push
2. `actions/setup-node` → `npm ci` → `npm run build` (vite.config.js의 `base: '/movie-lab-actions/'` 사용)
3. `actions/upload-pages-artifact`로 `dist`를 업로드
4. `actions/deploy-pages`로 Pages에 배포

## API 키

이 프로젝트는 TMDB API 키를 코드나 빌드 결과물에 포함하지 않습니다. 방문자가 사이트에 접속해서 직접 본인의 TMDB API Key(v3)를 입력하면 그 탭 메모리에만 저장되고, TMDB로만 전송됩니다.

워크플로의 `TMDB_API_KEY` 시크릿은 빌드 단계 환경변수로만 전달되고, `VITE_` 접두사가 아니라서 클라이언트 번들에는 절대 포함되지 않습니다 (Vite는 `VITE_` 로 시작하는 변수만 브라우저 코드에 노출함). 현재 앱 코드가 이 변수를 실제로 사용하진 않고, "키를 코드/시크릿으로 관리" 요구사항을 만족시키려고 추가한 것입니다.

레포 Settings → Secrets and variables → Actions 에서 `TMDB_API_KEY`라는 이름으로 값(본인 TMDB API Key 또는 임의 값)을 등록해야 워크플로가 참조 오류 없이 돌아갑니다.

## 로컬 실행

```sh
npm ci
npm run dev
```

## 체크포인트

- `checkpoints/01-key-form` : 키 입력 폼만 있는 시작 버전
- `checkpoints/02-popular` : 인기 영화 조회
- `checkpoints/03-search` : 검색 + 페이지네이션 (현재 `src/App.vue`로 사용 중인 완성본)

실습 원본 가이드는 `LAB_GUIDE_EN.md` 참고.
