# Development Port Registry
*Keep track of all running services to avoid port conflicts*

## Currently Allocated Ports

| Port  | Service         | Project Path                | Status    | GUI URL                    | Notes                    |
|-------|-----------------|----------------------------|-----------|----------------------------|--------------------------|
| 50001 | Agent Zero      | code/AgentZero            | Planned   | http://localhost:50001     | AI Agent Framework       |
| 30007 | DISPATCH Web    | dispatch-core             | Running   | http://localhost:30007     | SAR Coordination System  |
| 8000  | DISPATCH API    | dispatch-core/backend     | Running   | http://localhost:8000      | FastAPI backend          |
| 5432  | DISPATCH DB     | dispatch-core (postgres)  | Running   | -                          | PostgreSQL + PostGIS     |
| 6379  | DISPATCH Cache  | dispatch-core (redis)     | Running   | -                          | Redis cache/pubsub       |
| 3000  | -               | -                         | Available | -                          | Common React default     |
| 8080  | -               | -                         | Available | -                          | Common alt HTTP          |
| 11434 | Ollama          | Local Installation        | Available | -                          | Local LLM models         |

## Port Allocation Rules

### **Reserved Ranges**
- **50001-50099**: Agent Zero and AI frameworks
- **60001-60099**: Database GUIs (pgAdmin, Redis Commander, etc.)
- **70001-70099**: Monitoring tools (Grafana, Prometheus, etc.)
- **80001-80099**: Custom applications

### **Standard Defaults to Avoid**
- 3000 (React/Next.js)
- 8000 (FastAPI/Django)
- 5000 (Flask)
- 8080 (Tomcat/Alt HTTP)
- 3306 (MySQL)
- 5432 (PostgreSQL)
- 6379 (Redis)
- 27017 (MongoDB)

### **Assignment Process**
1. Check this registry before starting new service
2. Update table when allocating new port
3. Mark status: `Planned` → `Running` → `Stopped`
4. Update GUI URL when service starts

## Quick Commands

```bash
# Check what's running on ports
netstat -tulpn | grep LISTEN

# Check specific port
lsof -i :50001

# Kill service on port
sudo fuser -k 50001/tcp
```

## Project Templates

### Docker Compose Port Template
```yaml
services:
  app:
    ports:
      - "${PORT:-XXXX}:80"  # Use .env PORT or default XXXX
  db:
    ports:
      - "${DB_PORT:-YYYY}:5432"
```

### .env Port Section Template
```bash
# Project: [PROJECT_NAME]
PORT=XXXX
DB_PORT=YYYY
REDIS_PORT=ZZZZ
```