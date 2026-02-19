# PMOVES.AI Integration Guide for Surf

## Integration Complete

The PMOVES.AI integration template has been applied to Surf.

## Next Steps

### 1. Customize Environment Variables

Edit the following files with your service-specific values:

- `env.shared` - Base environment configuration
- `env.tier-agent` - AGENT tier specific configuration
- `chit/secrets_manifest_v2.yaml` - Add your service's required secrets

### 2. Update Docker Compose

Add the PMOVES.AI environment anchor to your `docker-compose.yml`:

```yaml
services:
  surf:
    <<: [*env-tier-agent, *pmoves-healthcheck]
    # Your existing service configuration...
```

### 3. Integrate Health Check

Add the health check endpoint to your service:

```python
from pmoves_health import add_custom_check, get_health_status

@app.get("/healthz")
async def health_check():
    return await get_health_status()
```

### 4. Add Service Announcement

Add NATS service announcement to your startup:

```python
from pmoves_announcer import announce_service

@app.on_event("startup")
async def startup():
    await announce_service(
        slug="surf",
        name="Surf Computer Use Agent",
        url=f"http://surf:3000",
        port=3000,
        tier="agent"
    )
```

### 5. Test Integration

```bash
# Test health check
curl http://localhost:3000/healthz

# Verify environment variables loaded
docker compose exec surf env | grep PMOVES
```

## Service Details

- **Name:** Surf Computer Use Agent
- **Slug:** surf
- **Tier:** agent
- **Port:** 3000 (web UI)
- **Health Check:** http://localhost:3000/healthz
- **NATS Enabled:** False
- **GPU Enabled:** False

## Support

For questions or issues, see the PMOVES.AI documentation.
