# DNS Record Sets

Unfortunately, Deployment Manager does not yet support Record Sets for Managed DNS Zones.
Until it does, or there's a better solution in place, management of records is *manual.*
Update the `chesshacker-zone.yml` file, then run the following to replace all the records
except the NS and SOA records.

```
gcloud dns record-sets import --delete-all-existing --zone=chesshacker chesshacker-zone.yml
```
