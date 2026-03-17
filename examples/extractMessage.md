# *_Recap Query for messages_*

<code>


fetch logs //, scanLimitGBytes: 500, samplingRatio: 1000
| sort timestamp desc
| filter matchesValue(env, "dev")
| filter matchesValue(applicationci, "dzd")
//| filter matchesValue(loglevel, "INFO")
| filter contains(content, "Time taken")
//| filter contains(content, "8f2b70b8a2076de2")
| filter matchesValue(container_name, "dzd-recapcmd-service-dev")
| fieldsAdd Message = jsonField(content, "Message")
| fields Message
</code>

# *_AVS query for messages_*

<code>
fetch logs //, scanLimitGBytes: 500, samplingRatio: 1000
| filter matchesValue(env, "qa")
| filter matchesValue(applicationci, "dzd")
//| filter matchesValue(loglevel, "INFO")
//| filter contains(content, "unhealthy")
//| filter contains(content, "solace")
//| filter matchesValue(container_id, "5038e5b99370467da513097221010142-1930243009")
| filter not contains(content, "Response Log") and 
	     not contains(content, "Request Log") and 
	     not contains(content, "DRAIN_MODE: Rejecting") and
	     not contains(content, "Time taken to publish flight data patch message to UDH") and
	     not contains(content, "Queue depth healthy")
| filter matchesValue(container_name, "dzd-avsfltcmd-service-qa")
| fieldsAdd Message = jsonField(content, "Message")
| fields timestamp, content, container_id
| sort timestamp desc

</code>


# *_Devops_*

<code>
fetch logs /
fetch logs //, scanLimitGBytes: 500, samplingRatio: 1000
| filter contains(applicationci,"dzd") //and matchesphrase(container_name,"indraw") //and not matchesPhrase(content,"] info    [")
| filter container_name == "dzd-avsfltcmd-service-stg"
//| filter loglevel == "INFO"
| filter aws.region == "us-east-1"
|  filter contains(content, "degraded")
//| filter loglevel == "NONE"
| fieldsadd ename = toarray(container_name)
| lookup [ fetch dt.entity.service, from:-60m ], sourceField: ename, lookupField:applicationName, fields:{ applicationReleaseVersion}
| fieldsRemove ename
</code>

# *_Solace Issues_*
<code>
fetch logs, scanLimitGBytes: 1500 //, samplingRatio: 1000
| filter matchesValue(env, "stg")
| filter matchesValue(applicationci, "dzd")
//|  filter contains(content, "common.solace")
|  filter contains(content, "SOLCLIENT") 
|  filer contains(content, "session_disconnected_event") 
|  filter contains(content, "Connection has been idle for")
//| filter matchesValue(container_name, "dzd-schedmaint-adapter-stg")
//|  filter matchesValue(container_id, "2b1ccd5082d946b49f4464db8cee39d9-1254064001")
| filter matchesValue(container_name, "dzd-avsrequest-adapter-stg")
</code>
