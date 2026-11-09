# AI에게 부탁하는 말 예시

[English](conversations.md)

AI에게 이렇게 말하면 됩니다. 파일 경로와 숫자는 예시이니 내 파일에 맞게 바꾸세요. 말투나 표현은 자유롭게 바꿔도 AI가 알아듣습니다.

각 예시 아래의 **AI가 하는 일**을 펼치면 AI가 편집기에 실제로 보내는 명령을 볼 수 있습니다. 궁금한 분만 보면 되고, 쓰는 데는 몰라도 됩니다. (예시는 1초에 30장면인 영상 기준입니다.)

## 1. 영상 열고 싱크 맞추기

> D:\videos\a.mp4를 원본으로, D:\videos\b.mp4를 이펙트로 열고 이펙트를 12프레임 늦춰 줘

원본 영상과 이펙트 영상을 열고, 이펙트 영상이 원본보다 12장면 늦게 시작하게 합니다.

<details>
<summary>AI가 하는 일</summary>

```json
{ "tool": "open_media", "arguments": { "kind": "source", "path": "D:\\videos\\a.mp4" } }
{ "tool": "open_media", "arguments": { "kind": "effect", "path": "D:\\videos\\b.mp4" } }
{ "tool": "edit_project", "arguments": { "changes": [
  { "op": "set_track_offset", "effect": 1, "offset": 12 }
] } }
```

두 번째 명령의 결과로 이펙트 번호(여기서는 1)를 받고, 그 번호로 늦춥니다.
</details>

## 2. 화면에 글자 넣기

> 3초부터 5초까지 오른쪽 위에 '안녕' 글자를 넣어 줘

AI가 글자 크기를 먼저 재서 오른쪽 위 가장자리에 맞춰 넣습니다.

<details>
<summary>AI가 하는 일</summary>

```json
{ "tool": "measure_text", "arguments": { "text": "안녕", "size": 72 } }
{ "tool": "edit_project", "arguments": { "changes": [
  { "op": "add_object", "frame": "00:03:00", "end_frame": "00:05:00",
    "shape": { "type": "text", "x": 1700, "y": 40, "text": "안녕", "size": 72 } }
] } }
```

`"00:03:00"`은 분:초:장면 형식으로 3초입니다.
</details>

## 3. 자막 넣기

> D:\videos\c.srt 자막을 넣고 0.5초 앞당겨 줘

<details>
<summary>AI가 하는 일</summary>

```json
{ "tool": "open_media", "arguments": { "kind": "subtitle", "path": "D:\\videos\\c.srt" } }
{ "tool": "edit_project", "arguments": { "changes": [
  { "op": "set_subtitle_offset", "offset": -15 }
] } }
```

0.5초는 30장면 영상에서 15장면입니다.
</details>

## 4. 영상 이어 붙이고 자르고 페이드 넣기

> 뒤에 D:\videos\d.mp4를 이어 붙이고, d.mp4의 앞 2초를 잘라 내고 1초 페이드 인을 넣어 줘

새 영상은 항상 맨 뒤에 붙습니다. 순서를 바꾸고 싶으면 "두 번째 영상을 세 번째로 옮겨 줘"처럼 부탁하세요(첫 번째 영상은 옮길 수 없습니다).

<details>
<summary>AI가 하는 일</summary>

```json
{ "tool": "open_media", "arguments": { "kind": "source_append", "path": "D:\\videos\\d.mp4" } }
{ "tool": "edit_project", "arguments": { "changes": [
  { "op": "set_source_cut", "source": 2, "start": 60, "end": 900 },
  { "op": "set_source_fade", "source": 2, "fade_in": 30, "fade_out": 0 }
] } }
```

자르기는 그 파일의 처음부터 센 장면 번호입니다. `end`(남길 부분의 끝)는 AI가 파일 길이를 먼저 읽어서 정합니다.
</details>

## 5. 결과 확인하기

> 지금 화면을 보여 줘

> 1분 20초의 페이드가 잘 됐는지 확인해 줘

AI가 그 구간을 사진 여러 장으로 보고, 소리 크기도 함께 확인합니다. 자세한 내용은 [AI가 결과를 확인하는 방법](../docs/checking-video-and-audio.ko.md)에 있습니다.

<details>
<summary>AI가 하는 일</summary>

```json
{ "tool": "look", "arguments": {} }
{ "tool": "frame_at_time", "arguments": { "time": "1:20" } }
{ "tool": "look_range", "arguments": { "start": 2400, "end": 2431, "count": 6, "max_size": 384 } }
{ "tool": "get_audio", "arguments": { "start": 2400, "end": 2431 } }
```
</details>

## 6. 되돌리기

> 방금 것 취소해 줘

AI의 부탁 한 번이 한 단계로 되돌려집니다. 편집기에서 직접 Ctrl+Z를 눌러도 같습니다.

<details>
<summary>AI가 하는 일</summary>

```json
{ "tool": "history", "arguments": { "action": "undo" } }
```
</details>

## 7. 저장하고 내보내기

> 프로젝트를 D:\work\my.evproj로 저장해 줘

> D:\out\final.mp4로 H.265로 내보내 줘

내보내기는 시간이 걸립니다. AI가 진행 상황을 확인하고 끝나면 알려 줍니다. 같은 이름의 파일이 이미 있으면 덮어써도 되는지 먼저 묻습니다.

<details>
<summary>AI가 하는 일</summary>

```json
{ "tool": "save_project", "arguments": { "path": "D:\\work\\my.evproj" } }
{ "tool": "export", "arguments": { "path": "D:\\out\\final.mp4", "codec": "h265" } }
{ "tool": "export_status", "arguments": {} }
```
</details>
