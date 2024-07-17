## 称号和Buffer

称号和Buffer是利用装备穿戴原理实现属性增幅的系统.

有如下特性:

- 不占用玩家装备格子,可叠加多个
- 在脚本中可动态的给予和移除称号
- 可脚本动态修改称号属性,自定义属性词条
- 可设置限时装备,到期自动回收
- 可自定义外观(无方向和动作)
- 可利用穿戴触发(TakeOn)和装备检测(CheckEquipItem),限时装备实现铭文,成就,动态Buffer等功能

![recording](./res/fenghao.gif)

### 如何在脚本动态给予和移除玩家称号



称号按名称唯一

- 多次给予同名称号仅第一个生效
- 多次给与同名称号会刷新称号的属性(如过期时间,隐藏显示)
- 玩家同时存在多个可显示外观特效的称号,仅显示最后一个称号的外观



```js
//称号相关
function 称号给与() {
    var hidden = getRandomInt(2) == 1;
    ctx.GiveFengHao("独步天下", hidden);
    console.log("hide fenghao:" +hidden);
    ctx.GiveFengHao("玛法至尊",!hidden);
    ctx.close();
    return ``;
}

function 称号回收() {
    ctx.RecycFengHao("独步天下");
    ctx.RecycFengHao("玛法至尊");
    ctx.close();
    return ``;
}

function 称号回收() {
    ctx.RecycFengHao("独步天下");
    ctx.RecycFengHao("玛法至尊");
    ctx.close();
    return ``;
}

```

### 数据库自定义称号

在数据库中增加Type=73的装备,

基础属性与普通装备一样

由于称号不可出售,且不会掉落

利用以下字段来设置装备的外观,

Price: baseIndex, 外观图片起始帧

Effect:动画图片数量

StackSize: 是否透明绘制,(黑底图片设置为1,透明图片为0)

Set: 是否在遮挡人物(设置为1会覆盖衣服)

```
baseIndex: (int)info.Price,
  count: info.Effect,
  duration: info.Effect * 100,
  owner, 0,
  isBlend: info.StackSize == 0
, drawBehind: info.Set > 0
```

装备位置:

称号装备在玩家身上的位置为:`13`

 (源代码定义ItemPostionType.FengHao)



### 限时Buffer

同限时装备设置 [](./限时装备.md)



### 其他

称号sql数据库配置参考:

```sql
INSERT INTO "main"."ItemInfo"("Index", "Name", "Type", "Shape", "Weight", "Image", "Durability", "Light", "MinAC", "MaxAC", "MinMAC", "MaxMAC", "MinDC", "MaxDC", "MinMC", "MaxMC", "MinSC", "MaxSC", "Accuracy", "Agility", "MaxHP", "MaxMP", "AttackSpeed", "Luck", "BagWeight", "HandWeight", "WearWeight", "Reflect", "Strong", "Holy", "Freezing", "PoisonAttack", "MagicSpeed", "MoveSpeed", "MagicResist", "PoisonResist", "HealthRecovery", "SpellRecovery", "PoisonRecovery", "CriticalRate", "CriticalDamage", "CriticalResist", "ReflectRate", "HpDrainRate", "MaxACRatePercent", "MaxMACRatePercent", "MaxDCRatePercent", "MaxMCRatePercent", "MaxSCRatePercent", "AttackSpeedRatePercent", "HPRatePercent", "MPRatePercent", "HPDrainRatePercent", "IgnoreAC", "IgnoreMaC", "DamageIncRate", "DamageDecRate", "ExpRatePercent", "ItemDropRatePercent", "GoldDropRatePercent", "MineRatePercent", "GemRatePercent", "FishRatePercent", "CraftRatePercent", "SkillGainMultiplier", "AttackBonus", "LoverExpRatePercent", "MentorDamageRatePercent", "MentorExpRatePercent", "DamageReductionPercent", "EnergyShieldPercent", "EnergyShieldHPGain", "ManaPenaltyPercent", "TeleportManaPenaltyPercent", "Grade", "RequiredType", "RequiredClass", "RequiredGender", "RequiredAmount", "Price", "Effect", "StackSize", "Set", "StartItem", "Bind", "Unique", "ToolTip", "CanFastRun", "CanAwakening", "RandomStatsId", "Slots", "NotifyAndMore") VALUES (3843, '独步天下', 73, 73, 1, 31081, 6000, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 188, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 13, 31, 3, 28, 119, 15, 0, 0, 0, 0, 0, '{骨灰级老玩家专属称号|70}\', 0, 0, 0, 50, 0);
INSERT INTO "main"."ItemInfo"("Index", "Name", "Type", "Shape", "Weight", "Image", "Durability", "Light", "MinAC", "MaxAC", "MinMAC", "MaxMAC", "MinDC", "MaxDC", "MinMC", "MaxMC", "MinSC", "MaxSC", "Accuracy", "Agility", "MaxHP", "MaxMP", "AttackSpeed", "Luck", "BagWeight", "HandWeight", "WearWeight", "Reflect", "Strong", "Holy", "Freezing", "PoisonAttack", "MagicSpeed", "MoveSpeed", "MagicResist", "PoisonResist", "HealthRecovery", "SpellRecovery", "PoisonRecovery", "CriticalRate", "CriticalDamage", "CriticalResist", "ReflectRate", "HpDrainRate", "MaxACRatePercent", "MaxMACRatePercent", "MaxDCRatePercent", "MaxMCRatePercent", "MaxSCRatePercent", "AttackSpeedRatePercent", "HPRatePercent", "MPRatePercent", "HPDrainRatePercent", "IgnoreAC", "IgnoreMaC", "DamageIncRate", "DamageDecRate", "ExpRatePercent", "ItemDropRatePercent", "GoldDropRatePercent", "MineRatePercent", "GemRatePercent", "FishRatePercent", "CraftRatePercent", "SkillGainMultiplier", "AttackBonus", "LoverExpRatePercent", "MentorDamageRatePercent", "MentorExpRatePercent", "DamageReductionPercent", "EnergyShieldPercent", "EnergyShieldHPGain", "ManaPenaltyPercent", "TeleportManaPenaltyPercent", "Grade", "RequiredType", "RequiredClass", "RequiredGender", "RequiredAmount", "Price", "Effect", "StackSize", "Set", "StartItem", "Bind", "Unique", "ToolTip", "CanFastRun", "CanAwakening", "RandomStatsId", "Slots", "NotifyAndMore") VALUES (3844, '玛法至尊', 73, 73, 1, 31081, 6000, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 188, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 13, 31, 3, 28, 0, 15, 0, 0, 0, 0, 0, '{骨灰级老玩家专属称号|70}\', 0, 0, 0, 50, 0);

```



