# Control Panel API Helm Chart

Installing this chart will deploy the [Analytical Platform Control Panel](https://github.com/ministryofjustice/analytics-platform-control-panel), a web
application and REST API with which the creation and maintenance of users, apps
and data sources on the platform can be managed.


## Installing the Chart

To install the chart:

```bash
helm install mojanalytics/cpanel \
  --name cpanel-$BRANCH_NAME \
  --set env.DEBUG=True \
  --set image.tag=$DOCKER_IMAGE_TAG \
  --set branch=$BRANCH_NAME
```

The instance will be available at https://controlpanel.<ServicesDomain> (
the exact URL will be shown in the helm output).


## Configuration

### Auth0 application

In Auth0 you need to install Extension 'Auth0 Authorization':

1. Login to https://manage.auth0.com/ and select the tenant for your environment
2. In the side-bar click "Extensions"
3. In the list of extensions, if "Auth0 Authorization" is not installed:
    1. Click "Auth0 Authorization" to start installation
    2. Choose "Webtask Storage"
    3. Click "Install"

### cpanel.yml

| Parameter  | Description     | Default |
| ---------- | --------------- | ------- |
| `aws.defaultRegion` | AWS region | `eu-west-1` |
| `aws.iamRole` | IAM role assumed by the instance running the API | |
| `env.DEBUG` | Used to set Django DEBUG mode | `False` |
| `env.ELASTICSEARCH_PORT` | Elasticsearch port number | `9243` |
| `env.ENABLE_K8S_RBAC` | Set to `True` if k8s RBAC is enabled | `False` |
| `env.ENV` | The environment name (`dev` or `alpha`) | |
| `env.K8S_WORKER_ROLE_NAME` | | |
| `env.LOGS_BUCKET_NAME` | | |
| `env.SAML_PROVIDER` | Name of SAML provider. Concatenated with `IAM_ARN_BASE:saml-provider/` to make an ARN | |
| `imagePullSecrets` | List of secret names containing the credentials to authenticate/pull from a docker container registry. | `[]` |
| `ingress.addTlsBlock` | Adds tls block to ingress resource. This needs to be `true` if you're using `nginx` as ingress-controller but it may need to be `false` for others (e.g. `traefik`) | `true` |
| `postgresql.postgresDatabase` | The database name where API data will be stored | |
| `postgresql.postgresHost` | The hostname of the database. Get it from terraform platform output `control_panel_api_db_host` | |
| `postgresql.postgresPassword` | The password to connect to the database with. Get it from the environment's terraform.tfvars | |
| `postgresql.postgresUser` | The username to connect to the database with | |
| `redis.scheme` | Scheme to connect to Redis cluster, can be `redis` for insecure connection or `rediss` to use SSL/TLS and encryption in-transit | `rediss` |
| `redis.host` | host for the Redis cluster, see "Primary endpoint" value in AWS EC | `""` |
| `redis.port` | Redis port | `"6379"` |
| `secretEnv.AWS_ACCOUNT_ID` | AWS account ID e.g. `123456789012`. Find this with e.g. `aws sts get-caller-identity --query Account --output text` (**DEPRECATED**) | `""` |
| `secretEnv.AWS_COMPUTE_ACCOUNT_ID` | AWS account ID where apps and tools run. | `""` |
| `secretEnv.AWS_DATA_ACCOUNT_ID` | AWS account ID where data sits. | `""` |
| `secretEnv.ELASTICSEARCH_HOST` | Elasticsearch hostname | `""` |
| `secretEnv.ELASTICSEARCH_PASSWORD` | Elasticsearch password | `""` |
| `secretEnv.ELASTICSEARCH_USERNAME` | Elasticsearch username | `""` |
| `secretEnv.NFS_HOSTNAME` | NFS hostname | |
| `secretEnv.OIDC_AUTH_EXTENSION_URL` | OIDC Auth extension url. See above for installing it. For the value, take `"https://AUTH_TENANT.eu.webtask.io/adf6e2f2b84784b57522e3b19dfc9201/api"` and replace AUTH_TENANT with your Auth0 tenant name. The rest of the URL, including the hex, is fixed e.g. `"https://gds-accelerator.eu.webtask.io/adf6e2f2b84784b57522e3b19dfc9201/api"` | `""` |
| `secretEnv.OIDC_CLIENT_ID` | Auth0 'kubectl-oidc' application's client ID | `""` |
| `secretEnv.OIDC_CLIENT_SECRET` | Auth0 'kubectl-oidc' application's client secret | `""` |
| `secretEnv.OIDC_DOMAIN` | Auth0 tenant domain e.g. `dev-analytics-moj.eu.auth0.com` | `""` |
| `secretEnv.RSTUDIO_AUTH_CLIENT_DOMAIN` | Auth0 tenant domain e.g. `dev-analytics-moj.eu.auth0.com` | `""` |
| `secretEnv.RSTUDIO_AUTH_CLIENT_ID` | Auth0 'RStudio' application's client ID (see [../rstudio/README.md]) | `""` |
| `secretEnv.RSTUDIO_AUTH_CLIENT_SECRET` | Auth0 'RStudio' application's client secret (see [../rstudio/README.md]) | `""` |
| `secretEnv.SENTRY_DSN` | Sentry credentials | |
| `secretEnv.TOOLS_DOMAIN` | Tools domain, e.g. `tools.dev.mojanalytics.xyz` | `""` |
| `servicesDomain` | DNS Domain where the app will be hosted | |
