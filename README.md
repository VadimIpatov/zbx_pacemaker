# Zabbix Pacemaker Template
A Zabbix template for pacemaker cluster monitoring with virtual ip.

Author: Vadim Ipatov <<vadim.ipatov@zabbix.com>> (<euphoria.vi@gmail.com>)

## Requires
* Zabbix >=3.4 (because the template uses [dependent items](https://www.zabbix.com/documentation/3.4/manual/config/items/itemtypes/dependent_items) and [value preprocessing](https://www.zabbix.com/documentation/3.4/manual/config/items/item#item_value_preprocessing) features that were introduced in 3.4)

## Metrics
| Metric                              | Description                                                                             |
|-------------------------------------|-----------------------------------------------------------------------------------------|
| pacemaker.cluster.dc                | Current cluster DC (Designated Coordinator)                                             |
| pacemaker.cluster.failed_actions    | The current number of failed actions                                                    |
| pacemaker.cluster.maintenance       | Shows if the cluster is in the maintenance state                                        |
| pacemaker.nodes.offline             | The current number of offline (or in the shutdown state) nodes                          |
| pacemaker.process.corosync.active   | Shows if corosync daemon is in the running state                                        |
| pacemaker.process.pacemakerd.active | Shows if pacemakerd daemon is in the running state                                      |
| pacemaker.resources.failed          | The current number of failed (non active or blocked) resources                          |
| pacemaker.status                    | Service item for data gathering                                                         |
| system.hostname                     | Node hostname. If it changes, it could mean that the VIP has been moved to another node |

## Installation
You need to configure every cluster nodes as shown below:
* Copy "configs/pacemaker.conf" into your zabbix_agent include folder (default: /etc/zabbix/zabbix_agentd.d/) or manually add that UserParameter to config:

``UserParameter=pacemaker.status, sudo /usr/sbin/crm_mon --as-xml``

* Restart zabbix_agent

* Copy "configs/zbx_pacemaker.sudoers" into /etc/sudoers.d or manually add that rule:

``zabbix	ALL=NOPASSWD: /usr/sbin/crm_mon --as-xml``

* Import "template_app_pacemaker.xml" into zabbix as template
* Create "virtual" host object with virtual ip address as network interface and link this template.

## Testing
You can check that everything works correctly by using crm_mon dumps in “tests” folder. 

For this purpose you need to copy dumps to cluster nodes (or use your own) and add this UserParameter to config:

``UserParameter=pacemaker.test[*], cat /etc/zabbix/zabbix_agentd.d/tests/$1``

Then change "pacemaker.status" item key to "pacemaker.test[sampleN.xml]" and save.

Also you could manually edit sample file for test different cluster states.