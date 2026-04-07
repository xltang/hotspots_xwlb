# hotspots_xwlb

`hotspots_xwlb` 是一个 OpenClaw Consumer Skill，用于拉取并展示「新闻联播」热点内容。

## 功能

- 调用接口：`GET /hotspots/platform/新闻联播?userId=$USER_ID&&timestamp=$TIME_STEMP`
- 入参生成：本地持久化 `USER_ID` + 每次请求生成分钟级 `TIME_STEMP`
- 展示方式：按 `source_name` 分组展示标题
- 支持状态检查：返回接口可达性、source 数量、item 总数
- 定时触发：安装/首次启用时自动注册每天上午 9:30 任务（Asia/Shanghai）

## 入参与本地存储

- `user_id` 存储目录：`~/.openclaw/hotspots_xwlb/user_id`
- 若 `user_id` 不存在：自动生成并写入，后续复用
- `timestamp` 生成方式（分钟级）：
  - `TIME_STEMP="$(TZ=Asia/Shanghai date +%Y-%m-%dT%H:%M)"`

## 接口信息

- Base URL: `https://hotspot.api4claw.com`
- Endpoint: `/hotspots/platform/新闻联播?userId=$USER_ID&&timestamp=$TIME_STEMP`

## 输出规则

- 不展示 Top Hot 排名
- 不展示 `fetched_at`、`data_date`
- 某个 source 的 `items` 为空时，保留分组并标记为空

## 失败处理

- 接口失败时明确返回失败原因
- 响应不是合法 JSON 时明确提示格式不匹配
- 服务不可达时不编造内容

## 关键文件

- `SKILL.md`: 详细的 Consumer Workflow、定时任务和安全规则
