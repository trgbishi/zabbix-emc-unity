# zabbix-emc-unity
Python script for monitoring EMC Unity storages


For succesfull work of script need installed python3, zabbix-sender.

In template "Template EMC Unity REST-API" in section "Macros" need set these macros:
- {$API_USER}
- {$API_PASSWORD}
- {$API_PORT}
- {$SUBSCRIBED_PERCENT}
- {$USED_PERCENT}

In agent configuration file, **/etc/zabbix/zabbix_agentd.conf** must be set parameter **ServerActive=xxx.xxx.xxx.xxx**

Scirpt must be copied to zabbix-server or zabbix-proxy, depending on what the torage is being monitored.


- In Linux-console on zabbix-server or zabbix-proxy need run this command to make discovery. Script must return value 0 in case of success.
```bash
./unity_get_state.py --api_ip=xxx.xxx.xxx.xxx --api_port=443 --api_user=username_on_storagedevice --api_password='password' --storage_name="storage-name_in_zabbix" --discovery
```
- On zabbix proxy or on zabbix servers need run **zabbix_proxy -R config_cache_reload** (zabbix_server -R config_cache_reload)

- In Linux-console on zabbix-server or zabbix-proxy need run this command to get value of metrics. Scripts must return value 0 in case of success.
```bash
./unity_get_stateNEW.py --api_ip=xxx.xxx.xxx.xxx --api_port=443 --api_user=username_on_storagedevice --api_password='password' --storage_name="storage-name_in_zabbix" --status
```
If you have executed this script from console from user root or from another user, please check access permission on file **/tmp/unity_state.log**. It must be allow read, write to user zabbix.

- Return code 1 or 2 is zabbix_sender return code. Read here - https://www.zabbix.com/documentation/4.4/manpages/zabbix_sender



1. 需要安装python3，并且将python3的执行文件链接到/usr/bin/python
    安装参考https://www.jianshu.com/p/e191f9dc1186，需要网络
2. unity_get_state.py 放到/usr/lib/zabbix/externalscripts目录下，并赋予权限
    chown -R zabbix:zabbix unity_get_state.py 
    chmod 777 unity_get_state.py 
    vi => :set ff=unix
3. 给日志文件赋权
    chown -R zabbix:zabbix unity_state.log


ps: 虽然模板写的是unity400，经测试480也支持