**AIエージェントに渡すためのプロンプト例（日本語）**

---

あなたは高度な Python データサイエンスアシスタントです。以下のタスクを順番に実行し、コードと結果をレポート形式で提示してください。説明は日本語で、コードブロックは `python` でマークしてください。

---

### 📂 設定

* 使用言語: **Python 3.10 以降**
* 必須ライブラリ: `pandas`, `numpy`, `scipy`, `statsmodels`（または `sklearn` の `SplineTransformer`＋`LinearRegression` でも可）、`matplotlib`（グラフ用）
* データ: **./TrainingDataForReport5.csv**（1 列目 `X`, 2 列目 `Y` と仮定）

---

### 📝 タスク

1. **データ読み込みと可視化**

   * CSV を読み込み、上位 5 行を表示
   * 散布図 (X vs Y) を描画し、外れ値の有無を簡潔にコメント

2. **3 次スプライン回帰モデル 𝑓(𝑋) の構築**

   * ノットを **c₁=2, c₂=4, c₃=6, c₄=8** に固定
   * 基底: 3 次 B スプライン（自然スプライン可）
   * 推定手法: 最小二乗回帰
   * 推定した係数 β とモデルの数式を出力

3. **学習データへのフィット結果の可視化**

   * 学習点に対する予測曲線をプロット
   * 残差プロット (X vs 残差) を描画し、パターンを 2〜3 行でコメント

4. **マジックフォーミュラによる CVLOO の計算**

   * ハット行列 **H = X (Xᵀ X)⁻¹ Xᵀ** を計算し、対角要素 hᵢᵢ を抽出
   * マジックフォーミュラ

     $$
     \text{CVLOO}_{\text{magic}}=\frac{1}{n}\sum_{i=1}^{n}\left( \frac{y_i-\hat{f}(x_i)}{1-h_{ii}} \right)^2
     $$
   * 得られた CVLOO 値を表示

5. **逐次除去（真の LOOCV）による CVLOO の計算**

   * 各 i について「i 番目を除いたデータ」でモデルを再学習し ̂𝑓₋ᵢ(𝑥ᵢ) を算出
   * $$
     \text{CVLOO}_{\text{brute}}=\frac{1}{n}\sum_{i=1}^{n}(y_i-\hat{f}_{-i}(x_i))^2
     $$
   * 得られた CVLOO 値を表示

6. **結果比較と考察**

   * 2 つの CVLOO 値を表形式で並べ、差を数値 (絶対差・パーセント差) で示す
   * 一致または非一致の理由をスプラインモデルの線形性・数値誤差の観点から 200 字以内で説明

---

### 📤 出力フォーマット

1. **セクション見出し**（Markdown 見出し `##`）
2. **コードブロック**：実行可能な最小限のコード。各ステップ毎にコメントを入れる
3. **図表**：matplotlib 図は PNG として表示
4. **テキスト説明**：要点を箇条書き or 簡潔な段落で。冗長な数式展開は不要
5. **最終サマリー**：結果の要約と学びを 3〜4 行で

---

### ✅ 評価観点

* コードの再現性（セルを上から順に実行して通ること）
* グラフ・残差分析を用いたモデル診断の妥当性
* 2 種類の CVLOO の数値一致 (≦1e-10 程度) の確認と解釈

---

以上を満たす完全な Python ノートブック形式の答えを生成してください。
