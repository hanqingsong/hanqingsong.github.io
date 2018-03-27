---
title: EFK-pipeline使用
date: 2017-11-01 09:29:56
categories:
    - ELK
tags:
    - 日志系统
    - ELK
---

## 文件格式

```
{
  "description" : "...",
  "processors" : [ ... ]
}
```

The description is a special field to store a helpful description of what the pipeline does.
The processors parameter defines a list of processors to be executed in order.

修改字段为小写 pipeline_1.json
```
{
    "description": "Test11 pipeline",
    "processors": [
        {
            "lowercase": {
                "field": "beat.name"
            }
        }
    ]
}
```

## Ingest APIs

- Put Pipeline API to add or update a pipeline
- Get Pipeline API to return a specific pipeline
- Delete Pipeline API to delete a pipeline
- Simulate Pipeline API to simulate a call to a pipeline

## Processor

- Append Processor
- Convert Processor
- Date Processor
- Date Index Name Processor
- Fail Processor
- Foreach Processor
- Grok Processor
- Gsub Processor
- JSON Processor
- KV Processor
- Lowercase Processor
- Remove Processor
- Rename Processor
- Script Processor
- Set Processor
- Split Processor
- Sort Processor
- Trim Processor
- Uppercase Processor
- Dot Expander Processor
- URL Decode Processor

### Append Processor
```
{
  "append": {
    "field": "field1",
    "value": ["item2", "item3", "item4"]
  }
}
```

### Convert Processor
```
{
  "convert": {
    "field" : "foo",
    "type": "integer"
  }
}
```


### Date Processor
```
{
  "description" : "...",
  "processors" : [
    {
      "date" : {
        "field" : "initial_date",
        "target_field" : "timestamp",
        "formats" : ["dd/MM/yyyy hh:mm:ss"],
        "timezone" : "Europe/Amsterdam"
      }
    }
  ]
}
```

### Fail Processor
```
{
  "fail": {
    "message": "an error message"
  }
}
```