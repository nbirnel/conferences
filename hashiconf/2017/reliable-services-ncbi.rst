National Center for Biotechnology Information

GenBank
PubMed
PubChem
Blast
PMC

blue/green deployment
routing table:
/service/foo =>/versionsed/foo-139

canary deployment:
/service/foo => 2 * /versioned/foo-139 & 8 * /versioned/foo-140

open container inititative for standards, but for now:
it's Docker. 

"Service Mesh" - linkerd, envoy by lyft, lstio

chromatica/grafana, telegraf, influxdb, kapacitor

telegraf -> kapacitor
influxdb -> grafana



todorov@ncbi.nlm.nih.gov
