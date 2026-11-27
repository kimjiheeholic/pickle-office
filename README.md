# 🏢 츄츄포포 × 오이지 합동 비서실

두 비서(츄츄포포 · 오이지)가 함께 쓰는 **오피스 맵 + 작업 현황판** 웹페이지입니다.
파일 하나(`index.html`)로 동작하고, Firebase Realtime Database 주소를 넣으면 두 기기가 **실시간으로 같은 비서실**을 봅니다.

- 미리보기: `preview.png` (모바일), `preview-desktop.png` (PC)
- 개인정보 없음: 캐릭터 이름과 샘플 데이터만 들어 있습니다. 본명·연락처·계정·일정 등은 넣지 않았습니다.

## 1. 구성

| 파일 | 설명 |
|---|---|
| `index.html` | 앱 전체 (HTML + CSS + JS, 외부 라이브러리 없음) |
| `office-clay-bg.png` / `.webp` | 클레이 디오라마 오피스 배경 (AI 생성) |
| `char-chuchupopo.png` | 츄츄포포 아바타 (청록/라임) |
| `char-oiji.png` | 오이지 아바타 (노랑/올리브) |
| `robots.txt` | 검색엔진 수집 차단 |

## 2. 되는 것

**🗺️ 오피스 맵**
- 클레이 디오라마 위 4개 방: 대회의실 · 작업실 · 자료실 · 라운지
- 방 핀을 누르면 내 캐릭터가 그 방으로 이동 + 방별 최근 대화·인원 표시
- 캐릭터를 누르면 말풍선 대사·상태(열일 중 / 회의 중 / 자료 찾는 중 / 휴식 중)·방 이동 설정
- `기본 / 클레이` 테마, `움직임 ON/OFF`(둥실둥실), 방 가동 수·참여 비서·진행 작업 배지

**💬 회의실**
- 방별 대화 필터, 두 캐릭터 말풍선 채팅, 보낸 사람 선택

**📋 작업실**
- 예정 / 진행 중 / 완료 3열 보드, 담당자(츄츄포포 · 오이지 · 공동)
- 작업 추가, 상태 전환, 삭제

**📊 비서현황**
- 캐릭터별 현재 방·상태·대사·작업 수, 전체 요약 타일, 최근 활동 피드

**📝 메모**
- 둘이 함께 보는 회의록(포스트잇), 작성자·시간 표시, 삭제

## 3. 바로 열어 보기

`index.html`을 더블클릭하면 됩니다. 이때는 **로컬 모드**로, 데이터가 그 브라우저(localStorage)에만 저장됩니다.
로컬 모드의 샘플 데이터는 첫 실행 때 자동으로 들어갑니다. 초기화하려면 브라우저 콘솔에서
`localStorage.removeItem('pickleOffice.state.v1')` 후 새로고침하세요.

## 4. 실시간 연결 (Firebase Realtime Database, 무료)

1. <https://console.firebase.google.com> → **프로젝트 추가** (Google Analytics는 꺼도 됩니다)
2. 왼쪽 **빌드 → Realtime Database → 데이터베이스 만들기** → 위치 선택 → **테스트 모드**로 시작
3. **규칙(Rules)** 탭에 아래를 붙여 넣고 **게시**

```json
{
  "rules": {
    "pickleOffice": {
      "$room": {
        ".read": true,
        ".write": true
      }
    }
  }
}
```

4. 데이터베이스 상단의 주소(`https://내-db-default-rtdb.firebaseio.com`)를 복사
5. 페이지 오른쪽 위 **로컬 모드** 배지를 눌러 **방 코드**(둘만 아는 긴 문자열)와 **DB 주소**를 넣고 **연결하기**
6. 상대 기기에서도 **같은 DB 주소 + 같은 방 코드**로 연결하면 끝

- 연결되면 배지가 초록색 **실시간 연결**로 바뀌고, 한쪽에서 작업·메모·대화·캐릭터 이동을 하면 다른 쪽에 바로 반영됩니다.
- 연결이 끊기면 자동으로 다시 붙고, 계속 안 되면 6초 간격 폴링으로 전환합니다.
- 무료(Spark) 한도: 저장 1GB, 다운로드 10GB/월, 동시 연결 100 — 이 정도 사용에는 충분합니다.

### 보안 주의

- 위 규칙은 **주소와 방 코드를 아는 사람은 읽고 쓸 수 있는** 공개 규칙입니다. 방 코드를 길게 지어 사실상 열쇠처럼 쓰세요.
- 이 페이지에는 비밀번호·연락처·계정·일정 같은 개인정보를 적지 마세요. 메모·작업에도 민감한 내용은 피하는 게 안전합니다.
- 나중에 규칙을 `auth != null` 등으로 조이려면 Firebase Authentication을 함께 붙여야 합니다.

## 5. GitHub Pages 배포

```bash
# 새 저장소를 만들었다면 (공개 저장소)
git init
git add index.html office-clay-bg.png office-clay-bg.webp char-chuchupopo.png char-oiji.png robots.txt
git commit -m "피클 합동 비서실"
git branch -M main
git remote add origin https://github.com/<아이디>/<저장소>.git
git push -u origin main
```

저장소 → **Settings → Pages → Source: Deploy from a branch → Branch: main / (root) → Save**
1~2분 뒤 `https://<아이디>.github.io/<저장소>/` 에서 열립니다.

> 무료 계정에서 Pages를 쓰려면 저장소가 **public**이어야 합니다. 주소를 아는 사람은 누구나 페이지를 볼 수 있으니(개인정보 없음), 검색 노출은 `robots.txt`와 `<meta name="robots" content="noindex">`로 막아 두었습니다.

## 6. 꾸미기

- 방 이름·설명·캐릭터 위치: `index.html` 안 `var ROOMS = {...}` (spots의 x/y는 맵 이미지 기준 %)
- 캐릭터 이름·이미지: `var CHARS = {...}`
- 기본 대사 추천: `var QUICK_LINES`
- 색감: `<style>` 맨 위 `:root` 변수
- 배경 이미지 교체: 같은 파일명으로 덮어쓰면 됩니다. (권장 1200×960, 5:4)
