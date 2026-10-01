# Design — Nanax Soft（nanaxsoft.github.io）

このサイト内で共有する設計。ページを足す・直すときは最初にこれを読む。
いまの対象は `apps/stockvaluation/index.html`（上場株式の相続税評価額 計算ツール）だけ。
`apps/stockvaluation/version.txt` はアプリの更新フィード＝デザインの対象外・触らない。

## 考え方
- 読み手は税理士・相続人。**信頼と正確さが最優先**。演出は一点だけ。
- ページそのものを**国税庁の帳票（上場株式の評価明細書）**として組む：罫線・二重罫・枠・欄外の注記・右下の様式記号。
- 中心の一点＝**照合盤**：4つの価額（相続開始日の終値／当月・前月・前々月の平均）を並べ、最も低い欄にだけ**蛍光黄のマーカー**（製品の証跡画像と同じ色の意味）と**朱の「採用」印**。
- 数字は**製品の実出力から機械で写す**（目視転記しない）。出典の注記を必ず付ける（「出力例」「原典と照合」）。

## Genre / 構造
- route: custom（bespoke）／ light / high-contrast-serif / warm
- nav: マストヘッド（屋号＋製品名・二重罫・ページ内目次）
- footer: 欄外の注記（販売事業者・免責の要約・©）＋右下の様式記号 `（Nanax Soft — <製品> <版>）`
- 節の見出しは**縦積み**（左に番号を吊る型は使わない）。番号は本当に順序があるもの（工程）だけ。

## 色（OKLCH・全部トークン経由）
| token | 値 | 役目 |
|---|---|---|
| `--color-paper` | oklch(97.4% 0.009 85) | 生成りの紙（地） |
| `--color-paper-2` | oklch(94.8% 0.013 85) | 帯・選択中 |
| `--color-sheet` | oklch(99% 0.004 85) | 帳票の一枚（枠の中） |
| `--color-rule` | oklch(84% 0.012 75) | 細罫 |
| `--color-neutral` | oklch(47% 0.012 70) | 注記（最小 5.85:1） |
| `--color-muted` | oklch(35% 0.012 65) | 本文の二次 |
| `--color-ink` | oklch(21% 0.012 60) | 墨（本文・太罫・ボタン地） |
| `--color-seal` | oklch(51% 0.18 32) | **朱＝「採用」印・リンク下線・フォーカス輪だけ** |
| `--color-marker` | oklch(92% 0.13 102) | **蛍光黄＝「採用された数字」と強調だけ**（墨の文字で 14.15:1） |

朱と黄は意味を持つ機能色。飾りに使わない（1画面の数%以内）。

## 書体
- 見出し：**Shippori Mincho B1** 700/800（Google Fonts・`text=` でページの字だけ）
- 本文：**BIZ UDPゴシック**（Windows 10/11 同梱を端末から使う・Web フォントで落とさない）→ ヒラギノ → Noto Sans JP
- 数字：**BIZ UDGothic**（`text=` で数字・記号だけ）＋ `tabular-nums`
- 見出し・本文とも斜体なし。見出しは句で折る（`.ph` の inline-block と `word-break:auto-phrase`）。
- 🔴 見出し・数字の字を変えたら `Product\_site_redesign_2026-10\stockvaluation\subset_fonts.py` を回して `text=` を作り直す。

## 寸法
- 4pt の名前付き尺度（`--space-3xs`〜`--space-3xl`）。罫は `--rule-hair` 1px／`--rule-strong` 2px。角は 2px（帳票は角を丸めない）。
- 本文の行長 `--measure: 38em`。ページ幅 `--page: 74rem`。
- 画像は原寸を直接出さず、720〜960px の派生版を `srcset`。図の下に「原寸で開く」。

## 動き
- 初回だけ：見出し・照合盤・購入欄が 10px 上がって現れる（560ms・ease-out）。
- 照合盤：採用欄のマーカーが左から引かれ、遅れて印が出る。銘柄切替は 130ms のクロスフェード。
- `prefers-reduced-motion: reduce` ではすべて即時（マーカーと印は最初から描かれている）。
- スクロールで出てくる演出は使わない。

## CTA
- 主：墨ベタ・白抜き・角2px「無料体験版をダウンロード →」（Vector の配布ページへ）。
- 副：枠線だけの小さな「無料体験版」（マストヘッド）。
- 固定バー：ヒーローを過ぎてから結びが見えるまで。

## 変えてはいけないもの
- 既存の URL・アンカー（`#top #how #features #env #faq #legal #vector`）。
- 免責・特商法・FAQ・税法の記述の文言（見た目だけ変えてよい。変えたら本文一致を機械で照合する）。
- 本名・住所・電話は載せない（特商法は請求時開示の文言のまま）。

## Exports

### tokens.css
```css
:root{
  --color-paper: oklch(97.4% 0.009 85);  --color-paper-2: oklch(94.8% 0.013 85);
  --color-sheet: oklch(99% 0.004 85);    --color-rule: oklch(84% 0.012 75);
  --color-neutral: oklch(47% 0.012 70);  --color-muted: oklch(35% 0.012 65);
  --color-ink: oklch(21% 0.012 60);      --color-ink-2: oklch(31% 0.014 60);
  --color-on-ink: oklch(97% 0.008 85);   --color-seal: oklch(51% 0.18 32);
  --color-marker: oklch(92% 0.13 102);   --color-focus: oklch(51% 0.18 32);
  --font-display: "Shippori Mincho B1", "Yu Mincho", "Hiragino Mincho ProN", serif;
  --font-body: "BIZ UDPGothic", "Hiragino Sans", "Noto Sans JP", "Yu Gothic UI", "Meiryo", sans-serif;
  --font-num: "BIZ UDGothic", "BIZ UDPGothic", "Hiragino Sans", ui-monospace, sans-serif;
  --space-3xs: .25rem; --space-2xs: .5rem; --space-xs: .75rem; --space-sm: 1rem; --space-md: 1.5rem;
  --space-lg: 2rem; --space-xl: 3rem; --space-2xl: 4.5rem; --space-3xl: 7rem;
  --text-xs: .75rem; --text-sm: .875rem; --text-base: 1rem; --text-md: 1.125rem; --text-lg: 1.375rem;
  --text-xl: clamp(1.5rem, 1.15rem + 1.3vw, 2.125rem);
  --text-display: clamp(1.875rem, 1rem + 3.7vw, 3.625rem);
  --rule-hair: 1px; --rule-strong: 2px; --radius-control: 2px;
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1); --ease-in: cubic-bezier(0.7, 0, 0.84, 0); --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);
  --dur-micro: 120ms; --dur-short: 220ms; --dur-long: 560ms;
}
```
