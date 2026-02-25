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

# *AVS qwuery for messages_*

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
