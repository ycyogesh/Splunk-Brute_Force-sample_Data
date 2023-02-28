# Splunk-Brute_Force-sample_Data

 | stats count(eval(action="success")) as successes count(eval(action="failure")) as failures by user 
| eval perc_failures_to_total = round((100*failures) / (failures + successes), 2) | sort - perc_failures_to_total failures


| bucket _time span=1h 
| stats dc(dest_port) as num_dest_port dc(dest_ip) as num_dest_ip by src_ip, _time 
| where num_dest_port > 1000 OR num_dest_ip > 1000
