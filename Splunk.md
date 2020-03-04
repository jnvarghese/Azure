SPLUNK

Stats and eval - major SPL in Splunk

| eval statusCategory = case(status ==200, ‘OK”, status >=400 and status <=500, “Client error”)


Calculated Fields.

| eval NewField = ResponseSize / 1024
| timechart p95(NewField)

Setting > Fields > Calculated Fields > Always select a sourceType.

http_user_agent example.
——
	Name - Client System
	Eval Expression - case(like(http_user_agent, “%iPad%”),”IPad”, like(http_user_agent”%%”),”ABC”)

Spl window -> | eval clientsystem = case(………)

==============
| eval. - command let to create new fields.
| Stats count by field 
| Top limit=15  field 
| rex - for spl
| timechart avg(ResponseSize)

dedup host | table host  - get unique after removing duplicate 
