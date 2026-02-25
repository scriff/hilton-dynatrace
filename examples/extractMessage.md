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

# *_AVS qwuery for messages_*

<code>
fetch logs //, scanLimitGBytes: 500, samplingRatio: 1000
| sort timestamp desc
| filter matchesValue(env, "qa")
| filter matchesValue(applicationci, "dzd")
//| filter matchesValue(loglevel, "INFO")
| filter contains(content, "solace")
//| filter contains(content, "solace")
//| filter contains(content, "8f2b70b8a2076de2")
| filter matchesValue(container_name, "dzd-avsfltcmd-service-qa")
| fieldsAdd Message = jsonField(content, "Message")
| fields timestamp, Message, container_id
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
