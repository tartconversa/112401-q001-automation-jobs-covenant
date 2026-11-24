# 技改项目的岗位影响分解与拨款基准复议

园区技改拨款演练使用两轮上报。付薪工时按班组和实际执行主体提供，产品数量来自另一个报表。币种为元，工时为小时，时点以园区当地日期记录。

## 资料约定

CSV 使用 UTF-8，首行为列名。多值字段以竖线分隔。空单元格表示来源未填。

- output.csv。产品批次报表。units 为合格数量，stage 是基准月份或当期。grade 是产品工艺版本，batch_id 供工时记录引用。
  字段为 `batch_id`、`stage`、`product`、`grade`、`units`。
- hours.csv。企业报送本行实际承担的有薪小时。joint_group 相同代表同一份共同产出工时，见 references。provider 标识自有、借调来源或独立外协。空白 hours 表示该行缺测。
  字段为 `hour_id`、`batch_id`、`step`、`team`、`provider`、`hours`、`joint_group`。
- references.csv。工程记录允许的基准份额区间，不是独立的建议分摊。同一个 scope 有共用余额，relates_to 是适用的工艺或产线范围。 对 E1 的更正由 E5 与复议 A1 关联，各区间为闭区间。
  字段为 `evidence_id`、`scope`、`new_step`、`old_step`、`low_share`、`high_share`、`relates_to`、`received`。
- placements.csv。带姓名映射的工时证明在演练中以班组代号脱敏。credit_id 是不可重复认领的工时凭证，范围由生效记录认定。 更正关系见 appeals。
  字段为 `credit_id`、`team`、`site`、`period`、`hours`、`from_affected`、`provider`、`received`。
- tranches.csv。各期拨款合同记录。required_hours 为下限起算量，displacement_share 为同等产出下减小时数中需额外保护的比例。credits 是企业申请计入的安置工时凭证，管道符分隔。paid 和 review 为外部经办状态。
  字段为 `tranche`、`period`、`required_hours`、`displacement_share`、`amount`、`credits`、`state`、`recorded`。
- appeals.csv。已接收的外部复议，replacement 是补交证明行，valid_from 是所主张的有效起日。
  字段为 `appeal`、`disputed`、`replacement`、`valid_from`、`recorded`、`scope`。

## 检查演练资料

使用 Python 3.11.2 标准库，无需安装第三方依赖。在项目根目录运行下列命令可检查 CSV 读取、资料关联及样例记录完整性。

```bash
python3 -m unittest discover -s tests -v
```
