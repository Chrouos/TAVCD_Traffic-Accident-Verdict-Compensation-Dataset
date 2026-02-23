# TAVCD（交通事故判決賠償資料集）

[English](./README.md) | 繁體中文

TAVCD 是針對台灣交通事故民事判決書建立的標註資料集，目標是從判決文本擷取事故事實、傷害情況、肇責比例與各項賠償金額。

## 資料集概覽

- 規模：1,000 筆判決
- 標註方式：雙標註（`Source/Labeler_1`、`Source/Labeler_2`）
- 格式：JSONL
- 主要用途：法律資訊擷取、命名實體擷取、判決分析

## 目錄結構

- `Source/Verdict/`：判決原文與清理後文本
- `Source/Labeler_1/`、`Source/Labeler_2/`：人工標註與文字位置
- `finetuning_training_data_golden.jsonl`：每筆判決完整結構化標註
- `finetuning_training_data.jsonl`：欄位層級訓練樣本
- `regular_fields.py`：欄位定義
- `processed_to_format.py`：格式轉換工具

## 欄位專有名詞對照與判決語意

下表整理中文欄位、英文名稱，以及該欄位在台灣判決書中的實際法律意涵。

### 核心欄位（對應實驗表）

| 中文欄位 | 英文名稱 | 在判決書中的意思 |
|---|---|---|
| 事故日期 | Accident Date | 指事故實際發生日，不是起訴日或判決宣判日。 |
| 事發經過 | Accident Details | 事故發生過程敘述，包含地點、行為、碰撞順序與肇責判斷基礎。 |
| 事故車出廠日期 | Vehicle Manufacturing Date | 車輛出廠或車齡參考資訊，用於折舊與車損價值判定。 |
| 傷勢 | Injury Status | 法院認定與事故具有因果關係的受傷內容。 |
| 職業 | Occupation | 原告職業背景，常用於工作損失或勞動能力減損評估。 |
| 折舊方法 | Depreciation Method | 法院採用的折舊計算方式，用於修車或財損金額調整。 |
| 被告肇責 | Defendant Liability | 被告應負擔的過失比例（通常為百分比），影響最終賠償分攤。 |
| 塗裝 | Coating | 車損修復中塗裝工項的費用。 |
| 工資 | Labor Costs | 修復工時與技工工資費用。 |
| 烤漆 | Painting | 烤漆工項費用，常為修車費細項之一。 |
| 鈑金 | Sheet Metal | 鈑金整修費用，屬車體修復項目。 |
| 耐用年數 | Durable Years | 法院用於折舊判斷的耐用年限。 |
| 修車費用 | Repair Costs | 車輛修復總額，通常由零件、工資、鈑金、烤漆等加總。 |
| 賠償金額總額 | Total Compensation Amount | 判決認定的最終應賠償總額（可能含扣除或抵充後結果）。 |
| 保險給付金額 | Insurance Payment Amount | 保險已給付或應抵充之金額，影響被告實際給付額。 |
| 居家看護天數 | Home Care Days | 法院認定合理的居家看護期間天數。 |
| 居家看護費用 | Home Care Amount | 居家看護總賠償金額。 |
| 每日居家看護金額 | Daily Home Care Amount | 計算居家看護費時採用的每日單價。 |

### 其他常見欄位

| 中文欄位 | 英文名稱 | 在判決書中的意思 |
|---|---|---|
| 醫療費用 | Medical Expenses | 與事故有因果關係、經法院認可的醫療支出。 |
| 精神賠償 | Non-Pecuniary Damages | 精神痛苦、生活影響等非財產上損害賠償。 |
| 住院看護天數 | Inpatient Care Days | 法院認定需要住院看護的天數。 |
| 住院看護費用 | Inpatient Care Amount | 住院看護總賠償金額。 |
| 每日住院看護金額 | Daily Inpatient Care Amount | 計算住院看護費時採用的每日單價。 |
| 每日工作收入 | Daily Work Income | 計算工作損失時採用的每日收入基準。 |
| 工作損失天數 | Work Loss Days | 因傷無法工作天數。 |
| 工作損失 | Work Loss Amount | 法院認定的工作收入損失總額。 |
| 每日營業收入 | Daily Business Income | 自營或營業損失計算的每日收入基準。 |
| 營業損失天數 | Business Loss Days | 法院認定的營業中斷天數。 |
| 營業損失 | Business Loss Amount | 法院准許的營業損失總額。 |
| 零件 | Parts | 修車更換零件費用。 |
| 材料 | Materials | 修復耗材與材料費用。 |
| 交通費用 | Transportation Costs | 就醫、復健或事故處理衍生且可歸責的交通支出。 |
| 財產損失 | Property Damage | 車輛外其他財物損害金額。 |

## JSONL 範例

```json
{
  "input": "完整判決文本...",
  "output": {
    "事故日期": "105年11月18日",
    "事發經過": "...",
    "被告肇責": "80",
    "賠償金額總額": "168萬1320"
  }
}
```

## 使用方式

1. 交通事故判決賠償趨勢分析
2. 法律資訊擷取模型訓練與評估
3. Prompt 型資訊抽取比較研究

```python
import json

with open("finetuning_training_data_golden.jsonl", "r", encoding="utf-8") as f:
    first = json.loads(next(f))
print(first.keys())
```

```bash
python ./processed_to_format.py \
  --type format_data_text \
  --data_path finetuning_training_data_golden.jsonl \
  --output_path ./instruction/
```

## 聯絡方式

若對數據有問題，也可以聯繫：`chrbezz0487@gmail.com`

## 授權

僅供學術研究與非商業用途，使用時請遵守相關法規與隱私要求。
