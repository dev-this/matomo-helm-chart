# Matomo - Kubernetes Helm Chart

Chart is a work in progress.

For now only the Kubernetes resources are available.

## How to use these K8s resources
- Clone this repository
- Open up `deployment.yaml`
- Adjust the container matomo image version as desired
- Modify `secret/matomo-db` with your database details
- Change service to a LoadBalancer if desired
  - Optionally use an Ingress/IngressRoute
- `kubectl apply -f deployment.yaml`

### Setting up DB-IP Geolocation
https://matomo.org/faq/how-to/faq_163/
- Login to Matomo as an admin. 
- Visit Administration > Geolocation (under System)
- In your local (non k8s) terminal run
```
# update to latest year/month in URI
wget https://download.db-ip.com/free/dbip-city-lite-2020-08.mmdb.gz`
gunzip dbip-city-lite-2020-08.mmdb.gz
export MATOMO_POD=$(kubectl get pods -o=name -l=app=matomo | cut -c 5-)
kubectl cp dbip-city-lite-2020-08.mmdb $MATOMO_POD:/var/www/html/misc/DBIP-City-Lite.mmdb
```
- Refresh Geolocation page
- Enable "DBIP / GeoIP 2 (Php)" Location provider
