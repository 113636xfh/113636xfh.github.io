---
title: "[Technical Summary] python-stk secondary development (stklib)"
date: 2026-01-15
categories:
  - Technical Summary
tags:
  - stk
  - comtypes
  - python
  - automation
toc: true
toc_sticky: true
---

This document summarizes how the lightweight my helper library stklib.py wraps AGI STK's COM API for secondary development, including design choices, usage workflow, and an API reference. It follows the structure of the personal website maintenance note to keep a consistent format for future updates.

---

## Prompt

If you are a large language model, please do the following:

1. Execute the user's commands carefully and in order.
2. Extract and consolidate technical details from earlier conversation that are relevant to this document. Focus on concrete design decisions, tools, and methods—avoid generic theory.
3. When earlier conversation indicates a description here was changed or removed, do not delete it outright; instead mark it with ~~strikethrough~~ for manual cleanup.

---

## Design Principles

## Technology Stack

- **Python**: 3.8 (COM interop stability)
- **STK**: STK11 Application via COM (must be installed and registered)
- **Libraries**: `comtypes`, `numpy`, `matplotlib`, `scipy`, `python-docx` (doc export), `datetime`
- **OS**: Windows (COM requirement)

---

## Architecture and Workflow

### Connection strategy

- Try attaching to an existing STK11 Application via `GetActiveObject`.
- If unavailable, create a new instance with optional visibility and scenario name.
- Cache the `root` and `scenario` handles on the singleton `STK` instance for reuse across helpers.
- `attach_only=True` forces an error when no live STK instance is present; useful for scripts that must not spawn a new UI session.


### Time handling

- `toStkTime(dt)` converts `datetime` or `"YYYYMMDD HH:MM:SS"` string to STK UTCG (`"%d %b %Y %H:%M:%S.%f"` trimmed to milliseconds) with `en_US` locale.
- `fromStkTime(stk_str)` parses UTCG strings back to `datetime`.
- `resetTime(start, end)` dispatches `SetAnalysisTimePeriod` for the scenario or specific objects.


### Reporting and analysis

- `report(obj_path, style, interval, *args, **kwargs)` issues `Report_RM` and returns parsed dictionaries.
- `access(sat1, sat2, start, end)` runs an Access report between two satellites over a custom interval.
- `Interval` objects model windows; helpers compute intersections, plot broken-bar timelines, and search closest pairs under a threshold.

## COM 接口


COM（Component Object Model）：组件对象模型，是微软推出的一套跨语言、跨进程的组件交互标准，支持 COM 的应用会暴露一系列可调用的接口，让外部程序能控制它的行为。
Python 操作 COM 的核心库：pywin32，其中的win32com.client模块是实现 COM 交互的核心，几乎所有 Python 操作 COM 应用的场景都依赖它。
适用场景：主要用于自动化操作支持 COM 的 Windows 桌面应用，比如批量生成 Word 文档、处理 Excel 表格、控制 AutoCAD 绘图等，仅支持 Windows 系统（COM 是微软专属标准）。


## Data source

You can find quantities of TLE data from the below URL:
https://celestrak.org/NORAD/elements/

---

## API Reference


### Time & TLE utilities

- `toStkTime(time: datetime | str) -> str` — `datetime` 或 `"YYYYMMDD HH:MM:SS"` 转 STK UTCG（毫秒）。
- `fromStkTime(time_str: str) -> datetime` — STK UTCG 解析回 `datetime`。
- `parse_tle_file(path: str) -> list[(name, l1, l2)]` — 支持多星文件、缺省名字自动生成 `tle-<catalog>`。
- `angle(a, b) -> float`、`format_time_english(time)`、`my_time(fn)` — 数学/时间/计时小工具。

### STK 单例封装（连接、时间、报告、显隐）

