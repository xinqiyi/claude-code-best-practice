---
name: weather-fetcher
description: 从 Open-Meteo API 获取阿联酋迪拜当前天气温度数据的说明
user-invocable: false
allowed-tools:
  - "WebFetch(*)"
---

# Weather Fetcher Skill

此 skill 提供了获取当前天气数据的说明。

## 任务

以请求的单位（摄氏度或华氏度）获取阿联酋迪拜的当前温度。

## 操作说明

1. **获取天气数据**：使用 WebFetch 工具从 Open-Meteo API 获取迪拜的当前天气数据。

   对于 **Celsius**：
   - URL：`https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=celsius`

   对于 **Fahrenheit**：
   - URL：`https://api.open-meteo.com/v1/forecast?latitude=25.2048&longitude=55.2708&current=temperature_2m&temperature_unit=fahrenheit`

2. **提取温度**：从 JSON 响应中提取当前温度：
   - 字段：`current.temperature_2m`
   - 单位标签位于：`current_units.temperature_2m`

3. **返回结果**：清晰地返回温度值和单位。

## 预期输出

完成此 skill 的操作说明后：
```
Current Dubai Temperature: [X]°[C/F]
Unit: [Celsius/Fahrenheit]
```

## 备注

- 仅获取温度，不执行任何转换或写入任何文件
- Open-Meteo 免费使用，无需 API key，且基于坐标查询以保证可靠性
- 迪拜坐标：纬度 25.2048，经度 55.2708
- 清晰返回数值温度值和单位
- 根据调用方请求支持摄氏度和华氏度
