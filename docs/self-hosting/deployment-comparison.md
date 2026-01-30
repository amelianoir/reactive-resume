# Reactive Resume - Deployment Options Comparison

This guide helps you choose the best deployment method for Reactive Resume.

## Quick Comparison

| Feature | Docker | Vercel + Supabase |
|---------|--------|-------------------|
| **Setup Complexity** | Medium | Easy |
| **Infrastructure Management** | Self-managed | Managed |
| **Cost (small usage)** | $5-10/month VPS | Free tier available |
| **Scaling** | Manual | Automatic |
| **Database** | Self-hosted PostgreSQL | Supabase managed PostgreSQL |
| **Storage** | Local or S3-compatible | Supabase Storage (S3-compatible) |
| **PDF Generation** | Included (headless Chrome) | External service required |
| **Best For** | Full control, privacy, custom infrastructure | Quick start, serverless, minimal maintenance |

## Docker Deployment

### ✅ Choose Docker if you:
- Want full control over your infrastructure
- Have a VPS or dedicated server
- Need maximum privacy and data sovereignty
- Want to avoid external dependencies
- Require custom configurations or modifications
- Have experience with Docker and server management

### 📦 Requirements:
- VPS or dedicated server (2GB+ RAM recommended)
- Docker and Docker Compose installed
- Domain name (optional, can use IP)
- Basic Linux/server administration knowledge

### 💰 Estimated Monthly Cost:
- **VPS**: $5-20/month (DigitalOcean, Linode, etc.)
- **Domain**: $10-15/year (optional)
- **Total**: ~$5-20/month

### 📚 Documentation:
- [Docker Self-Hosting Guide](./docker.mdx)
- [Docker Compose Examples](./examples.mdx)

## Vercel + Supabase Deployment

### ✅ Choose Vercel + Supabase if you:
- Want the quickest setup (< 30 minutes)
- Prefer managed infrastructure
- Don't want to manage servers
- Need automatic scaling
- Want to start with free tier
- Are comfortable with external dependencies

### 📦 Requirements:
- Vercel account (free tier available)
- Supabase account (free tier available)
- Browserless account or alternative (for PDF generation)
- GitHub/GitLab repository
- Basic understanding of environment variables

### 💰 Estimated Monthly Cost:

**Free Tier (Hobby/Personal):**
- **Vercel**: $0 (100GB bandwidth, 6K build minutes)
- **Supabase**: $0 (500MB DB, 1GB storage)
- **Browserless**: $0 (6 hours/month)
- **Total**: $0/month (with usage limits)

**Paid Tier (Production):**
- **Vercel Pro**: $20/month (better limits, 60s function timeout)
- **Supabase Pro**: $25/month (8GB DB, 100GB storage)
- **Browserless**: $30-100+/month (unlimited usage)
- **Total**: $75-145+/month

### 📚 Documentation:
- [Vercel + Supabase Guide](./vercel-supabase.mdx)
- [Slovak Quick Start](./README-SK.md) (Slovenský návod)

## Decision Flow

```
Do you have a VPS/server?
├─ Yes ──> Want full control? 
│          ├─ Yes ──> Docker ✓
│          └─ No ──> Either works
└─ No ──> Want to manage infrastructure?
           ├─ Yes ──> Get VPS, then Docker
           └─ No ──> Vercel + Supabase ✓

Do you need maximum privacy?
├─ Yes ──> Docker (self-hosted) ✓
└─ No ──> Either works

Budget < $10/month?
├─ Yes ──> Vercel + Supabase (free tier) ✓
└─ No ──> Either works

Want fastest setup?
├─ Yes ──> Vercel + Supabase ✓
└─ No ──> Either works

Heavy PDF generation usage?
├─ Yes ──> Docker (included) or Vercel Pro ✓
└─ No ──> Either works
```

## Feature Comparison

### Database
| Feature | Docker | Vercel + Supabase |
|---------|--------|-------------------|
| Type | Self-hosted PostgreSQL | Managed PostgreSQL |
| Backups | Manual | Automatic (Pro plan) |
| Scaling | Manual | Automatic |
| Performance | Depends on server | Optimized by Supabase |
| Connection Pooling | Manual setup | Built-in |

