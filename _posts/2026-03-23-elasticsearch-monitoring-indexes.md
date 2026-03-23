---
layout: post
title: ElasticSearch - monitoring index management
date: 2026-03-23
categories: elasticsearch kibana
---

1. You have enabled self monitoring in Kibana?
2. Now you have daily generated indexes like:
```
.monitoring-es-7-2026.03.23
.monitoring-es-7-2026.03.22
...
.monitoring-kibana-7-2026.03.23
.monitoring-kibana-7-2026.03.22
```
3. By default these indexes are stored 7 days
4. Probably, you want to store this data only for 2 days:
```bash
PUT _cluster/settings
{
  "persistent": {
    "xpack.monitoring.history.duration": "2d"
  }
}
```
5. By default metrics are recorded every 10 seconds
6. Probably, you want to reduce index size and collect data every minute:
```bash
PUT _cluster/settings
{
  "persistent": {
    "xpack.monitoring.collection.interval": "60s"
  }
}
```
7. Probably, you want to follow official recommendation and setup `metricbeat` ?
