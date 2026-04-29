# Game Concept: 砦戦略ボードゲーム（仮題）

*Created: 2026-04-29*
*Status: Draft*

---

## Elevator Pitch

> 道を繋ぎ、補給線を維持し、敵の砦を奪うために相手の意図を読み合う、ターン制のオンライン頭脳戦ボードゲーム。

---

## Core Identity

| Aspect | Detail |
| ---- | ---- |
| **Genre** | 抽象戦略ボードゲーム（Tactical Strategy / Abstract Strategy） |
| **Platform** | Cross-platform（PC + Web、将来的に Mobile） |
| **Target Audience** | See Player Profile section below |
| **Player Count** | 2人対戦（PvP メイン、PvE は Tier 1 以降） |
| **Session Length** | 30〜90 分（盤面サイズに依存） |
| **Monetization** | Premium（buy-once、未確定） |
| **Estimated Scope** | Medium MVP (2-3 ヶ月、ソロ) → Large Full Vision (6-9 ヶ月、ソロ) |
| **Comparable Titles** | Into the Breach、Hive、Onitama、Carcassonne |

---

## Core Fantasy

シンプルなルールから生まれる無限の盤面で、**相手の意図を読み、自分の意図を隠し、罠と包囲で勝利を掴む**「純粋な頭脳戦」を体験する。運や反射神経ではなく、計算と読みだけで勝負が決まる戦略ゲーム。

「道を繋ぐ」という能動的行為が「補給線を維持する」という制約を生み、その制約の中で「相手を欺く」という心理戦が成立する三層構造が中核体験。

---

## Unique Hook

> Like Stratego、AND ALSO **「駒の移動が道を生み、道が陣地と補給線を作る」** — つまり**プレイヤーの動きそのものが戦場の地形を変えていく**。

通常のボードゲームでは盤面は固定だが、本作では「過去のすべての移動」が現在の戦略地形になる。プレイヤーは「自分の歩んだ歴史」と戦うことになる。

---

## Player Experience Analysis (MDA Framework)

### Target Aesthetics

| Aesthetic | Priority | How We Deliver It |
| ---- | ---- | ---- |
| **Sensation** | N/A | – |
| **Fantasy** | N/A | – |
| **Narrative** | N/A | – |
| **Challenge** | **1** | 頭脳戦・読み合い・計算が中核 |
| **Fellowship** | **2** | PvP 対戦相手との心理的駆け引き |
| **Discovery** | **3** | ランダム生成マップで毎回違う盤面の探索 |
| **Expression** | **4** | 道の引き方・陣地形状にプレイヤー個性が出る |
| **Submission** | N/A | – |

### Key Dynamics

- プレイヤーは自然と「道を最適化しつつ意図を隠す」二重思考を始める
- 補給線の短さと拡張のリスク・リターン計算が常に発生
- 相手の道の伸び方から次の手を予測する読み合いが生まれる
- 「あと一手で詰む/詰まれる」緊張感が中盤以降継続

### Core Mechanics

1. 駒の移動による道生成（過去の移動が地形になる）
2. 道で囲まれた領域の陣地化と陣地内駒配置
3. 道経由の補給線維持と砦崩壊フェーズ

---

## Player Motivation Profile

### Primary Psychological Needs Served

| Need | How This Game Satisfies It | Strength |
| ---- | ---- | ---- |
| **Autonomy** | 各ターンに無数の選択肢、道の引き方は完全に自由 | Core |
| **Competence** | 道パターン認識・補給計算が上達で見える | Core |
| **Relatedness** | PvP 対戦相手との心理戦による接続 | Supporting |

### Player Type Appeal (Bartle Taxonomy)

- [x] **Killers/Competitors** — 他プレイヤーを頭脳で打ち負かす中核体験
- [x] **Achievers** — 砦レベル/ランクでマスタリーを実感
- [x] **Explorers** — ランダム生成マップで盤面探索（二次的）
- [ ] **Socializers** — 直接対戦以外の社交要素はなし（意図的に対象外）

### Flow State Design

- **Onboarding**: 9×9 のチュートリアル戦で道・補給・砦勝利を10分以内に体験
- **Difficulty Scaling**: 盤面サイズと駒種数で段階的に複雑化（9×9 → 18×18 → 24×24）
- **Feedback Clarity**: 道・陣地・補給線が常に視覚的に明示される
- **Recovery from Failure**: 1ゲーム30〜90分、敗北しても次戦すぐ開始可能

---

## Core Loop

### Moment-to-Moment (30 seconds)

駒を選ぶ → 動かす → 道生成 → 陣地と補給線を再計算 → 相手の応手を読む。**コア思考は「道をどう設計するか」**。空間設計のジレンマ（陣地拡張 vs 補給線短縮）が常に発生。

### Short-Term (5-15 minutes)

「資源まで道を通す」「峡谷を抑える」「敵補給を切るための包囲を組み立てる」といった3〜5ターン規模の短期ゴール達成。

### Session-Level (30-120 minutes)

