# Honeycomb Setup Guide

## Why Honeycomb?

Honeycomb is perfect for debugging performance issues in your MCP server because it:
- Auto-instruments Express, PostgreSQL, HTTP requests, and external APIs (TMDB)
- Shows you the full trace of each request through your system
- Makes it easy to find slow database queries or API calls
- Has excellent support for distributed tracing and OpenTelemetry

## Setup Steps

### 1. Create a Honeycomb Account
1. Go to [honeycomb.io](https://honeycomb.io)
2. Sign up for a free account (no credit card required)
3. The free tier includes 20M events/month - more than enough for this demo

### 2. Get Your API Key
1. After signing up, go to your Team Settings
2. Click on "API Keys" in the left sidebar
3. Copy your API key (or create a new one)

### 3. Add to Your Environment

**For local development:**
```bash
# Add to backend/.env
HONEYCOMB_API_KEY=your_api_key_here
```

**For Render deployment:**
1. Go to your service in Render dashboard
2. Navigate to "Environment" tab
3. Add environment variable:
   - Key: `HONEYCOMB_API_KEY`
   - Value: your API key
4. Save and redeploy

### 4. Deploy and Test
Once deployed with the API key, Honeycomb will automatically:
- ✅ Trace all incoming HTTP requests (except health checks)
- ✅ Trace all PostgreSQL database queries
- ✅ Trace all outgoing HTTP requests (to TMDB API)
- ✅ Show you the full request waterfall

## Using Honeycomb

### View Traces
1. Go to Honeycomb dashboard
2. Select the `movie-context-provider` dataset
3. You'll see all traces automatically

### Find Slow Requests
1. Click on "Query" tab
2. Visualize by `duration_ms` 
3. Sort by P99 to find the slowest requests
4. Click on any trace to see the full waterfall

### Example Queries

**Find slow database queries:**
- Filter: `db.system = postgresql`
- Group by: `db.statement`
- Visualize: P95(duration_ms)

**Find slow TMDB API calls:**
- Filter: `http.url CONTAINS themoviedb.org`
- Visualize: AVG(duration_ms)

**See full MCP tool execution:**
- Filter: `http.target = /mcp/messages`
- Click any trace to see the full request flow

## What Gets Traced

✅ **Automatically traced:**
- HTTP requests (GET, POST to /mcp/messages, etc.)
- Database queries (SELECT, INSERT, UPDATE)
- External API calls (TMDB searches, movie details)
- DNS lookups, TCP connections

❌ **Not traced (to reduce noise):**
- Health check requests (`/health`)
- File system operations

## No Code Changes Required

The beauty of Honeycomb with OpenTelemetry is that it auto-instruments your code. You don't need to add any manual spans or logging - just set the API key and it works!

## Need Help?

Honeycomb has excellent docs at [docs.honeycomb.io](https://docs.honeycomb.io)

