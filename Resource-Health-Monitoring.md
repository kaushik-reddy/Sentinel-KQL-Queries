Resource Health Monitoring in Sentinel(3rd Party Integrations)

Description - This query will monitor the health status of the 3rd party service integrations with sentinel.

KQL Query:

let Devices = dynamic(["Add your Devices/computers list here"]);
let MultiDataSources =
(union isfuzzy=true
(
Heartbeat
| summarize max(TimeGenerated) by Computer
| where Computer in (Devices)
| where max_TimeGenerated < ago(1h)
),
(
CommonSecurityLog
| summarize Last_Log = datetime_diff("second", now(), max(TimeGenerated)) by Computer
| where Computer in (Devices)
| where Last_Log > 3600 // 1hr log check mentioned in seconds
)
);
let notreportingdevices = MultiDataSources;
notreportingdevices

Note:

Multiple log sources/Tables can be added to the current query based on the requirements.
Environment based tuning is reuqired to reduce the false alerts.
