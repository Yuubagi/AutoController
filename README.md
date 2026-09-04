# AutoController

**In-app automation for sideloaded iOS apps**  
**サイドロードしたiOSアプリ向けのアプリ内自動操作ツール**

AutoController is a binary-only automation component that can be injected into a modified iOS IPA.  
It provides touch recording, AutoTouch-compatible script playback, repeat/speed controls, script management, and an in-app floating controller.

AutoControllerは、改変したiOS IPAへ組み込んで使用するバイナリ形式の自動操作ツールです。  
タッチ操作の録画、AutoTouch互換スクリプトの再生、リピート・速度調整、スクリプト管理、フローティングボタンによる操作に対応しています。

---

## 日本語

### 概要

AutoControllerは、対象アプリの内部でタッチ操作を録画・再生するためのiOS向け自動化ツールです。

脱獄環境全体を操作するツールではなく、**AutoControllerを組み込んだ対象アプリ内のみ**で動作します。

### 主な機能

- アプリ内タッチ操作の録画
- AutoTouch形式 `.lua` スクリプトとして保存
- AutoTouchスクリプトの読み込み・再生
- マルチタッチ対応
- ドラッグ・スワイプ操作の記録と再生
- 録画時の解像度を基準に座標を自動スケーリング
- スクリプト名の変更
- スクリプト削除
- Filesアプリからスクリプトをインポート
- 再生回数の指定
  - `0` = 無限リピート
- リピート間インターバル指定
  - 0.1秒刻み
- 再生速度変更
  - 0.1倍刻み
- 日本語 / English UI切り替え
- フローティングボタン
  - 待機中: 自動化アイコン
  - 録画中: `REC`
  - 再生中: `▶`
- 再生中に `▶` を押して即時停止
- 録画中に `REC` を押して停止・保存

### 対応しているAutoTouch関数

現在、以下のAutoTouch形式の命令に対応しています。

```lua
appActivate("com.example.app")

touchDown(1, 400, 1200)
touchMove(1, 450, 1100)
touchUp(1, 450, 1100)

usleep(1000000)
```

対応:

| 関数 | 内容 |
|---|---|
| `appActivate(bundleId)` | Bundle IDを読み取ります。別アプリへの切り替えは行いません |
| `touchDown(id, x, y)` | 指を押す |
| `touchMove(id, x, y)` | 指を移動する |
| `touchUp(id, x, y)` | 指を離す |
| `usleep(microseconds)` | 指定時間待機 |

AutoTouch録画ヘッダーの以下の情報も利用します。

```lua
-- Resolution: 1170, 2532
-- Front most app: Example
-- Orientation of front most app: Portrait
```

`Resolution` は端末解像度に合わせた座標変換に使用されます。

### インストール

GitHubの **Releases** から最新のビルド済み `AutoController.dylib` をダウンロードしてください。

AutoController単体ではアプリとして起動できません。  
使用するには、利用者自身が所有・利用するIPAへ `AutoController.dylib` を組み込み、適切に再署名してインストールする必要があります。

一般的な配置先:

```text
Payload/
└── Example.app/
    └── Frameworks/
        └── AutoController.dylib
```

使用するIPA編集・dylib注入・再署名ツールに従って組み込んでください。

> IPAやアプリの改変が利用規約・ライセンス・法律に抵触しないことを、利用者自身で確認してください。

### 基本操作

#### メニューを開く

アプリ上に表示されるフローティングボタンをタップします。

#### 操作を録画

1. メニューを開く
2. `操作を録画` を選択
3. メニューが閉じ、ボタンが `REC` に変化
4. 対象アプリを操作
5. `REC` をタップ
6. 録画を停止し `.lua` として保存

録画したスクリプトはAutoControllerのスクリプト一覧から再生できます。

#### スクリプトを再生

1. 保存済みスクリプトを選択
2. 再生オプションを指定
3. `再生` をタップ
4. メニューが閉じてから再生開始

設定可能な項目:

- **リピート回数**
  - 1回刻み
  - `0` = 無限
- **インターバル**
  - 0.1秒刻み
- **再生速度**
  - 0.1倍刻み

再生中はフローティングボタンが `▶` になります。  
`▶` を押すと即座に再生を停止します。

### スクリプトの管理

保存済みスクリプトを左へスワイプすると、

- 名前変更
- 削除

を選択できます。

名前変更は表示名のみ変更し、元の `.lua` ファイル内容は変更しません。

### 対応環境

- iOS / iPadOS
- arm64
- iOS 13.0以降をターゲットとしてビルド
- IPAへdylibを組み込める環境

AutoControllerはUIKitの内部APIや動的に取得したシステムAPIを利用しています。  
そのため、**すべてのiOSバージョン・すべてのアプリでの動作を保証するものではありません。**

ゲームエンジン、独自描画UI、iOSアップデートなどにより動作が変わる場合があります。

### 注意事項

