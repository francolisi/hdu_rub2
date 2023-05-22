<p align="center">
  <a href="https://github.com/Albresky/HDU-Library-Master"><img src="https://s2.loli.net/2023/05/22/3y6dc51NXmabzgj.png" alt="HDU-Library-Master"></a>
</p>
<p align="center">
Daily Seats Reservation for HDU Library
</p>

**中文** | [English](https://github.com/Albresky/HDU-Library-Master/blob/main/readme/README_EN.md)

`Github Actions` 自动触发每日订座任务

---

## 一、使用方法

### 1、`Fork` 本仓库

### 2、添加 `Secrets`

- `Settings` - `[Security]Secrets and Variables` - `Actions` - `New repository secret`

  - `HLMUSERID` 学号
  - `HLMPASSWORD` 图书馆系统登录密码
    - [注] `非数字杭电登录密码`
  - `HLMPLANCODE` 订座任务代码，以英文逗号分隔
    - 如 `code1,code2,code3,...`
    - [注] 至少一个任务代码
  - **[可选]** `HLMMAXTRIALS` 最大尝试次数
    - 默认为 `2`
  - **[可选]** `HLMDELAY` 超时时间，单位为秒
    - 默认为 `10`

### 3、**[可选]** 自定义触发时间

 - 修改 `.github/workflows/workflow.yaml`中的 `cron` 表达式，以控制任务触发时间
 - [注] `cron` 表达式中的时间为 `UTC` 时间，需根据时区调整（东八区时间减8）
   - 如果任务触发时间为 `20:00`（**系统预约隔天座位的开放时间**），则 `cron` 表达式为 `0 12 * * *`


## 二、数据字段格式说明


 - `code` 订座任务代码
   - `roomType:floor:seatNum:startTime:duration`
 - `roomType` 房间类型
    - `1` 自习室
    - `2` 教师休息室
    - `3` 阅览室
    - `4` 讨论室
 - `floor` 楼层

```json
{
  "自习室":{
      "二楼自习室": 1000,
      "二楼电子阅览室": 1524,
      "三楼大厅": 1525,
      "四楼自习室": 1221
  },
  "教师休息室": {}, # 暂不可用
  "阅览室": {
      "自然科学书库（三楼东）": 1403,
      "社会科学书库（三楼西）": 1404,
      "社会科学书库（三楼北）": 1405,
      "研修中心（六楼）": 1407,
      "自然科学第二书库（七楼）": 1408,
      "社会科学第二数据（八楼）": 1409,
      "文学艺术书库（九楼）": 1410,
      "综合第二书库（十楼）": 1411,
      "综合第一书库（十一楼）": 1412
  },
  "讨论室": {} # 暂不可用
}
```

 - `seatNum` 座位编号
   - 官方预约系统中该房间的座位编号
 - `startTime` 开始时间
   - 24小时制，如:
     - `8` 代表 `08:00`
     - `14` 代表 `14:00`
     - `20` 代表 `20:00`

 - `duration` 预约时长
   - 以小时为单位，如:
     - `1` 代表 `1` 小时
     - `2` 代表 `2` 小时
     - `3` 代表 `3` 小时
   - **[注意]** 预约时长须根据 `startTime` 确定


## 三、`Secrets` 示例

 - `HLMUSERID`
   - `12345678`
 - `HLMPASSWORD`
   - `hDu123123`
 - `HLMPLANCODE`
   - `1:1000:15:8:2,1:1524:34:14:5,3:1412:22:18:2`


 - 上述示例 `HLMPLANCODE` 对应的预约计划如下:

| 序号 | 类别     | 位置              | 座位号 | 时间          | 时长   |
| ---- | -------- | ----------------- | ------ | ------------- | ------ |
| 1    | 自习室   | 二楼自习室        | 15     | 08:00 - 10:00 | 2小时  |
| 2    | 自习室   | 二楼电子阅览室    | 34     | 14:00 - 19:00 | 5小时  |
| 3    | 阅览室   | 综合第一书库（十一楼）| 22     | 18:00 - 20:00 | 2小时 |


## 四、鸣谢

 - [LittleHeroZZZX/hdu-library-killer](https://github.com/LittleHeroZZZX/hdu-library-killer)
