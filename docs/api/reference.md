# MegaReach API Reference

Complete API documentation for integrating MegaReach features into your applications.

## Overview

The MegaReach API provides programmatic access to all platform features including ReplyGuy AI, MegaBoost, analytics, and blockchain integration.

### Base URL
```
Production: https://api.megareach.xyz/v1
Testnet: https://testnet-api.megareach.xyz/v1
```

### Authentication

All API requests require authentication using an API key:

```bash
curl -H "Authorization: Bearer YOUR_API_KEY" \
     https://api.megareach.xyz/v1/user
```

### Rate Limits

| Plan | Requests/Hour | Burst Limit | Concurrent |
|------|--------------|-------------|------------|
| Free | 100 | 10/min | 1 |
| Starter | 1,000 | 100/min | 5 |
| Growth | 5,000 | 500/min | 10 |
| Pro | 20,000 | 2,000/min | 50 |
| Enterprise | Unlimited | Custom | Custom |

## Authentication

### Getting API Keys

1. Log into MegaReach Dashboard
2. Navigate to Settings → API
3. Click "Generate New Key"
4. Store securely (shown only once)

### API Key Security

```javascript
// Good - Use environment variables
const apiKey = process.env.MEGAREACH_API_KEY;

// Bad - Never hardcode keys
const apiKey = "sk_live_abc123...";
```

### Request Headers

```http
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json
X-Request-ID: unique-request-id
```

## Core Endpoints

### User Management

#### Get User Profile
```http
GET /user
```

Response:
```json
{
  "id": "user_123",
  "email": "user@example.com",
  "plan": "growth",
  "credits": {
    "replies": 450,
    "boosts": 5800
  },
  "connected_accounts": [
    {
      "platform": "twitter",
      "username": "@user",
      "followers": 10000
    }
  ]
}
```

#### Update User Settings
```http
PATCH /user/settings
```

Request:
```json
{
  "notifications": {
    "email": true,
    "sms": false,
    "push": true
  },
  "timezone": "America/New_York"
}
```

### ReplyGuy AI

#### Generate Reply
```http
POST /replyguy/generate
```

Request:
```json
{
  "tweet_id": "1234567890",
  "tweet_text": "What's the best AI tool for social media?",
  "voice": "professional",
  "max_length": 280,
  "include_hashtags": true
}
```

Response:
```json
{
  "reply_id": "reply_456",
  "text": "Great question! MegaReach combines AI-powered engagement with automated boosting. Our ReplyGuy AI understands context and maintains your authentic voice while scaling interactions. #AITools #SocialMediaGrowth",
  "confidence": 0.92,
  "estimated_engagement": 85
}
```

#### Configure ReplyGuy
```http
POST /replyguy/config
```

Request:
```json
{
  "enabled": true,
  "frequency": {
    "min": 2,
    "max": 10,
    "unit": "hour"
  },
  "voice_settings": {
    "tone": "friendly",
    "formality": 0.6,
    "humor": 0.3
  },
  "filters": {
    "min_followers": 100,
    "languages": ["en", "es"],
    "exclude_keywords": ["politics", "spam"]
  }
}
```

#### Get Reply History
```http
GET /replyguy/history?limit=50&offset=0
```

Response:
```json
{
  "replies": [
    {
      "id": "reply_789",
      "tweet_id": "1234567890",
      "text": "Thanks for sharing!",
      "timestamp": "2024-12-01T10:30:00Z",
      "engagement": {
        "likes": 15,
        "replies": 3
      }
    }
  ],
  "total": 1250,
  "has_more": true
}
```

### MegaBoost

#### Apply Boost
```http
POST /boost/apply
```

Request:
```json
{
  "tweet_id": "1234567890",
  "multiplier": 5,
  "strategy": "maximum_reach",
  "target_impressions": 10000,
  "schedule": {
    "start": "2024-12-01T12:00:00Z",
    "duration_hours": 24
  }
}
```

Response:
```json
{
  "boost_id": "boost_321",
  "status": "active",
  "credits_used": 50,
  "estimated_reach": 12000,
  "end_time": "2024-12-02T12:00:00Z"
}
```

