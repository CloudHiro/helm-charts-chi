# Tokto Helm Chart

## Installation

Add the Tokto repository:
```bash
helm repo add tokto https://cloudhiro.github.io/helm-charts-chi/tokto
```

Update the repository:
```bash
helm repo update tokto
```

Install/upgrade the chart:
```bash
helm upgrade -n mlops2 --create-namespace tokto --install \
  tokto/tokto
```

## Configuration

The chart can be configured using a values file. Example:
```bash
# Using a custom values file
helm upgrade -n mlops2 --create-namespace tokto --install \
  -f /path/to/your/values.yaml \
  tokto/tokto
```

**Note**: Replace `/path/to/your/values.yaml` with the actual path to your values file.

## Self-Hosting on Kubernetes

By default, MySQL is deployed as part of the Helm installation. However, if you prefer to use an external MySQL database, you can disable the built-in MySQL and configure the external connection.

### Using Built-in MySQL
For self-hosted deployments with the default MySQL, configure the following essential variables in your values file:

```yaml
# MySQL database credentials
secrets:
  mysql:
    password: aGreatPassword  # Replace with a secure password
    user: mlops

# Domain configuration
global:
  frontendDomain: "tokto.yourdomain.com"    # Replace with your frontend domain
  backendDomain: "tokto-be.yourdomain.com"  # Replace with your backend domain
```

You can apply these settings using a values file or directly via helm command:

```bash
helm upgrade -n mlops2 --create-namespace tokto --install \
  --set secrets.mysql.password=aGreatePassword \
  --set secrets.mysql.user=mlops \
  --set global.frontendDomain=tokto.yourdomain.com \
  --set global.backendDomain=tokto-be.yourdomain.com \
  tokto/tokto
```

**Note**: For production deployments, ensure you use secure passwords and configure appropriate domain names.

### Using External MySQL
To use an external MySQL database, disable the built-in MySQL and configure the connection settings:

```yaml
# Disable built-in MySQL
mysql:
  enabled: false

# External database configuration
global:
  env:
    SPRING_DATASOURCE_URL: "jdbc:mysql://<your-mysql-host>:3306/<your-database>?allowPublicKeyRetrieval=true&useSSL=false"
    SPRING_DATABASE_URL: "jdbc:mysql://<your-mysql-host>:3306/<your-database>?allowPublicKeyRetrieval=true&useSSL=false"
    DB_HOST_PORT: "<your-mysql-host>:3306"
```

**Important**: 
- Replace `<your-mysql-host>` with your database host
- Replace `<your-database>` with your database name
- Ensure your database is accessible from the Kubernetes cluster
- Configure appropriate security groups and network access

### Ingress Configuration
To expose your services through an AWS Application Load Balancer (ALB), you can use the following ingress configuration:

```yaml
ingress:
  enabled: true
  className: alb
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
    alb.ingress.kubernetes.io/listen-ports: '[{"HTTP": 80}, {"HTTPS":443}]'
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    alb.ingress.kubernetes.io/ssl-redirect: '443'
    alb.ingress.kubernetes.io/healthcheck-path: /ping
```

This configuration:
- Enables ALB ingress controller
- Sets up both HTTP (80) and HTTPS (443) listeners
- Configures SSL redirect to HTTPS
- Uses IP target type for the ALB
- Defines a health check endpoint at `/ping`
- Makes the ALB internet-facing

**Note**: Ensure you have the AWS Load Balancer Controller installed in your cluster before enabling the ALB ingress.

## Support

For support or questions, please contact your account manager or visit [CloudHiro support](https://cloudhiro.com/support).
