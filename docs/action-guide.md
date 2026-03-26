# 발표 행동 가이드

> 발표 당일 실제로 해야 할 행동을 시간순으로 정리한 문서

---

## 사전 준비 (발표 전날까지)

### 1. 계정 & CLI 설치 확인

#### Vercel

- [ ] vercel.com 가입 (GitHub 계정 연동 추천)
- [ ] Vercel CLI 설치
  ```bash
  npm i -g vercel
  ```
- [ ] 로그인 확인
  ```bash
  vercel login
  # 브라우저 팝업 → 승인 클릭
  ```
- [ ] 테스트 배포 한 번 해보기 (아무 Next.js 프로젝트로)

#### Netlify (백업용)

- [ ] netlify.com 가입 (GitHub 계정 연동)
- [ ] Netlify CLI 설치
  ```bash
  npm i -g netlify-cli
  ```
- [ ] 로그인 확인
  ```bash
  netlify login
  # 브라우저 팝업 → Authorize 클릭
  ```

#### Claude Code

- [ ] Claude Code CLI 설치 및 로그인 상태 확인
  ```bash
  claude --version
  ```

#### Stitch MCP

- [ ] Stitch에서 API 키 발급 (stitch.withgoogle.com → 프로필 → Stitch settings → API key → Create key)
- [ ] MCP 등록
  ```bash
  claude mcp add stitch --transport http https://stitch.googleapis.com/mcp \
    --header "X-Goog-Api-Key: 발급받은-API-KEY" -s user
  ```
- [ ] 연결 테스트: Claude Code에서 `stitch 디자인 목록 보여줘` 요청

#### 기타

- [ ] Node.js 18+ 설치 확인: `node -v`
- [ ] npm/pnpm 설치 확인
- [ ] 인터넷 연결 안정성 확인 (발표장 Wi-Fi 테스트)

---

### 2. 비상용 자료 준비

- [ ] Stitch에서 미리 디자인 하나 생성해두기 (MCP 실패 대비)
- [ ] 해당 디자인의 코드를 로컬에 `backup/` 폴더로 저장
- [ ] 배포 성공한 스크린샷 캡처해두기 (인터넷 끊김 대비)
- [ ] 완성된 프로젝트를 미리 Vercel에 한 번 배포해두기 (최악의 경우 이 URL 공유)

---

## 발표 당일 체크리스트

### 발표 30분 전

- [ ] 터미널 열기 — 폰트 크기 크게 (청중이 볼 수 있도록)
  - macOS 터미널: `Cmd + +` 여러 번
  - iTerm2: `Cmd + +` 또는 Preferences → Profiles → Text → Font Size → 18pt 이상
- [ ] 브라우저 열기 — 확대 150% (`Cmd + +`)
- [ ] 프로젝트 폴더 이동
  ```bash
  cd ~/Workspace/how-to-vibe
  ```
- [ ] 인터넷 연결 확인
  ```bash
  curl -s https://api.vercel.com/v2 | head -c 50
  ```
- [ ] Vercel 로그인 상태 확인
  ```bash
  vercel whoami
  ```
- [ ] Claude Code 실행 확인
  ```bash
  claude
  ```
- [ ] 불필요한 알림 끄기 (방해금지 모드)
  - macOS: 제어 센터 → 집중 모드 → 방해금지

---

## 시연 행동 순서

### DEMO 01 — Stitch MCP 디자인 가져오기 (0:10~0:20)

#### 순서대로 따라하기

**Step 1**: Stitch에서 디자인 생성 (미리 해두거나 라이브)

- stitch.withgoogle.com 접속
- "Vibe Design" 선택
- 자연어로 UI 설명 입력 (예: "팀 매칭 서비스 메인 페이지")

**Step 2**: Claude Code에서 디자인 가져오기

```
(Claude Code 프롬프트)
> stitch에서 방금 만든 홈 화면 디자인 코드 가져와줘
```

**Step 3**: Next.js 컴포넌트 변환

```
(Claude Code 프롬프트)
> 이걸 TypeScript + Tailwind CSS 기반 Next.js 컴포넌트로 바꿔줘
```

**Step 4**: 로컬 확인

```bash
npm run dev
# 브라우저에서 localhost:3000 열기
```

**핵심 멘트 (잊지 말기)**:

> "지금 제가 코드를 복붙한 게 아니라 AI가 API로 직접 가져왔습니다. 이게 MCP입니다."

