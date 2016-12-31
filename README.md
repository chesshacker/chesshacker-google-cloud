[![Build Status](https://travis-ci.org/chesshacker/chesshacker-infrastructure.svg?branch=master)](https://travis-ci.org/chesshacker/chesshacker-infrastructure)

# ChessHacker Infrastructure

Google Cloud Platform Infrastructure for chesshacker.com.

Project ID: chesshacker-primary

Travis-CI will take care of a lot, but there was some manual setup:

* A service account was setup for travis-ci on chesshacker-primary, and its
  client-secret was saved and encrypted in this repo.

* To encrypt and decrypt files, a shared secret was chosen. This can be passed
  as an environment variable (super_secret_password) or loaded from a text file
  (super-secret-password.txt). The shared password was saved as an encrypted
  global variable (`travis encrypt super_secret_password=XXX --add`).

* Enable Deployment Manager and Cloud DNS on the project:

```
gcloud service-management enable deploymentmanager-json.googleapis.com
gcloud service-management enable dns.googleapis.com
```

* The deployment was created manually. Travis-CI will automatically keep it
  updated once it has been initally created.

```
gcloud deployment-manager deployments create deployment --config=deployment.yml
```

* Google App Engine was manually setup. The app itself will take care of updates,
  configuration and deployment. All I needed to do was pick a region, us-central1.

One aspect of Google Cloud that isn't maintainable through deployment-manager is
DNS records. I keep the zone file in this repo, but I haven't found a great way
to only update it when the zone file changes. So, for now, updates to the
record set are saved here but manually deployed:

```
gcloud dns record-sets import --delete-all-existing --zone=chesshacker --zone-file-format chesshacker.zone
```
