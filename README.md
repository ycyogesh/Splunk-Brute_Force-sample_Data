# Splunk-Brute_Force-sample_Data

 | stats count(eval(action="success")) as successes count(eval(action="failure")) as failures by user 
| eval perc_failures_to_total = round((100*failures) / (failures + successes), 2) | sort - perc_failures_to_total failures




# Splunk-Port scanning 

| bucket _time span=1h 
| stats dc(dest_port) as num_dest_port dc(dest_ip) as num_dest_ip by src_ip, _time 
| where num_dest_port > 1000 OR num_dest_ip > 1000


index:

source="firewall_traffic.csv" host="scan" index="scanning" sourcetype="scanning_1"


Line Chart:

source="firewall_traffic.csv" host="scan" index="scanning" sourcetype="scanning_1"
 | eval _time = relative_time(_time, "@d") | multireport [ | stats count by src_ip | eval type="bysrc"] [ | stats count by dest_ip | eval type="bydest"] [ | stats count by dest_port | eval type="byport"] [ | stats count(eval(action="blocked")) as blocks by _time | eval type="blocksbytime"] | search type=blocksbytime | timechart span=1d values(blocks) as blocks | search blocks=*
 
 
 
 
 
Top Destinations:
 
 source="firewall_traffic.csv" host="scan" index="scanning" sourcetype="scanning_1"
 | eval _time = relative_time(_time, "@d") | multireport [ | stats count by src_ip | eval type="bysrc"] [ | stats count by dest_ip | eval type="bydest"] [ | stats count by dest_port | eval type="byport"] [ | stats count(eval(action="blocked")) as blocks by _time | eval type="blocksbytime"] | search type=bydest | sort - count | fields - type
 
 
Top Dest Port:
 
source="firewall_traffic.csv" host="scan" index="scanning" sourcetype="scanning_1" 
 | eval _time = relative_time(_time, "@d") | multireport [ | stats count by src_ip | eval type="bysrc"] [ | stats count by dest_ip | eval type="bydest"] [ | stats count by dest_port | eval type="byport"] [ | stats count(eval(action="blocked")) as blocks by _time | eval type="blocksbytime"] | search type=byport | sort - count | fields - type
 
 Outlier:
 
 source="firewall_traffic.csv" host="scan" index="scanning" sourcetype="scanning_1" 
 | bucket _time span=1h 
| stats dc(dest_port) as num_dest_port dc(dest_ip) as num_dest_ip by src_ip, _time 
| where num_dest_port > 1000 OR num_dest_ip > 1000  | stats count




| eval _time = relative_time(_time, "@d")
| stats count by src_ip
| search count > 0

| bucket _time span=1h 
| where dest_port!=0 OR dest_ip!=""
| stats count by src_ip, _time 
| where count > 1000