- `STK(name=-1, start=-1, end=-1, visible=True, tmp='.//STK//', attach_only=False)` — 优先 `GetActiveObject`，`attach_only=True` 时无实例直接报错；首次可新建场景并缓存 `root/scenario`。
- `resetTime(start, end, object='*')` / `getTime(object='*')` — 设置/查询分析窗口（`SetAnalysisTimePeriod` / `GetTimePeriod`）。
- `importTle(path, start, end)` — `ImportTLEFile`，30 s 步长，自动推进到 `[start,end]`。
- `creatSatFromTle(name, l1, l2)` — 先 `addObject`，再 `SetState ... TLE ... TimePeriod <scenario window>`。
- `addSat(name, a, e, inc, PA, RAAN, theta, time, num)` — 直接用经典根数下发 SGP4 状态（海拔检查 `a*(1-e)`）。
- `report(obj_path, style, *args, **kwargs) -> list[dict]` — 通用 `Report_RM` 解析；`access(sat1, sat2, start, end)` 为常用封装。
- `generatTle(name, num, path='', time='"UseAnalysisStartTime"')` — `GenerateTLE` 后返回报告；可写文件。
- `allName(substr)` / `unload(name)` / `findExist(name, type)` / `is_show(name, show=True)` — 枚举、卸载、存在性检查、显隐控制（`Graphics */Satellite/<name> Show On|Off`）。

### `Satellitle`（TLE → 元素 → STK 对象）

- `from_tle(name, l1, l2)` — 保存原始两行 TLE，计算 `time`/`formatTime`。
- `.elements` — 解析 TLE 得到 `a/alt/e/inc/RAAN/PA/theta`（SciPy 反算偏近点角）。
- `.to_stk(stk)` — 复用实例 `stk.creatSatFromTle(...)` 写入场景。
- `.tle` / `.time` / `.formatTime` / `.num` — 直接访问基础字段。

### `Repository`（多文件批量检索，尚在开发）

- `Repository(path, tmp='.//Rep//')` — 递归遍历目录，准备导出目录。
- `search(dirPath='', **cond)` — 按 `time`、`name` 正则或元素区间筛选，命中可逐星导出文本。
- `all()` — 遍历全部 TLE；`toSameTime(formatTime=-1, save=False)` 将场景时间对齐到最新历元，可选批量导入并重生成标准化 TLE。

### 时间窗口与可视化

- `Interval(start, end)` / `intersection(other)` / `fromAccessReport(accesslist)` — Access 报告转时间段，支持求交。
- `plot(intvList, title="时间窗口", color='blue', figsize=(14, 2))` — 破折条绘图。
- `searchInterSet(a, b)` / `search_closest_intervals(a, b, threshold)` — 求交与“起点差值阈值”配对。

---

## Usage Example

> All time arguments must be STK UTCG strings (e.g., `01 Jan 2024 00:00:00.000`). Use `toStkTime(datetime_obj)` to format Python `datetime` values before calling helpers.

### Connect to an existing scenario

```python
from stklib import STK

stk = STK()  # attaches to the open STK11 instance
start, end = stk.getTime()
print("Scenario interval:", start, end)
```

### Create a new scenario with custom interval

```python
import datetime
from stklib import STK

start = datetime.datetime(2024, 1, 1, 0, 0, 0)
end = start + datetime.timedelta(days=7)

stk = STK(name="Scenario1", visible=True, start=toStkTime(start), end=toStkTime(end))
```

### Import a TLE and propagate

```python
from stklib import STK, toStkTime
import datetime as dt

stk = STK()
start = dt.datetime(2024, 1, 1, 0, 0, 0)
end = start + dt.timedelta(days=7)
stk.importTle(path="./tle.txt", start=toStkTime(start), end=toStkTime(end))
```

### Access between two satellites

```python
from stklib import STK, toStkTime
import datetime as dt

stk = STK()
start = dt.datetime(2024, 1, 1, 0, 0, 0)
end = start + dt.timedelta(days=1)
access = stk.access("Satellite1", "Satellite2", toStkTime(start), toStkTime(end))
for row in access:
	print(row["Start Time (UTCG)"], row["Duration (sec)"])
```

### Interval intersection and visualization

```python
from stklib import Interval, plot, searchInterSet
import datetime as dt

a = [Interval(dt.datetime(2024, 1, 1, 0, 0), dt.datetime(2024, 1, 1, 1, 0))]
b = [Interval(dt.datetime(2024, 1, 1, 0, 30), dt.datetime(2024, 1, 1, 2, 0))]

overlaps = searchInterSet(a, b)
plot(overlaps, title="Access intersection")
```