### Storage
| Feature | Docker | Vercel + Supabase |
|---------|--------|-------------------|
| Type | Local filesystem or S3 | Supabase Storage (S3-compatible) |
| Capacity | Limited by server disk | 1GB free, unlimited paid |
| CDN | Manual setup | Built-in |
| Access Control | Manual | Built-in RLS |

### PDF Generation
| Feature | Docker | Vercel + Supabase |
|---------|--------|-------------------|
| Service | Included (Browserless/Chrome) | External (Browserless Cloud) |
| Cost | Included in VPS | $0-100+/month |
| Reliability | Depends on server | High (managed service) |
| Scaling | Limited by server | Automatic |

### Deployment
| Feature | Docker | Vercel + Supabase |
|---------|--------|-------------------|
| Setup Time | 30-60 minutes | 15-30 minutes |
| Updates | Manual (`docker compose pull`) | Automatic (Git push) |
| Rollbacks | Manual | One-click |
| CI/CD | Manual setup | Built-in |
| Preview Environments | Manual | Automatic |

## Hybrid Approach

You can also mix deployment methods:

### Option A: Docker with External Services
- **App**: Self-hosted Docker
- **Database**: Supabase (managed)
- **Storage**: Supabase Storage or S3
- **Printer**: Browserless Cloud

**Benefits**: 
- Full control over app
- Managed database and storage
- Easier maintenance

### Option B: Vercel with Self-hosted Database
- **App**: Vercel
- **Database**: Self-hosted PostgreSQL
- **Storage**: Self-hosted S3 or Supabase
- **Printer**: Browserless Cloud

**Benefits**:
- Automatic app scaling
- Full control over data
- More complex networking

## Migration Path

### From Docker to Vercel + Supabase:
1. Export database: `pg_dump $DATABASE_URL > backup.sql`
2. Create Supabase project
3. Import data to Supabase
4. Upload files to Supabase Storage
5. Deploy to Vercel with Supabase credentials

### From Vercel + Supabase to Docker:
1. Export Supabase database
2. Set up Docker environment
3. Import database to self-hosted PostgreSQL
4. Download files from Supabase Storage
5. Configure Docker with local storage

## Recommendations

### For Personal Use:
- **Start with**: Vercel + Supabase (free tier)
- **Why**: Zero cost, minimal maintenance, easy setup
- **Upgrade when**: You exceed free tier limits

### For Small Teams (< 10 users):
- **Recommended**: Vercel + Supabase (free or paid)
- **Why**: Automatic scaling, managed infrastructure, low maintenance
- **Alternative**: Docker on budget VPS ($5-10/month)

### For Medium Organizations (10-100 users):
- **Recommended**: Docker on good VPS or cloud server
- **Why**: Better cost efficiency, more control, predictable costs
- **Alternative**: Vercel Pro + Supabase Pro

### For Large Organizations (100+ users):
- **Recommended**: Docker on dedicated infrastructure or Kubernetes
- **Why**: Maximum control, best performance, cost efficiency at scale
- **Consider**: Enterprise support from Vercel/Supabase if needed

### For Maximum Privacy/Security:
- **Recommended**: Docker on self-hosted infrastructure
- **Why**: Complete control over data, no external dependencies
- **Considerations**: Higher maintenance burden, need security expertise

### For Quick Prototyping/Testing:
- **Recommended**: Vercel + Supabase (free tier)
- **Why**: Fastest setup, zero cost, easy to tear down
- **Upgrade to**: Production setup when needed

## Getting Help

- **Docker**: [Docker Guide](./docker.mdx) | [Examples](./examples.mdx)
- **Vercel + Supabase**: [Deployment Guide](./vercel-supabase.mdx) | [Slovak Guide](./README-SK.md)
- **General**: [GitHub Issues](https://github.com/amruthpillai/reactive-resume/issues) | [Discord](https://discord.gg/hzwkZbyvUW)

---

**Still not sure?** Start with Vercel + Supabase free tier. You can always migrate to Docker later if needed.
