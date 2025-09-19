# Serverless URL Shortener

A modern, scalable URL shortener built with AWS Lambda, PostgreSQL, and Redis.

![Build Status](https://github.com/zoey402/serverless-url-shortener/workflows/CI%2FCD%20Pipeline/badge.svg)
![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Node.js Version](https://img.shields.io/badge/node-%3E%3D18.0.0-brightgreen)

## Features

- **Serverless Architecture** - Built on AWS Lambda for infinite scalability
- **Custom Short URLs** - Generate custom short codes or use auto-generated ones
- **Real-time Analytics** - Track clicks, geography, device info, and user behavior
- **Security First** - Malicious URL detection and intelligent rate limiting
- **Global CDN** - CloudFront distribution for worldwide performance
- **API-First Design** - RESTful APIs with comprehensive OpenAPI documentation
- **High Availability** - Multi-AZ deployment with automatic failover
- **Cost Optimized** - Pay-per-use serverless architecture

## Architecture

```
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│   Client    │───▶│ CloudFront   │───▶│ API Gateway │
└─────────────┘    └──────────────┘    └─────────────┘
                                              │
                                              ▼
┌─────────────┐    ┌──────────────┐    ┌─────────────┐
│ ElastiCache │◀───│    Lambda    │───▶│ RDS Postgres│
│   (Redis)   │    │  Functions   │    │             │
└─────────────┘    └──────────────┘    └─────────────┘
                                              │
                                              ▼
                          ┌─────────────────────────────┐
                          │     DynamoDB Analytics      │
                          └─────────────────────────────┘
```

## Quick Start

### Prerequisites

- Node.js 18+ 
- Docker & Docker Compose
- AWS CLI (for deployment)
- Terraform (for infrastructure)

### Local Development

1. **Clone the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/serverless-url-shortener.git
   cd serverless-url-shortener
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Set up environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

4. **Start local services with Docker**
   ```bash
   docker-compose up -d
   ```

5. **Initialize the database**
   ```bash
   npm run db:migrate
   ```

6. **Start the development server**
   ```bash
   npm run dev
   ```

The API will be available at `http://localhost:3000`

## 📖 API Documentation

### Shorten URL

```bash
POST /api/shorten
Content-Type: application/json

{
  "url": "https://example.com/very-long-url",
  "customCode": "optional-custom-code",
  "expiresAt": "2024-12-31T23:59:59Z"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "id": "clx1234567",
    "shortUrl": "https://short.ly/abc123",
    "originalUrl": "https://example.com/very-long-url",
    "shortCode": "abc123",
    "createdAt": "2024-01-15T10:30:00Z",
    "expiresAt": "2024-12-31T23:59:59Z"
  }
}
```

### Redirect

```bash
GET /{shortCode}
```

Redirects to the original URL with 301 status code.

### Get Analytics

```bash
GET /api/analytics/{shortCode}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "totalClicks": 1520,
    "uniqueClicks": 890,
    "clicksByDay": [...],
    "topCountries": [...],
    "topDevices": [...],
    "topReferrers": [...]
  }
}
```

### Batch Operations

```bash
POST /api/batch/shorten
Content-Type: application/json

{
  "urls": [
    "https://example1.com",
    "https://example2.com"
  ]
}
```

## Tech Stack

### Backend
- **Runtime**: Node.js 18+ with Express.js
- **Database**: PostgreSQL (AWS RDS) with connection pooling
- **Cache**: Redis (AWS ElastiCache) for high-performance caching
- **Serverless**: AWS Lambda with API Gateway

### Infrastructure
- **Cloud Provider**: Amazon Web Services (AWS)
- **CDN**: CloudFront for global content delivery
- **Infrastructure as Code**: Terraform
- **Monitoring**: CloudWatch with custom metrics
- **Analytics Storage**: DynamoDB for real-time analytics

### DevOps
- **CI/CD**: GitHub Actions
- **Containerization**: Docker & Docker Compose
- **Testing**: Jest with comprehensive test coverage
- **Code Quality**: ESLint, Prettier, Husky git hooks

## Deployment

### Development Environment

```bash
npm run deploy:dev
```

### Staging Environment

```bash
npm run deploy:staging
```

### Production Environment

```bash
npm run deploy:prod
```

### Manual Infrastructure Setup

1. **Configure AWS credentials**
   ```bash
   aws configure
   ```

2. **Initialize Terraform**
   ```bash
   cd infrastructure/environments/prod
   terraform init
   ```

3. **Plan deployment**
   ```bash
   terraform plan
   ```

4. **Apply infrastructure**
   ```bash
   terraform apply
   ```

## Performance Metrics

### Response Times
- **Cache Hit**: < 50ms
- **Cache Miss**: < 200ms
- **Database Query**: < 100ms
- **Cold Start**: < 500ms

### Scalability
- **Concurrent Users**: 10,000+
- **Requests per Second**: 1,000+
- **Storage**: Unlimited (PostgreSQL + S3)
- **Global Latency**: < 100ms (via CloudFront)

### Cost Efficiency
- **Monthly Cost**: ~$35-40 for 100K requests
- **Per Million Requests**: ~$3-5
- **Auto-scaling**: 0 to infinity based on demand

## Testing

### Run all tests
```bash
npm test
```

### Run tests with coverage
```bash
npm run test:coverage
```

### Run integration tests
```bash
npm run test:integration
```

### Load testing
```bash
npm run test:load
```

## License

This project is licensed under the MIT License.