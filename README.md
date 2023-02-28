# Splunk-Brute_Force-sample_Data

 | stats count(eval(action="success")) as successes count(eval(action="failure")) as failures by user 
| eval perc_failures_to_total = round((100*failures) / (failures + successes), 2) | sort - perc_failures_to_total failures
