# microBall / moNa2（ZMK）― 39キー・5レイヤ運用 要件定義 & 再開用プロンプト

このドキュメントは、これまでの合意内容を保存し、次回すぐに作業を再開できるようにするための **要件定義** と **再開用プロンプト** です。

---

## ✅ ゴール / コンテキスト
- 対象ハード: **microBall / moNa2**, 右手トラックボール付き分割, **39キー**
- ファーム: **ZMK**（QMKは参考。基本はZMK運用）
- 用途: **プログラミング中心**
- ユーザー嗜好: **テンキー派（数字入力）**, **矢印キー多用**, **右手片手でポインタ操作～クリック完結**
- 運用方針: まず **B) 通常の zmk-config（Fork→Actions→UF2）** で管理 → 迅速な試行が必要になったら **A) ZMK Studio** を上乗せ

---

## 🧩 キーマップ設計要件（5レイヤ）
- **L0 Base**
  - 文字入力＋**クリック常時可**（推奨: コンボで `J+K=左クリック`, `K+L=右クリック`）
  - **IME**: 親指に `LANG4（ひらがな）` / `LANG5（半角/全角）`、必要に応じ OSショートカット・マクロ併用
  - Hold-Tap（Mod-Tap）は **tap-preferred**, `tapping-term` は 140–180ms 目安（誤爆抑制）
- **L1 Symbols**
  - プログラミング記号を **Shift不要で単打**（数字はここに置かない）
- **L2 Num + Nav**
  - **左＝テンキー**（KP_*）、**右＝矢印＋編集**（Home/End/PgUp/PgDn/Del など）を**右手寄せ**
- **L3 Scroll**
  - **縦/横スクロールを分離**した専用層。必要に応じて**トグル**や一時レイヤ（`&mo 3` 等）
- **L4 Fn / Utility**
  - F1–F12（テンキー位置に対応させると覚えやすい）, BT選択/クリア, bootloader など

> クリックを L0 に“常駐”させる手段は 2通り：  
> ① 親指キーに `&mkp LCLK/RCLK` を直割り当て ② **コンボで常時可**（J+K=左 / K+L=右）。  
> ②はホームポジション維持＆物理キー節約の観点で推奨。

---

## 🔧 実装メモ（ZMK）
- **コンボ**: `.keymap` の `/ { combos { ... } }` に定義  
  - `timeout-ms: 50–60ms` を基準に、誤爆が気になるものは `require-prior-idle-ms: 100–150` を付加
  - 代表例:  
    - `J+K → 左クリック`, `K+L → 右クリック`  
    - `U+I → ↑`, `H+J → ←`, `J+L → →`, `M+, → ↓`  
    - `[ + ] → &mo 3`（Scroll層を一時有効）
- **スクロール**: `&msc SCRL_UP/DOWN/LEFT/RIGHT` を L3 に割当（縦/横を分離）
- **IME**: `LANG4/LANG5` に加え、OSショートカット（Win+Space / Ctrl+Space 等）をマクロで用意し併用
- **Actions（CI）**: 全ブランチ発火（`on.push.branches: ["**"]`）で運用
- **Git運用**: **Forkで作業**（本家を汚さない）、必要に応じて `upstream` から取り込み

---

## 🛠️ ZMK Studio（必要になったら上乗せ）
- `build.yaml` 左（中央）側に `snippet: studio-rpc-usb-uart` + `-DCONFIG_ZMK_STUDIO=y`
- `.keymap` に `&studio_unlock` を 1キー割り当て
- **注意**: Studioでの編集は **デバイス側に保存**（Git履歴には残らない）。取り込みたい場合は **エクスポート→コミット**。  
  `.keymap` を再度“正”にする際は **Restore Stock Settings** を実行してから適用。

---

## 🧪 動作チェック・チェックリスト
1. 文字入力と Mod（Shift/Ctrl/GUI）  
2. **コンボ**: `J+K` / `K+L` でクリック、`U+I/H+J/J+L/M+,` で矢印  
3. **L1** 記号が単打で出る（Shift不要）  
4. **L2** 左テンキー / 右矢印＋編集（Home/End/PgUp/PgDn/Del）  
5. **L3** 縦/横スクロール  
6. **IME** 切替（LANG4/LANG5／マクロ）

---

## ▶︎ 次回の投入物（ユーザーから）
- 現在の **`.keymap`**（最新）  
- **board / shield 名**（例: `nice_nano_v2` + `microball_left/right` / `mona2_left/right`）  
- OS（Win / macOS / Linux）と IME 方針（LANG/INTキー or OSショートカット）  
- 可能なら **キー位置インデックス表**（なければこちらで推定）

---

## 🔁 再開用プロンプト（これを貼るだけ）
> **以下を丸ごと貼って送ってください。**