#### Get Boost Status
```http
GET /boost/{boost_id}
```

Response:
```json
{
  "boost_id": "boost_321",
  "status": "completed",
  "metrics": {
    "impressions": 13542,
    "engagements": 892,
    "clicks": 234
  },
  "credits_used": 50,
  "roi": 2.7
}
```

#### Boost Analytics
```http
GET /boost/analytics?period=7d
```

Response:
```json
{
  "period": "7d",
  "total_boosts": 45,
  "credits_consumed": 2250,
  "metrics": {
    "total_impressions": 567890,
    "total_engagements": 34567,
    "avg_boost_performance": 1.8,
    "best_performing": {
      "tweet_id": "1234567890",
      "impressions": 45678
    }
  }
}
```

### Monitoring & Analytics

#### Create Monitor
```http
POST /monitor/create
```

Request:
```json
{
  "name": "Brand Mentions",
  "keywords": ["@brand", "#brand", "brand product"],
  "platforms": ["twitter", "reddit"],
  "actions": {
    "auto_reply": true,
    "alert": true,
    "add_to_crm": true
  }
}
```

#### Get Monitoring Results
```http
GET /monitor/{monitor_id}/results
```

Response:
```json
{
  "monitor_id": "mon_456",
  "results": [
    {
      "id": "result_789",
      "platform": "twitter",
      "content": "Love the new @brand product!",
      "author": "@customer",
      "sentiment": 0.9,
      "reach": 5000,
      "action_taken": "auto_replied"
    }
  ]
}
```

#### Analytics Dashboard
```http
GET /analytics/dashboard?period=30d
```

Response:
```json
{
  "period": "30d",
  "growth": {
    "followers": {
      "current": 15000,
      "change": 3500,
      "percent": 30.4
    }
  },
  "engagement": {
    "rate": 5.2,
    "total_interactions": 45678,
    "avg_per_post": 234
  },
  "top_content": [
    {
      "id": "1234567890",
      "impressions": 100000,
      "engagement_rate": 8.5
    }
  ]
}
```

### Workflows

#### Create Workflow
```http
POST /workflows/create
```

Request:
```json
{
  "name": "Customer Support Flow",
  "trigger": {
    "type": "mention",
    "conditions": {
      "sentiment": "< 0.5",
      "contains": ["help", "support", "problem"]
    }
  },
  "actions": [
    {
      "type": "reply",
      "template": "support_response"
    },
    {
      "type": "notify",
      "channel": "slack",
      "team": "support"
    },
    {
      "type": "tag",
      "tag": "needs_attention"
    }
  ]
}
```

#### Execute Workflow
```http
POST /workflows/{workflow_id}/execute
```

Request:
```json
{
  "context": {
    "tweet_id": "1234567890",
    "user": "@customer",
    "sentiment": 0.3
  }
}
```

### Blockchain Integration

#### Get Token Balance
```http
GET /blockchain/balance/{address}
```

Response:
```json
{
  "address": "0x123...",
  "balance": {
    "mega": "10000.50",
    "staked": "5000.00",
    "pending_rewards": "125.75"
  }
}
```

#### Claim Rewards
```http
POST /blockchain/rewards/claim
```

Request:
```json
{
  "address": "0x123...",
  "signature": "0xabc..."
}
```

Response:
```json
{
  "transaction_hash": "0xdef...",
  "amount": "125.75",
  "status": "pending"
}
```

#### Stake Tokens
```http
POST /blockchain/stake
```

Request:
```json
{
  "amount": "1000",
  "duration_days": 90,
  "address": "0x123...",
  "signature": "0xabc..."
}
```

## Webhooks

### Setting Up Webhooks

```http
POST /webhooks/create
```

Request:
```json
{
  "url": "https://your-app.com/webhook",
  "events": [
    "reply.posted",
    "boost.completed",
    "follower.new",
    "mention.detected"
  ],
  "secret": "webhook_secret_key"
}
```

### Webhook Events

#### Reply Posted
```json
{
  "event": "reply.posted",
  "timestamp": "2024-12-01T10:30:00Z",
  "data": {
    "reply_id": "reply_123",
    "tweet_id": "1234567890",
    "text": "Great point!",
    "url": "https://twitter.com/..."
  }
}
```

