# TAVCD (Traffic Accident Verdict Compensation Dataset)

English | [繁體中文](./README_zh.md)

TAVCD is an annotated dataset for Taiwan traffic-accident civil verdicts.  
It targets legal information extraction from judgment text, including accident facts, injuries, liability allocation, and compensation components.

## Dataset Snapshot

- Size: 1,000 verdicts
- Annotation: double annotation (`Source/Labeler_1`, `Source/Labeler_2`)
- Format: JSONL
- Main tasks: legal IE, NER-style extraction, judgment analytics

## Directory Structure

- `Source/Verdict/`: raw and cleaned verdict text
- `Source/Labeler_1/`, `Source/Labeler_2/`: human annotations with span positions
- `finetuning_training_data_golden.jsonl`: full structured targets per verdict
- `finetuning_training_data.jsonl`: field-level extraction examples
- `regular_fields.py`: canonical field definitions
- `processed_to_format.py`: conversion utilities for training formats

## Field Terminology and Legal Meaning

The table below maps domain-specific Chinese labels to English terms and clarifies what each field means in Taiwan verdicts.

### Core Fields (used in experiment table)

| Chinese Field | English Term | Meaning in Verdict Context |
|---|---|---|
| 事故日期 | Accident Date | The date when the accident actually occurred (fact-finding section), not the filing or judgment date. |
| 事發經過 | Accident Details | The narrative of how the collision happened: parties, behaviors, place, sequence, and fault-related facts. |
| 事故車出廠日期 | Vehicle Manufacturing Date | The manufacturing/registration-era reference used for valuation and depreciation in damage calculation. |
| 傷勢 | Injury Status | Medically described injuries recognized by the court as caused by the accident. |
| 職業 | Occupation | The claimant's occupation used to support lost-income or earning-capacity claims. |
| 折舊方法 | Depreciation Method | The court-accepted rule for reducing repair/parts value due to age or usage. |
| 被告肇責 | Defendant Liability | The defendant's fault share (usually a percentage) used to apportion payable damages. |
| 塗裝 | Coating | Coating-related repair cost component for damaged vehicle/property items. |
| 工資 | Labor Costs | Repair labor charges recognized by the court. |
| 烤漆 | Painting | Painting/baking-paint process cost component within repair expenses. |
| 鈑金 | Sheet Metal | Bodywork/metal-forming repair cost component. |
| 耐用年數 | Durable Years | Useful life period used by the court for depreciation and residual value reasoning. |
| 修車費用 | Repair Costs | Total recognized vehicle repair amount (often aggregated from parts, labor, coating, paint, sheet metal). |
| 賠償金額總額 | Total Compensation Amount | Final payable compensation amount determined by the judgment (before/after offsets as written by the court). |
| 保險給付金額 | Insurance Payment Amount | Insurance amount already paid or to be offset against defendant payment obligations. |
| 居家看護天數 | Home Care Days | Number of days for home nursing/care accepted by the court. |
| 居家看護費用 | Home Care Amount | Total home care compensation granted by the court. |
| 每日居家看護金額 | Daily Home Care Amount | Daily rate used by the court for home care calculation. |

### Additional Common Fields

| Chinese Field | English Term | Meaning in Verdict Context |
|---|---|---|
| 醫療費用 | Medical Expenses | Court-recognized treatment and related medical spending causally linked to the accident. |
| 精神賠償 | Non-Pecuniary Damages | Compensation for mental suffering (pain and emotional distress). |
| 住院看護天數 | Inpatient Care Days | Number of inpatient nursing days recognized by the court. |
| 住院看護費用 | Inpatient Care Amount | Total inpatient nursing compensation amount. |
| 每日住院看護金額 | Daily Inpatient Care Amount | Daily inpatient care rate used in damage calculation. |
| 每日工作收入 | Daily Work Income | Daily earning basis used to compute work-loss claims. |
| 工作損失天數 | Work Loss Days | Number of days the claimant could not work due to injury. |
| 工作損失 | Work Loss Amount | Total loss-of-earnings amount accepted by the court. |
| 每日營業收入 | Daily Business Income | Daily business revenue basis for self-employed/business loss claims. |
| 營業損失天數 | Business Loss Days | Days of business interruption accepted by the court. |
| 營業損失 | Business Loss Amount | Total business interruption loss amount granted. |
| 零件 | Parts | Parts replacement cost component in repair damages. |
| 材料 | Materials | Consumable/material cost component in repair damages. |
| 交通費用 | Transportation Costs | Transport-related expenses causally linked to treatment, commuting, or handling accident consequences. |
| 財產損失 | Property Damage | Non-vehicle property damage amount recognized by the court. |

## JSONL Format (Example)

```json
{
  "input": "Full judgment text...",
  "output": {
    "事故日期": "105年11月18日",
    "事發經過": "...",
    "被告肇責": "80",
    "賠償金額總額": "168萬1320"
  }
}
```

## Usage

1. Legal compensation analysis for Taiwan traffic verdicts
2. Supervised IE training and evaluation
3. Prompt-based extraction benchmarking

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

## Contact

If you find any dataset issues, please contact: `chrbezz0487@gmail.com`

## License

For academic research and non-commercial use only.  
Please comply with applicable legal and privacy requirements.
