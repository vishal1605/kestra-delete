# Data Migration with Kestra

Automated data migration workflows using Kestra orchestration platform. Sync data from MSSQL to Frappe ERP across multiple environments.

## 🎯 Project Overview

This project uses Kestra to:
- Extract data from MSSQL databases
- Transform and validate records
- Load data into Frappe via REST API
- Support incremental and full migrations
- Run scheduled syncs across dev/stage/prod environments

## 🚀 Quick Start

### Prerequisites
- Docker & Docker Compose
- Git
- Access to MSSQL and Frappe instances

### Setup (For Managers)

1. **Clone repository**
```bash
   git clone https://github.com/vishal1605/kestra-delete
   cd kestra-delete
```

2. **Create credentials file**
```bash
   cp credentials.env.template credentials.env
   nano credentials.env
   # Fill in base64-encoded secrets
```

3. **Set permissions**
```bash
   chmod 600 credentials.env
```

4. **Start Kestra**
```bash
   docker-compose up -d
```

5. **Access UI**
```
   http://localhost:8081
```

## 🔐 Security

- All secrets must use `SECRET_` prefix
- Values must be base64 encoded
- `credentials.env` is never committed to Git
- Only managers have access to production secrets

### Encoding Secrets
```bash
echo -n "your_secret_value" | base64
```

## 📁 Project Structure
```
.
├── docker-compose.yml          # Kestra configuration
├── credentials.env.template    # Secret template (commit this)
├── credentials.env            # Real secrets (DO NOT COMMIT)
├── .gitignore                 # Excludes secrets from Git
└── flows/                     # Kestra workflow definitions
    ├── mssql-to-frappe.yaml
    ├── sync-employees.yaml
    └── verify-secrets.yaml
```

## 🔄 Workflow

### For Developers
1. Create/edit flows in `flows/` directory
2. Test locally with dev credentials
3. Commit and push changes
```bash
   git add flows/
   git commit -m "Updated migration flow"
   git push
```

### For Managers
1. Pull latest changes
```bash
   git pull
```
2. Restart Kestra to load new flows
```bash
   docker-compose restart kestra
```

## 🌍 Environments

Flows support multiple environments via input selection:
- **dev** - Local development
- **stage** - Staging server
- **prod** - Production server

Use `{{secret(inputs.environment | upper ~ '_MSSQL_PASSWORD')}}` pattern in flows.

## 📊 Available Flows

- **mssql-to-frappe-sync** - Main migration workflow
- **verify-secrets** - Test secret configuration
- **test-api** - Verify API connectivity

## 🛠️ Troubleshooting

**Secrets not loading?**
```bash
docker-compose down
docker-compose up -d
```

**Permission denied?**
```bash
chmod 600 credentials.env
```

## 📝 License

Internal use only - Ashida Electronics Pvt Ltd

## 👥 Contributors

- Manager Team - Secret management & deployment
- Developer Team - Flow development & testing