#### Boost Completed
```json
{
  "event": "boost.completed",
  "timestamp": "2024-12-01T12:00:00Z",
  "data": {
    "boost_id": "boost_456",
    "tweet_id": "1234567890",
    "impressions": 25000,
    "engagements": 1500
  }
}
```

### Webhook Security

Verify webhook signatures:

```javascript
const crypto = require('crypto');

function verifyWebhook(payload, signature, secret) {
  const hash = crypto
    .createHmac('sha256', secret)
    .update(payload)
    .digest('hex');
  
  return hash === signature;
}
```

## Error Handling

### Error Response Format

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests",
    "details": {
      "limit": 1000,
      "reset_at": "2024-12-01T11:00:00Z"
    }
  }
}
```

### Common Error Codes

| Code | HTTP Status | Description |
|------|------------|-------------|
| INVALID_API_KEY | 401 | Invalid or expired API key |
| RATE_LIMIT_EXCEEDED | 429 | Too many requests |
| INSUFFICIENT_CREDITS | 402 | Not enough credits |
| RESOURCE_NOT_FOUND | 404 | Resource doesn't exist |
| VALIDATION_ERROR | 400 | Invalid request parameters |
| INTERNAL_ERROR | 500 | Server error |

## SDKs & Libraries

### Official SDKs

#### JavaScript/Node.js
```bash
npm install @megareach/sdk
```

```javascript
const MegaReach = require('@megareach/sdk');
const client = new MegaReach('YOUR_API_KEY');

// Generate reply
const reply = await client.replyguy.generate({
  tweetId: '1234567890',
  voice: 'professional'
});
```

#### Python
```bash
pip install megareach
```

```python
from megareach import MegaReach
client = MegaReach('YOUR_API_KEY')

# Apply boost
boost = client.boost.apply(
    tweet_id='1234567890',
    multiplier=5
)
```

#### Go
```bash
go get github.com/megareach/go-sdk
```

```go
import "github.com/megareach/go-sdk"

client := megareach.NewClient("YOUR_API_KEY")
reply, err := client.ReplyGuy.Generate(
    megareach.GenerateOptions{
        TweetID: "1234567890",
    },
)
```

## Testing

### Test Environment

Use testnet for development:
- Base URL: `https://testnet-api.megareach.xyz/v1`
- Test API Key: Request from dashboard
- Test Credits: 10,000 monthly
- Data Reset: Weekly

### Postman Collection

Download our Postman collection:
[MegaReach API Collection](https://postman.com/megareach-api)

### Example Requests

```bash
# Test authentication
curl -X GET \
  https://testnet-api.megareach.xyz/v1/user \
  -H "Authorization: Bearer TEST_API_KEY"

# Test reply generation
curl -X POST \
  https://testnet-api.megareach.xyz/v1/replyguy/generate \
  -H "Authorization: Bearer TEST_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "tweet_id": "test_123",
    "voice": "friendly"
  }'
```

## Best Practices

### Performance

1. **Use pagination** for large datasets
2. **Cache responses** when appropriate
3. **Batch requests** when possible
4. **Use webhooks** instead of polling

### Security

1. **Never expose API keys** in client code
2. **Use HTTPS** for all requests
3. **Validate webhook signatures**
4. **Implement rate limiting** in your app

### Error Handling

1. **Implement exponential backoff** for retries
2. **Log all errors** for debugging
3. **Handle rate limits** gracefully
4. **Provide user feedback** for failures

## Support

### Resources

- API Status: [status.megareach.xyz](https://status.megareach.xyz)
- GitHub: [github.com/megareach/api](https://github.com/megareach/api)
- Changelog: [api.megareach.xyz/changelog](https://api.megareach.xyz/changelog)

### Contact

- Email: api@megareach.xyz
- Discord: [#api-support](https://discord.gg/megareach)
- Twitter: [@MegaReachAPI](https://twitter.com/megareachapi)

---

*API Version: v1.0.0 | Last Updated: December 2024*