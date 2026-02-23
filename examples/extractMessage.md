# *_Query for messages_*

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
