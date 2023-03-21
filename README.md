# teleng_skills_challenge
## Input
- Using http and cURL as the input source. This allows logstash to update the config file automatically without terminating it. Makes for smoother debugging. 
- Command used to send input: 
    curl -XPUT -d '<14>1 2016-12-25T09:03:52.754646-06:00 contosohost1 antivirus 2496 - - alertname="Virus Found" computername="contosopc42" computerip="216.58.194.142" severity="1"' http://127.0.0.1:8080

## Filter
### Grok
- Using grok patterns to match desired values of the message field which is the raw input. 
- The first several fields of the message are not defined by the challenge so they are not saved. 
- The alertname field is matched with the GREEDYDATA pattern, which matches all the characters between the quotation marks. 
- Computername is matched with the HOSTNAME pattern, which matches strings resembling a computer hostname (letters, numbers, dashes).
- Computer IP is matched with IP pattern, which matches strings in IPv4 or IPv6 format. 
- Severity is matched with POSINT, which matches positive integers. 
### Mutate
- Converted the severity field from a string into an integer.
### Ruby
- This short script maps the severity field from 1, 2, or 3 to "High", "Medium", or "Low", respectively.

## Output
### Stdout
- For immediate feedback/debug purposes. 
### File
- Output file saved if needed. 

## Questions
### Grok pattern:
- Had issues matching the alertname due to the spaces. Is there a better way than the GREEDYDATA pattern? I also tried QUOTEDSTRING but that included the string literals in the field as well. 
- The DATA pattern also works the same way. What is the difference between DATA and GREEDYDATA?