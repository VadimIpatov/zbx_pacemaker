# Zabbix Pacemaker Template
A Zabbix template for pacemaker cluster monitoring with virtual ip
Author: Vadim Ipatov <<vadim.ipatov@zabbix.com>> (<euphoria.vi@gmail.com>)

## Requires
* Zabbix >=3.4

## Installation
You need to configure every cluster nodes as shown below:
1. Copy "configs/pacemaker.conf" into your zabbix_agent include folder (default: /etc/zabbix/zabbix_agentd.d/) or manually add that UserParameter to config:
``UserParameter=pacemaker.status, sudo /usr/sbin/crm_mon --as-xml``
2. Copy "configs/zbx_pacemaker.sudoers" into /etc/sudoers.d or manually add that rule:
``zabbix	ALL=NOPASSWD: /usr/sbin/crm_mon --as-xml``
3. Import "template_app_pacemaker.xml" into zabbix as template
4. Create "virtual" host object with virtual ip address as network interface and link this template.

## Testing
You can check that everything working correctly using crm_mon dumps in “tests” folder. For this purpose you need to copy dumps to cluster nodes (or use your own) and add this UserParameter to config:
``UserParameter=pacemaker.test[*], cat /etc/zabbix/zabbix_agentd.d/tests/$1``
Then change "pacemaker.status" item key to "pacemaker.test[sampleN.xml]" and save.
Also you could manually edit sample file for test different cluster states.