序盤：拡張（道を伸ばし資源確保）→ 中盤：陣地境界の小競り合い → 終盤：補給戦・崩壊フェーズ・砦突入のドラマ。盤面サイズで30分（9×9）〜90分（24×24）。

### Long-Term Progression

砦レベル＝プレイヤーランク。メタ進化は最小（駒は固定能力、プレイヤースキルだけが成長）。これは意図的設計：「装備で勝つ」ゲームではなく「腕で勝つ」ゲームに留める。

### Retention Hooks

- **Mastery**: 道パターン認識の上達、駒組み合わせの最適化
- **Curiosity**: ランダム生成マップごとの最適解探し
- **Social**: PvP マッチでの読み合い対戦
- **Investment**: ランクアップによる対戦相手の質変化

---

## Game Pillars

### Pillar 1: 道は意図を語る

道の設計は機能だけでなく、プレイヤーの意図そのものを盤面に刻む行為。読まれるリスクと欺くチャンスが同居する。

*Design test*: 「機能的に最適な道」と「意図を隠せる道」の選択を迫られたら → **後者を支援する設計を選ぶ**。

### Pillar 2: シンプルなルール、深い帰結

1ターンで行うことはシンプル（駒を動かす）。しかし道・補給・陣地が連鎖して複雑な盤面を生む。

*Design test*: 新規ルール追加 vs 既存ルールの組み合わせで深みを出す → **後者**。

### Pillar 3: 頭脳戦の純粋性

反射神経・運・パワー差ではなく、読み・計算・心理戦だけで勝負が決まる。

*Design test*: ランダム要素を増やすか減らすか → **減らす（決定論寄り）**。

### Pillar 4: 包囲は罠である

直接戦闘ではなく、補給切断・崩壊フェーズによる「じわじわ首を絞める」ドラマがクライマックス。

*Design test*: 直接攻撃と包囲、どちらを派手に演出するか → **包囲**。

### Anti-Pillars (What This Game Is NOT)

- **NOT リアルタイム反射**: ターン制を崩さない。時間制限はあっても思考時間優先
- **NOT 運による逆転**: サイコロ・ガチャ・引き運で勝敗が変わるシステムは入れない
- **NOT 派手な戦闘演出主導**: エフェクトより戦略の透明性。盤面の状態が常に明確に見える
- **NOT キャラ育成・装備強化**: 駒は固定能力。プレイヤースキルだけが成長する

---

## Visual Identity Anchor (暫定 — `/art-bible` で本格化)

- **方向**: ジオラマ・ストラテジック（俯瞰型）
- **One-line visual rule**: 「すべての戦略要素が一目でわかる、清潔で機能的な俯瞰ジオラマ」

### Supporting Principles
1. **可読性最優先** — 道・補給線・陣地境界は常に明確に区別可能
   *Design test*: 装飾 vs 情報明示 → **情報明示**
2. **戦場の生命感** — 単なる記号ではなく、ジオラマとしての世界感を持つ
   *Design test*: 抽象アイコン vs ミニチュア表現 → **ミニチュア**
3. **静かな緊張感** — 派手な色彩より、落ち着いた自然色＋陣営アクセント色
   *Design test*: ネオン vs 自然色 → **自然色**

### Color Philosophy
自然のアースカラー（緑・茶・砂色・水色）を基調に、2陣営を明確に対比させるアクセント色（例：藍 vs 朱）。情報レイヤー（道・補給・陣地境界）は彩度や線種で識別。

---

## Inspiration and References

| Reference | What We Take From It | What We Do Differently | Why It Matters |
| ---- | ---- | ---- | ---- |
| Into the Breach | 完全決定論・短時間タイトな戦術・読み合い | 道生成と補給線で空間設計が中核 | 戦術深度の検証成功例（25万本以上） |
| Hive | 抽象駒・シンプルルール・PvP特化 | 動的に変わる地形と陣地化 | 抽象戦略のデジタル化成功例 |
| Carcassonne | タイル/道による領域形成 | 駒の能動的移動が地形を作る（受動配置ではない） | 道・領域メカニクスの参考 |
| Onitama | 2人用シンプル抽象戦略 | 大きな盤面と長期的な戦略 | コンパクト戦略ゲームの設計参考 |

**Non-game inspirations**: 古代の城塞戦（ローマ・戦国時代）、補給線戦争史、囲碁の地と境界の概念。

---

## Target Player Profile

| Attribute | Detail |
| ---- | ---- |
| **Age range** | 20-45 |
| **Gaming experience** | Mid-core 〜 Hardcore |
| **Time availability** | 30〜90分の集中したセッションを取れる |
| **Platform preference** | PC（メイン）、Web（カジュアル） |
| **Current games they play** | Hive、Into the Breach、Onitama、将棋/囲碁、Slay the Spire |
| **What they're looking for** | 短すぎず長すぎない、純粋に頭脳を使える対戦ゲーム |
| **What would turn them away** | 運要素過多、リアルタイム要素、派手な演出による情報隠蔽、キャラ育成必須 |

---

## Technical Considerations