- 対象アプリ内の自動操作を目的としています
- 他アプリを横断して操作するシステム全体の自動化ツールではありません
- アプリやサービスによっては自動化が利用規約で禁止されている場合があります
- 利用は自己責任で行ってください
- アプリ更新やiOS更新により動作しなくなる可能性があります
- バイナリの解析・改変対策を施していますが、完全な耐解析性を保証するものではありません

### サポート

AutoControllerを気に入っていただけた場合は、Ko-fiから開発を支援していただけます。

**Ko-fi:** https://ko-fi.com/yuunagisan

### ライセンス / 配布条件

Copyright © 2026 Yuunagi. All rights reserved.

---

## English

### Overview

AutoController is an iOS in-app automation tool designed to record and replay touch interactions inside a modified/sideloaded application.

It is **not a system-wide automation tool**.  
Automation is limited to the application into which AutoController has been injected.

### Features

- Record in-app touch interactions
- Save recordings as AutoTouch-compatible `.lua` scripts
- Import and play AutoTouch scripts
- Multi-touch support
- Record and replay drag/swipe gestures
- Automatic coordinate scaling based on the recording resolution
- Rename scripts
- Delete scripts
- Import scripts from the iOS Files picker
- Repeat count
  - `0` = infinite repeat
- Repeat interval
  - Adjustable in 0.1-second steps
- Playback speed
  - Adjustable in 0.1x steps
- Japanese / English UI
- Floating controller
  - Idle: automation icon
  - Recording: `REC`
  - Playing: `▶`
- Tap `▶` to immediately stop playback
- Tap `REC` to stop and save a recording

### Supported AutoTouch Functions

AutoController currently supports the following AutoTouch-style commands:

```lua
appActivate("com.example.app")

touchDown(1, 400, 1200)
touchMove(1, 450, 1100)
touchUp(1, 450, 1100)

usleep(1000000)
```

Supported commands:

| Function | Description |
|---|---|
| `appActivate(bundleId)` | Reads the bundle ID. It does not switch to another app |
| `touchDown(id, x, y)` | Touch down |
| `touchMove(id, x, y)` | Move an active touch |
| `touchUp(id, x, y)` | Release a touch |
| `usleep(microseconds)` | Wait for the specified duration |

AutoController also reads AutoTouch recording metadata such as:

```lua
-- Resolution: 1170, 2532
-- Front most app: Example
-- Orientation of front most app: Portrait
```

The `Resolution` value is used to scale recorded coordinates to the current device/window size.

### Installation

Download the latest prebuilt `AutoController.dylib` from the GitHub **Releases** page.

AutoController is not a standalone application.  
You must inject the dylib into an IPA that you are authorized to use, then properly re-sign and install the modified IPA.

Typical location:

```text
Payload/
└── Example.app/
    └── Frameworks/
        └── AutoController.dylib
```

Follow the instructions of your preferred IPA injection and signing tool.

> You are responsible for ensuring that modifying or sideloading an application complies with its license, terms of service, and applicable laws.

### Usage

#### Open AutoController

Tap the floating button displayed over the target app.

#### Record Actions

1. Open AutoController
2. Select `Record Actions`
3. The menu closes and the floating button changes to `REC`
4. Use the target app normally
5. Tap `REC`
6. Recording stops and is saved as a `.lua` script

Recorded scripts are immediately available in the Saved Scripts list.

#### Play a Script

1. Select a saved script
2. Configure playback options
3. Tap `Play`
4. The menu closes before playback begins

Playback options:

- **Repeat**
  - Step: 1
  - `0` = infinite
- **Interval**
  - Step: 0.1 seconds
- **Playback Speed**
  - Step: 0.1x

During playback, the floating button changes to `▶`.  
Tap `▶` to stop playback immediately.

### Script Management

Swipe left on a saved script to:

- Rename
- Delete

Renaming changes only the display name.  
The original `.lua` file contents are not modified.

### Compatibility

- iOS / iPadOS
- arm64
- Built with an iOS 13.0 deployment target
- Requires a workflow capable of injecting a dylib into an IPA

AutoController uses internal UIKit behavior and dynamically resolved system APIs.

Therefore, **compatibility with every iOS version and every application is not guaranteed**.

Application updates, iOS updates, custom rendering engines, or other implementation details may affect automation behavior.

### Important Notes

- Intended for automation inside the injected application
- Not designed for system-wide or cross-app automation
- Some applications or services may prohibit automation in their terms of service
- Use at your own risk
- Future iOS or app updates may break compatibility
- The distributed binary includes anti-analysis/obfuscation measures, but complete resistance to reverse engineering cannot be guaranteed

### Support Development

If you enjoy AutoController and would like to support its development:

**Ko-fi:** https://ko-fi.com/yuunagisan

### License / Distribution

Copyright © 2026 Yuunagi. All rights reserved.

---

## Releases

Prebuilt binaries are distributed from the GitHub **Releases** section.

Recommended release asset:

```text
AutoController.dylib
```

No source code is included in this repository.

---

Created by **Yuunagi**
