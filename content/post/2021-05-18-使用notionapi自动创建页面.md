---
type: tech
title: NotionAPI测试
date: 2021-05-18
category: tech
description: 使用NotionAPI每天自动创建页面
---
# 20210519

Created: May 19, 2021 8:00 AM

It is never too late to be what you might have been.

# Notion API

## 使用Notion自动创建每天页面

1. 创建Notion机器人

[Notion - The all-in-one workspace for your notes, tasks, wikis, and databases.](https://www.notion.so/my-integrations)

![createnotionintertaion.gif]({{site.url}}/assets/createnotionintertaion.gif)

2. 将需要自动创建页面的Notion workspace分享给Notion机器人

![sharetointegration-robot.gif]({{site.url}}/assets/sharetointegration-robot.gif)

3. 获取页面的DatabaseID

1. 通过分享链接获取

    [https://www.notion.so/485bb239e154448a85bfe8f262eaccac?v=11ef1ae0a3ba472a9c5165223b8697c3](https://www.notion.so/485bb239e154448a85bfe8f262eaccac?v=11ef1ae0a3ba472a9c5165223b8697c3) 则 DatabaseID为[485bb239-e154-448a-85bf-e8f262eaccac](https://www.notion.so/485bb239e154448a85bfe8f262eaccac?v=11ef1ae0a3ba472a9c5165223b8697c3)

![sharetointegration.gif]({{site.url}}/assets/sharetointegration.gif)

1. 通过API查询

```bash
# $NOTION_API_KEY为第一步创建Notion的secret
curl 'https://api.notion.com/v1/databases'    -H 'Authorization: Bearer '"$NOTION_API_KEY"''    -H 'Notion-Version: 2021-05-13'
```

4. 准备创建的数据

```json
 {
    "parent": { "database_id": "485bb239-e154-448a-85bf-e8f262eaccac" },
    "properties": {
        "title": {
      "title": [{ "type": "text", "text": { "content": "'"${create_date}"'"} }]
        }
    },
    "children": [
    {
      "object": "block",
      "type": "paragraph",
      "paragraph": {
        "text": [{ "type": "text", "text": { "content": "It is never too late to be what you might have been." } }]
      }
    }
  ]
}
```

5. 发送请求,创建数据

```bash
curl -X POST https://api.notion.com/v1/pages \
  -H 'Authorization: Bearer '"$NOTION_API_KEY"'' \
  -H "Content-Type: application/json" \
  -H "Notion-Version: 2021-05-13" \
  --data '{
    "parent": { "database_id": "485bb239-e154-448a-85bf-e8f262eaccac" },
    "properties": {
        "title": {
      "title": [{ "type": "text", "text": { "content": "'"${create_date}"'"} }]
        }
    },
    "children": [
    {
      "object": "block",
      "type": "paragraph",
      "paragraph": {
        "text": [{ "type": "text", "text": { "content": "It is never too late to be what you might have been." } }]
      }
    }
  ]
}'
```

6 创建定时任务

```bash
0 8 * * * cd /data/ && sh notion.sh
```

7. 问题难点

1. DatabaseID  只有为database类型的才有databaseid 
2. 通过api查询databaseid 有时候会查询不到,所以还是建议通过share链接 获取databaseid