| Consideration | Assessment |
| ---- | ---- |
| **Recommended Engine** | `/setup-engine` で決定。候補: Godot 4（軽量・2D強・無料・マルチプラットフォーム）、Unity（モバイル強化 / アセット豊富） |
| **Key Technical Challenges** | (1) 公平なランダムマップ生成、(2) 道/補給/陣地のリアルタイム判定、(3) UX（情報の可視化） |
| **Art Style** | 2D 俯瞰ジオラマ |
| **Art Pipeline Complexity** | Medium（カスタム 2D アセット、自然テーマ） |
| **Audio Needs** | Minimal（環境音 + UI 音中心、BGMは控えめ） |
| **Networking** | MVP: なし（hot-seat）、Tier 2: Client-Server PvP |
| **Content Volume** | MVP: 駒3-4種、9×9盤面、3地形タイプ。Full Vision: 9駒、3盤面サイズ、5地形 |
| **Procedural Systems** | マップ生成（地形・資源・公平性保証アルゴリズム） |

---

## Risks and Open Questions

### Design Risks

- 道の設計が「意図を隠す」ほど複雑にならず、機能最適化に収束する可能性
- 30秒ループの「道生成」が単独で楽しいか未検証
- 完全公開＋一部隠蔽のバランスが取れず、どちらか一方に倒れる可能性

### Technical Risks

- ランダムマップの公平性アルゴリズム（プロトタイプで早期検証必須）
- 補給線判定のパフォーマンス（24×24 で道再計算が重い可能性）
- 公平性保証と多様性の両立（毎回同じようなマップになるリスク）

### Market Risks

- 抽象戦略ボードゲームは比較的ニッチ市場
- PvP 専用ゲームは初期マッチング難（hot-seat MVP で回避）
- 対戦相手不在時の遊び方が弱い（PvE AI がないと遊びにくい）

### Scope Risks

- 9駒・3サイズ・トラップ・崩壊・鯨を全部入れると初開発で時間超過
- → **MVP 厳守でこのリスク軽減**
- 公平マップ生成アルゴリズムが想定以上に難しい可能性

### Open Questions

- 道生成 + 補給線が30秒ループとして楽しいか？ → **`/prototype` で検証**
- 公平なマップ生成は実装可能か？ → プロトタイプで早期検証
- 隠蔽要素（トラップ等）は MVP 以降でいいか？ → MVP プレイテストで判断
- Hot-seat PvP で実プレイヤーから FB を取れるか？ → 友人/家族でテスト計画

---

## MVP Definition

**Core hypothesis**: **「道生成と補給線維持の頭脳戦は、2人 hot-seat 対戦で30分間継続的に楽しい」**

このゲーム全体の「面白さの根幹」がこの仮説で、これが偽なら他の機能（トラップ・崩壊・鯨）を作っても救えない。

### Required for MVP

1. 9×9 ランダム生成マップ（平地・山・川の3地形）
2. 駒3〜4種（狼・熊・人 + 1種）
3. 道生成・陣地化・補給線判定
4. 砦勝利条件（敵砦への侵入で勝利）
5. Hot-seat PvP（1端末で2人交代対戦）
6. 基本 UI（道・補給・陣地の可視化）

### Explicitly NOT in MVP (defer to later)

- トラップ、崩壊フェーズ、鯨ユニット
- 18×18 / 24×24 盤面
- PvE AI、オンライン PvP
- ランクマッチ、メタ進化
- モバイル版
- 5地形すべて、9駒すべて

### Scope Tiers

| Tier | Content | Features | Timeline |
| ---- | ---- | ---- | ---- |
| **MVP** | 9×9 / 3-4駒 / 3地形 | コアループのみ・hot-seat | 2-3 ヶ月 |
| **Vertical Slice** | + 崩壊フェーズ + トラップ | コア + ドラマ要素 | +1-2 ヶ月 |
| **Alpha** | + 全駒・全盤面・鯨 | フル機能、ローカル AI | +2-3 ヶ月 |
| **Full Vision** | 全コンテンツ + 磨き込み | フル機能、磨き込み | 6-9 ヶ月（合計） |
| **Tier 2 (post-launch)** | オンラインPvP・モバイル・ランク | 拡張機能 | – |

---

## Next Steps

- [ ] `/setup-engine` でエンジン決定（Godot 4 / Unity 推奨候補）とリファレンスドキュメント整備
- [ ] `/art-bible` でビジュアルアイデンティティを本格化
- [ ] `/design-review design/gdd/game-concept.md` で本ドキュメント検証
- [ ] `/map-systems` でコンセプトをシステム分解（依存関係マッピング・優先度割当）
- [ ] `/design-system [system]` で各 MVP システムの GDD 作成
- [ ] `/review-all-gdds` でクロスシステム整合性チェック
- [ ] `/gate-check` でアーキテクチャフェーズへの準備確認
- [ ] `/create-architecture` でアーキテクチャブループリント作成
- [ ] `/architecture-decision (×N)` で主要技術決定の記録
- [ ] `/prototype core-loop` で MVP コアループのプロトタイプ
- [ ] `/playtest-report` で核仮説の検証
- [ ] `/sprint-plan new` で初スプリント計画
