---
title: "Cw_logs_conf"
date: 2018-10-25T16:20:37+09:00
draft: false
---
## CloudWatch Logs設定サンプル
```
[general]
state_file = /var/lib/awslogs/agent-state
use_gzip_http_content_encoding = true

[/var/log/messages]
file = /var/log/messages
log_group_name = /var/log/messages
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[/var/log/cron]
file = /var/log/cron
log_group_name = /var/log/cron
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[/var/log/maillog]
file = /var/log/maillog
log_group_name = /var/log/maillog
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[/var/log/secure]
file = /var/log/secure
log_group_name = /var/log/secure
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[/var/log/yum.log]
file = /var/log/yum.log
log_group_name = /var/log/yum.log
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[/var/log/cloud-init.log]
file = /var/log/cloud-init.log
log_group_name = /var/log/cloud-init.log
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[/var/log/awslogs.log]
file = /var/log/awslogs.log
log_group_name = /var/log/awslogs.log
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[/var/log/nginx/access.log]
file = /var/log/nginx/access.log
log_group_name = /var/log/nginx/access.log
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[/var/log/nginx/error.log]
file = /var/log/nginx/error.log
log_group_name = /var/log/nginx/error.log
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[command_history_ec2_user]
file = /home/ec2-user/.bash_history
log_group_name = command_history_ec2_user
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S

[command_history_root]
file = /root/.bash_history
log_group_name = command_history_root
log_stream_name = {instance_id}
datetime_format = %Y-%m-%d %H:%M:%S
```