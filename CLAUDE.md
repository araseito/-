@SKILL.md

---

## Project Rules (araseito/-)

### コスト最適化ルール（Businessプラン）

現在のアカウントは **Topview AI Businessプラン** を契約しています。動画生成時は必ず以下のコスト最小化ルールを適用してください。

1. **モデルは `Fast` をデフォルトとする** — `Standard` と `Fast` はコストが同じだが `Fast` の方がターンアラウンドが速い。ユーザーが「最高品質」を明示した場合のみ `Standard` に切り替える。
2. **デュレーションは最小限に** — ユーザーが長さを指定しない場合は `5s` をデフォルトとする。
3. **サウンドはデフォルト `off`** — `--sound off` を基本とし、BGM・効果音が必要な場合のみ `on` にする（コスト増加の可能性あり）。
4. **生成数は `--count 1`** — バリエーションが不要な場合は1本のみ生成する。
5. **毎回 `estimate-cost` で確認** — 初回セッションおよびパラメータが変わる場合は必ずコスト見積もりを実行してからユーザーに提示する。
6. **解像度は `720`** — Standard/Fast は720が最高解像度かつコストが変わらないため、常に720を使用する。

### Seedance 2.0 デフォルト設定

> Seedance 2.0 = API上のモデル名 `Fast`（高速） または `Standard`（最高品質）

**デフォルトパラメータ（ユーザー指定がない場合）:**

| パラメータ | デフォルト値 | 理由 |
|-----------|-------------|------|
| `--model` | `Fast` | コスト同一・速度優先 |
| `--resolution` | `720` | Seedance 2.0 最高解像度 |
| `--duration` | `5` | 最小コスト |
| `--sound` | `off` | コスト抑制 |
| `--count` | `1` | コスト抑制 |
| `--aspect-ratio` | `16:9` | 用途未指定時の汎用比率 |

**ネガティブプロンプト（Seedance 2.0 標準）:**

Seedance 2.0 は `--negative-prompt` パラメータを持たないため、プロンプトの末尾に以下を付加する形式で品質を制御する。

```
Avoid: blurry, out of focus, low quality, distorted, deformed, watermark, text overlay, subtitle, logo, signature, flickering, unstable motion, overexposed, underexposed, noise, grain, artifacts, glitch, bad anatomy, choppy, duplicate subjects.
```

人物が含まれる場合は追加:
```
Avoid: bad face, deformed hands, extra limbs, missing limbs, bad proportions, uncanny valley.
```

**プロンプト構成テンプレート:**

```
[Subject + Action] [Environment + Lighting] [Camera motion] [Style/Mood].
Avoid: blurry, out of focus, low quality, distorted, watermark, text, flickering, artifacts, noise.
```

例:
```
A woman walks through a sunlit forest path in autumn. Warm golden light filters through the leaves. Slow dolly forward, shallow depth of field. Cinematic, realistic.
Avoid: blurry, out of focus, low quality, distorted, watermark, text, flickering, artifacts, noise.
```

### 生成前チェックリスト

すべての動画生成タスクで以下を確認する:

1. モデルが `Fast`（または明示的に指定されたモデル）であること
2. `estimate-cost` でクレジット消費を確認・ユーザーに提示済みであること
3. ネガティブプロンプトがプロンプト末尾に含まれていること
4. `--board-id` が設定されていること（セッション内で1回 `board.py list --default -q` を実行）
