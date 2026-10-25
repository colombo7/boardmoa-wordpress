# 게시판모아(BoardMoa) 앱 연동 — WordPress 플러그인

스마트폰 게시판 앱 **게시판모아**가 WordPress 사이트의 글·페이지와 KBoard·망보드·bbPress 게시판을 읽고 쓸 수 있게 하는 플러그인입니다.

- 최신 버전: **1.0.2 (시험판)** · 내려받기: [releases/appapi-wp-1.0.2.zip](releases/appapi-wp-1.0.2.zip) · [latest.json](latest.json)
- 설치한 뒤에는 관리 화면(설정 → 게시판모아 앱 연동)의 [업데이트 확인]으로 새 버전을 받습니다.
- XE 1.11·Rhymix 사이트용 모듈은 [boardmoa-appapi](https://github.com/colombo7/boardmoa-appapi), XE3 사이트용은 [boardmoa-appapi-xe3](https://github.com/colombo7/boardmoa-appapi-xe3) 에 있습니다.


> **시험판입니다.** WordPress 연동은 아직 시험 중이라 일부 기능만 됩니다.

플러그인 하나로 사이트에 깔린 게시판을 모두 앱에 연결합니다.

| 게시판 | 읽기 | 글쓰기·고치기·지우기 | 댓글 | 그 밖 |
|---|---|---|---|---|
| 워드프레스 글(분류·글 페이지)·페이지 | ✓ (비공개 글·비밀번호 글 포함) | ✓ 글쓴이 이상(기여자는 검토 대기), 사진 | ✓ (회원·손님 이름+메일) | |
| KBoard (+ KBoard 댓글) | ✓ (비밀글·'내 글만 보기'·역할별 권한, 답글, 계층형 분류) | ✓ 회원·손님(비밀번호), 사진('사진' 칸) | ✓ 회원·손님(비밀번호) | 좋아요·싫어요, 첨부 내려받기 |
| bbPress | ✓ (공개·비공개·숨김 포럼) | ✓ 토픽 쓰기·고치기, 관리자 지우기 | ✓ 답글·답글의 답글 | |
| 망보드 | ✓ (레벨 권한·비밀글·비밀번호, 답글) | ✓ 회원·손님(비밀번호) | ✓ 회원·손님(비밀번호) | 추천·비추천 |

웹에서 하는 것: 회원가입(앱이 웹 가입 화면을 연다), 메일 변경, 프로필 사진, KBoard·망보드 그림 글자 CAPTCHA(구글 reCAPTCHA v2 는 앱에서 푼다), 망보드 사진 첨부, 글의 답글 쓰기.
워드프레스에 없는 쪽지·친구·알림은 앱에서 보이지 않습니다.

XE 1.11·Rhymix 사이트는 [boardmoa-appapi](https://github.com/colombo7/boardmoa-appapi), XE3 사이트는 [boardmoa-appapi-xe3](https://github.com/colombo7/boardmoa-appapi-xe3)를 설치합니다.

## 필요한 것
- WordPress 6.0 이상 (7.1.3 에서 시험), PHP 7.4 이상
- 게시판 플러그인은 깔린 것만 씁니다: KBoard 6.7, KBoard 댓글, 망보드 2.4, bbPress 2.6 에서 시험. 함께 시험한 플러그인 20여 개는 아래 '다른 플러그인과 함께'

## 설치
1. `appapi-wp-<버전>.zip` 을 받습니다.
2. 관리자 화면 → 플러그인 → 새로 추가 → **플러그인 업로드** 로 zip 을 올리고 켭니다.
   (또는 압축을 풀어 `appapi` 폴더째 `wp-content/plugins/` 에 올린 뒤 플러그인 화면에서 켭니다. → `wp-content/plugins/appapi/appapi.php`)
3. 확인: 브라우저에서 `https://<사이트>/wp-content/plugins/appapi/api.php?r=site` 를 열어 `"ok":true` 와 `"engine":"wordpress"` 가 보이면 됩니다.
   그 뒤 앱에서 사이트 주소를 추가하면 됩니다.
   - 보안 플러그인이 `wp-content/plugins` 안의 PHP 를 직접 여는 것을 막으면 앱이 연결되지 않습니다. 이 파일만 허용해 주세요.

## 메뉴와 권한
- 앱 메뉴는 사이트 메뉴(외모 → 메뉴의 머리 메뉴, 블록 테마는 내비게이션)를 따릅니다. 메뉴의 분류는 게시판, KBoard·망보드 숏코드가 든 페이지는 그 게시판, 포럼은 포럼 게시판이 됩니다.
  메뉴에 없는 KBoard·망보드 게시판과 포럼은 메뉴 끝 '그 밖의 게시판' 묶음에 따로 나옵니다.
- 볼 수 있는지는 워드프레스 권한과 각 게시판 플러그인의 설정을 그대로 따릅니다(웹 화면과 같은 판단 함수). 볼 권한이 없는 게시판·글은 앱의 메뉴·새 글·검색 어디에도 나오지 않습니다.
- 앱 로그인은 웹 로그인과 같은 `wp_authenticate`(보안 플러그인의 로그인 제한·2단계 인증 포함)를 거치고, 앱 전용 토큰을 줍니다. 비밀번호를 바꾸면 앱도 다시 로그인해야 합니다.

## 다른 플러그인과 함께
- **회원 전용 사이트**(Force Login, Members '사이트 비공개', Ultimate Member '로그인한 회원만', Restricted Site Access)·**Elementor 점검 모드**: 웹과 같이 비회원에게는 앱에서도 로그인을 먼저 권합니다.
- **회원 등급으로 글 가리기**(Members, Ultimate Member, Paid Memberships Pro): 웹과 같은 안내 문구가 보이고 댓글은 보이지 않습니다.
- **로그인 CAPTCHA**(CAPTCHA 4WP 의 reCAPTCHA v2 체크박스·Turnstile): 앱에서 풉니다. 보이지 않는 reCAPTCHA·v3 는 앱에서 풀 수 없습니다.
- **2단계 인증**: Wordfence 는 비밀번호 칸에 비밀번호와 6자리 코드를 이어서 넣습니다. Two Factor·WP 2FA·Really Simple Security 는 프로필 화면의 **애플리케이션 비밀번호**를 만들어 비밀번호 대신 넣습니다(애플리케이션 비밀번호를 지워도 이미 로그인한 앱은 계속 됩니다. 끊으려면 비밀번호를 바꿉니다).
- **본문 속 양식**(Contact Form 7 등)은 앱에서 웹 화면 링크로 보입니다. **WP-Polls** 설문은 앱에서 투표할 수 있습니다. **Polylang** 사이트는 앱 화면 언어(한국어·영어)가 사이트 언어에 있으면 그 언어로 보입니다.
- 페이지 캐시(WP Super Cache, W3 Total Cache, LiteSpeed Cache, Autoptimize)는 앱 응답을 저장하지 않습니다. 서버의 nginx·프록시 캐시도 `Cache-Control: no-store, private` 를 지키게 두세요.

## 업데이트·지우기
- 업데이트: 설정 → 게시판모아 의 **[업데이트 확인]** → **[업데이트]**. 공개 저장소에서 받아 전자서명을 확인하고, 지금 파일을 `wp-content/uploads/boardmoa-backup/` 에 백업한 뒤 바꿉니다.
- 지우기: 플러그인을 끄고 삭제합니다. 플러그인이 만든 표 `wp_appapi_tokens`(앱 로그인)·`wp_appapi_ids`(번호 대응)는 남으니 필요하면 지웁니다.

## 문의
easyapplabs@gmail.com