#### MCP 연결 실패 시

1. 당황하지 말고: "네트워크 이슈가 있네요, 미리 준비한 걸로 보여드리겠습니다"
2. `backup/` 폴더의 코드를 Claude Code로 읽게 하기
3. 이후 변환 과정은 동일하게 진행

---

### DEMO 02 — Vercel 배포 (0:20~0:30)

#### 순서대로 따라하기

**Step 1**: 로컬에서 결과 확인

```bash
npm run dev
# 브라우저에서 확인 → "이게 지금 제 노트북에서만 돌아가는 겁니다"
```

**Step 2**: Vercel 배포 (이게 핵심!)

```bash
vercel
```

- 나오는 질문들:
  - `Set up and deploy?` → **Y** (엔터)
  - `Which scope?` → 본인 계정 선택 (엔터)
  - `Link to existing project?` → **N** (엔터)
  - `What's your project's name?` → 원하는 이름 입력 (예: `ddalgak-demo`)
  - `In which directory is your code located?` → **./** (엔터)
  - 프레임워크 자동 감지 → 엔터
- 약 30초 대기 → URL 출력

**Step 3**: URL 공유

```
✓ https://ddalgak-demo.vercel.app
```

- 이 URL을 청중에게 공유
- 핸드폰으로 직접 열어보기

**핵심 멘트 (잊지 말기)**:

> "서버 설정 없었고, 배포 버튼 없었고, 명령어 하나였습니다."

#### Vercel 배포 실패 시 → Netlify로 전환

```bash
# 빌드
npm run build

# Netlify 배포
netlify deploy --prod --dir=.next
# 또는 정적 빌드라면
netlify deploy --prod --dir=out
```

- 나오는 질문들:
  - `What would you like to do?` → **Create & configure a new site**
  - `Team` → 본인 팀 선택
  - `Site name` → 원하는 이름 입력
- 완료되면 URL 출력: `https://ddalgak-demo.netlify.app`

---

## Vercel 웹 대시보드 (참고용)

만약 CLI가 아닌 웹에서 배포해야 할 경우:

1. vercel.com 로그인
2. **"Add New..."** 버튼 → **"Project"**
3. **"Import Git Repository"** → GitHub 연동 → 레포 선택
4. **Framework Preset**: Next.js 자동 감지됨
5. **"Deploy"** 클릭
6. 1~2분 후 URL 생성

---

## Netlify 웹 대시보드 (참고용)

1. netlify.com 로그인
2. **"Add new site"** → **"Import an existing project"**
3. GitHub 연동 → 레포 선택
4. Build settings:
   - Build command: `npm run build`
   - Publish directory: `.next` (Next.js) 또는 `out` (정적)
5. **"Deploy site"** 클릭
6. 1~2분 후 URL 생성 (xxx.netlify.app)

---

## 비상 대응 매뉴얼

| 상황                  | 즉시 행동                                 | 멘트                                                                           |
| --------------------- | ----------------------------------------- | ------------------------------------------------------------------------------ |
| Stitch MCP 실패       | `backup/` 폴더 코드 사용                  | "네트워크 이슈가 있어서 미리 준비한 걸로 진행하겠습니다"                       |
| 빌드 에러             | 에러 메시지를 Claude Code에 붙여넣기      | "이것도 바이브 코딩입니다. 에러를 그대로 붙여넣으면 됩니다" (오히려 좋은 시연) |
| Vercel 배포 실패      | `netlify deploy --prod`                   | "플랜 B가 있습니다"                                                            |
| Netlify도 실패        | 미리 배포해둔 URL 공유                    | "라이브에선 이런 일도 있죠, 미리 배포해둔 결과입니다"                          |
| 인터넷 끊김           | `npm run dev`로 로컬 시연 + 배포 스크린샷 | "로컬에서 보여드리고, 배포는 이렇게 됩니다"                                    |
| Claude Code 응답 느림 | 말로 설명하며 기다리기                    | "AI도 생각하는 시간이 필요합니다 (웃음)"                                       |

---

## 발표 후 할 일

- [ ] 배포된 URL이 살아있는지 확인
- [ ] 리소스 QR코드/링크 공유 (claude.ai, lovable.dev, v0.app, stitch.withgoogle.com)
- [ ] 발표 자료 공유 (원하면 GitHub 레포 공개)
