# 🏗️ Technical Architecture Guide

This document outlines the current and future technical architecture of Boutique-To-Box, providing detailed insights into system design, scalability patterns, and implementation strategies.

## 📋 Table of Contents
- [Current Architecture](#current-architecture)
- [Target Architecture](#target-architecture)
- [System Components](#system-components)
- [Data Architecture](#data-architecture)
- [Security Architecture](#security-architecture)
- [Scalability Patterns](#scalability-patterns)
- [Deployment Architecture](#deployment-architecture)
- [Monitoring & Observability](#monitoring--observability)

---

## 🏛️ Current Architecture

### High-Level Overview
```mermaid
graph TB
    subgraph "Client Layer"
        A[React Web App]
        B[Mobile Browser]
    end
    
    subgraph "API Layer"
        C[Express.js Server]
        D[Appwrite Backend]
    end
    
    subgraph "External Services"
        E[RunwayML API]
        F[OpenAI GPT-4]
        G[Printful API]
        H[Mapbox API]
    end
    
    subgraph "Data Layer"
        I[Appwrite Database]
        J[File Storage]
    end
    
    A --> C
    B --> C
    C --> D
    C --> E
    C --> F
    C --> G
    C --> H
    D --> I
    D --> J
```

### Current Tech Stack
- **Frontend**: React 18 + TypeScript + Vite + Tailwind CSS
- **Backend**: Node.js + Express.js + Appwrite
- **Database**: Appwrite (NoSQL)
- **Authentication**: Appwrite Auth
- **File Storage**: Appwrite Storage
- **Deployment**: Vercel (Frontend) + Railway (Backend)

---

## 🎯 Target Architecture (Phase 4)

### Microservices Architecture
```mermaid
graph TB
    subgraph "Client Applications"
        A[React Web App]
        B[React Native Mobile]
        C[Desktop App]
        D[Admin Dashboard]
    end
    
    subgraph "API Gateway Layer"
        E[Kong API Gateway]
        F[Load Balancer]
    end
    
    subgraph "Core Services"
        G[User Service]
        H[Design Service]
        I[AI/ML Service]
        J[Manufacturing Service]
        K[Payment Service]
        L[Notification Service]
    end
    
    subgraph "Data Services"
        M[User Database]
        N[Design Database]
        O[Analytics Database]
        P[File Storage]
        Q[Cache Layer]
    end
    
    subgraph "External Integrations"
        R[AI Providers]
        S[Manufacturing APIs]
        T[Payment Gateways]
        U[Social Platforms]
    end
    
    subgraph "Infrastructure"
        V[Message Queue]
        W[Background Jobs]
        X[Monitoring]
        Y[Logging]
    end
    
    A --> E
    B --> E
    C --> E
    D --> E
    E --> F
    F --> G
    F --> H
    F --> I
    F --> J
    F --> K
    F --> L
    
    G --> M
    H --> N
    I --> O
    J --> P
    K --> Q
    
    I --> R
    J --> S
    K --> T
    L --> U
    
    V --> W
    X --> Y
```

---

## 🔧 System Components

### 1. Frontend Architecture

#### Web Application (React)
```typescript
// Component Architecture
src/
├── components/          # Reusable UI components
│   ├── ui/             # Base UI components (shadcn/ui)
│   ├── forms/          # Form components
│   ├── layout/         # Layout components
│   └── features/       # Feature-specific components
├── pages/              # Page components
├── hooks/              # Custom React hooks
├── services/           # API services
├── stores/             # State management (Zustand/Redux)
├── utils/              # Utility functions
└── types/              # TypeScript definitions
```

#### State Management Strategy
```typescript
// Zustand Store Example
interface DesignStore {
  designs: Design[];
  currentDesign: Design | null;
  isLoading: boolean;
  
  // Actions
  fetchDesigns: () => Promise<void>;
  createDesign: (data: CreateDesignData) => Promise<Design>;
  updateDesign: (id: string, data: UpdateDesignData) => Promise<Design>;
  deleteDesign: (id: string) => Promise<void>;
}

const useDesignStore = create<DesignStore>((set, get) => ({
  designs: [],
  currentDesign: null,
  isLoading: false,
  
  fetchDesigns: async () => {
    set({ isLoading: true });
    try {
      const designs = await designService.getAll();
      set({ designs, isLoading: false });
    } catch (error) {
      set({ isLoading: false });
      throw error;
    }
  },
  
  // ... other actions
}));
```

#### Mobile Application (React Native)
```typescript
// React Native Architecture
src/
├── components/         # Shared components
├── screens/           # Screen components
├── navigation/        # Navigation configuration
├── services/          # API services
├── hooks/             # Custom hooks
├── utils/             # Utility functions
├── assets/            # Images, fonts, etc.
└── types/             # TypeScript definitions
```

### 2. Backend Architecture

#### Microservices Design
```typescript
// Service Structure
services/
├── user-service/
│   ├── src/
│   │   ├── controllers/
│   │   ├── services/
│   │   ├── models/
│   │   ├── middleware/
│   │   └── routes/
│   ├── tests/
│   ├── Dockerfile
│   └── package.json
├── design-service/
├── ai-service/
├── manufacturing-service/
└── notification-service/
```

#### API Gateway Configuration
```yaml
# Kong Gateway Configuration
services:
  - name: user-service
    url: http://user-service:3001
    routes:
      - name: user-routes
        paths: ["/api/users", "/api/auth"]
        
  - name: design-service
    url: http://design-service:3002
    routes:
      - name: design-routes
        paths: ["/api/designs"]
        
plugins:
  - name: rate-limiting
    config:
      minute: 100
      hour: 1000
      
  - name: jwt
    config:
      secret_is_base64: false
      key_claim_name: kid
```

### 3. AI/ML Service Architecture

#### Model Management Pipeline
```python
# AI Service Structure
ai-service/
├── models/
│   ├── style_transfer/
│   ├── pattern_generation/
│   ├── trend_prediction/
│   └── quality_assessment/
├── pipelines/
│   ├── training/
│   ├── inference/
│   └── evaluation/
├── api/
│   ├── endpoints/
│   ├── middleware/
│   └── schemas/
└── utils/
    ├── preprocessing/
    ├── postprocessing/
    └── monitoring/
```

#### Model Serving Infrastructure
```yaml
# Kubernetes Deployment for AI Service
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ai-service
spec:
  replicas: 3
  selector:
    matchLabels:
      app: ai-service
  template:
    metadata:
      labels:
        app: ai-service
    spec:
      containers:
      - name: ai-service
        image: boutique-to-box/ai-service:latest
        ports:
        - containerPort: 8000
        resources:
          requests:
            memory: "2Gi"
            cpu: "1000m"
            nvidia.com/gpu: 1
          limits:
            memory: "4Gi"
            cpu: "2000m"
            nvidia.com/gpu: 1
        env:
        - name: MODEL_PATH
          value: "/models"
        volumeMounts:
        - name: model-storage
          mountPath: /models
      volumes:
      - name: model-storage
        persistentVolumeClaim:
          claimName: model-pvc
```

---

## 🗄️ Data Architecture

### Database Design Strategy

#### Multi-Database Approach
```mermaid
graph TB
    subgraph "Operational Databases"
        A[PostgreSQL - User Data]
        B[MongoDB - Design Data]
        C[Redis - Cache & Sessions]
    end
    
    subgraph "Analytics Databases"
        D[ClickHouse - Events]
        E[Elasticsearch - Search]
        F[InfluxDB - Metrics]
    end
    
    subgraph "Data Pipeline"
        G[Apache Kafka]
        H[Apache Spark]
        I[Data Warehouse]
    end
    
    A --> G
    B --> G
    C --> G
    G --> H
    H --> D
    H --> E
    H --> F
    H --> I
```

#### Data Models

##### User Service Schema
```sql
-- PostgreSQL Schema
CREATE TABLE users (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    name VARCHAR(255) NOT NULL,
    avatar_url TEXT,
    subscription_tier VARCHAR(50) DEFAULT 'free',
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE user_profiles (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID REFERENCES users(id) ON DELETE CASCADE,
    bio TEXT,
    website VARCHAR(255),
    social_links JSONB,
    preferences JSONB,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_user_profiles_user_id ON user_profiles(user_id);
```

##### Design Service Schema
```javascript
// MongoDB Schema
const designSchema = new mongoose.Schema({
  _id: { type: mongoose.Schema.Types.ObjectId, auto: true },
  userId: { type: String, required: true, index: true },
  title: { type: String, required: true },
  description: { type: String },
  prompt: { type: String, required: true },
  
  // Design data
  imageUrl: { type: String, required: true },
  modelUrl: { type: String },
  thumbnailUrl: { type: String },
  
  // Metadata
  tags: [{ type: String, index: true }],
  category: { type: String, required: true, index: true },
  style: { type: String, required: true },
  colors: [{ type: String }],
  
  // AI generation data
  aiModel: { type: String, required: true },
  generationParams: { type: mongoose.Schema.Types.Mixed },
  confidence: { type: Number, min: 0, max: 1 },
  
  // Social features
  isPublic: { type: Boolean, default: false },
  likes: { type: Number, default: 0 },
  views: { type: Number, default: 0 },
  
  // Timestamps
  createdAt: { type: Date, default: Date.now, index: true },
  updatedAt: { type: Date, default: Date.now }
});

// Compound indexes for efficient queries
designSchema.index({ userId: 1, createdAt: -1 });
designSchema.index({ category: 1, isPublic: 1, createdAt: -1 });
designSchema.index({ tags: 1, isPublic: 1 });
```

### Data Pipeline Architecture

#### Event-Driven Data Flow
```typescript
// Event Schema
interface DesignEvent {
  eventId: string;
  eventType: 'design_created' | 'design_updated' | 'design_viewed' | 'design_liked';
  userId: string;
  designId: string;
  timestamp: Date;
  metadata: Record<string, any>;
}

// Event Publisher
class EventPublisher {
  private kafka: Kafka;
  
  constructor() {
    this.kafka = new Kafka({
      clientId: 'boutique-to-box',
      brokers: ['kafka:9092']
    });
  }
  
  async publishDesignEvent(event: DesignEvent): Promise<void> {
    const producer = this.kafka.producer();
    await producer.connect();
    
    await producer.send({
      topic: 'design-events',
      messages: [{
        key: event.designId,
        value: JSON.stringify(event),
        timestamp: event.timestamp.getTime().toString()
      }]
    });
    
    await producer.disconnect();
  }
}
```

---

## 🔐 Security Architecture

### Authentication & Authorization

#### JWT-Based Authentication
```typescript
// JWT Service
class JWTService {
  private readonly secret: string;
  private readonly issuer: string;
  
  constructor() {
    this.secret = process.env.JWT_SECRET!;
    this.issuer = 'boutique-to-box';
  }
  
  generateToken(payload: TokenPayload): string {
    return jwt.sign(payload, this.secret, {
      issuer: this.issuer,
      expiresIn: '24h',
      algorithm: 'HS256'
    });
  }
  
  verifyToken(token: string): TokenPayload {
    return jwt.verify(token, this.secret, {
      issuer: this.issuer,
      algorithms: ['HS256']
    }) as TokenPayload;
  }
}

// RBAC Implementation
interface Permission {
  resource: string;
  action: string;
}

interface Role {
  name: string;
  permissions: Permission[];
}

class RBACService {
  private roles: Map<string, Role> = new Map();
  
  constructor() {
    this.initializeRoles();
  }
  
  private initializeRoles(): void {
    this.roles.set('user', {
      name: 'user',
      permissions: [
        { resource: 'design', action: 'create' },
        { resource: 'design', action: 'read' },
        { resource: 'design', action: 'update' },
        { resource: 'design', action: 'delete' }
      ]
    });
    
    this.roles.set('admin', {
      name: 'admin',
      permissions: [
        { resource: '*', action: '*' }
      ]
    });
  }
  
  hasPermission(userRole: string, resource: string, action: string): boolean {
    const role = this.roles.get(userRole);
    if (!role) return false;
    
    return role.permissions.some(permission => 
      (permission.resource === '*' || permission.resource === resource) &&
      (permission.action === '*' || permission.action === action)
    );
  }
}
```

### API Security

#### Rate Limiting & DDoS Protection
```typescript
// Rate Limiting Middleware
import rateLimit from 'express-rate-limit';
import RedisStore from 'rate-limit-redis';
import Redis from 'ioredis';

const redis = new Redis(process.env.REDIS_URL);

export const createRateLimit = (windowMs: number, max: number) => {
  return rateLimit({
    store: new RedisStore({
      sendCommand: (...args: string[]) => redis.call(...args),
    }),
    windowMs,
    max,
    message: {
      error: 'Too many requests',
      retryAfter: Math.ceil(windowMs / 1000)
    },
    standardHeaders: true,
    legacyHeaders: false,
  });
};

// Different limits for different endpoints
export const authLimiter = createRateLimit(15 * 60 * 1000, 5); // 5 requests per 15 minutes
export const apiLimiter = createRateLimit(15 * 60 * 1000, 100); // 100 requests per 15 minutes
export const designLimiter = createRateLimit(60 * 60 * 1000, 10); // 10 designs per hour
```

#### Input Validation & Sanitization
```typescript
// Validation Schemas with Zod
import { z } from 'zod';

export const createDesignSchema = z.object({
  title: z.string().min(1).max(100).trim(),
  description: z.string().max(500).optional(),
  prompt: z.string().min(10).max(1000).trim(),
  category: z.enum(['dress', 'shirt', 'pants', 'accessories']),
  style: z.enum(['casual', 'formal', 'sporty', 'vintage']),
  colors: z.array(z.string().regex(/^#[0-9A-F]{6}$/i)).max(5),
  isPublic: z.boolean().default(false)
});

// Validation Middleware
export const validateRequest = (schema: z.ZodSchema) => {
  return (req: Request, res: Response, next: NextFunction) => {
    try {
      const validated = schema.parse(req.body);
      req.body = validated;
      next();
    } catch (error) {
      if (error instanceof z.ZodError) {
        return res.status(400).json({
          error: 'Validation failed',
          details: error.errors
        });
      }
      next(error);
    }
  };
};
```

---

## ⚡ Scalability Patterns

### Horizontal Scaling Strategy

#### Load Balancing Configuration
```nginx
# Nginx Load Balancer
upstream api_servers {
    least_conn;
    server api-1:3000 weight=3;
    server api-2:3000 weight=3;
    server api-3:3000 weight=2;
    
    # Health checks
    keepalive 32;
}

server {
    listen 80;
    server_name api.boutique-to-box.com;
    
    location / {
        proxy_pass http://api_servers;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # Connection pooling
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        
        # Timeouts
        proxy_connect_timeout 5s;
        proxy_send_timeout 10s;
        proxy_read_timeout 10s;
    }
}
```

#### Database Scaling

##### Read Replicas Setup
```typescript
// Database Connection Pool
import { Pool } from 'pg';

class DatabaseManager {
  private writePool: Pool;
  private readPools: Pool[];
  private currentReadIndex: number = 0;
  
  constructor() {
    // Write connection (master)
    this.writePool = new Pool({
      host: process.env.DB_WRITE_HOST,
      port: parseInt(process.env.DB_PORT || '5432'),
      database: process.env.DB_NAME,
      user: process.env.DB_USER,
      password: process.env.DB_PASSWORD,
      max: 20,
      idleTimeoutMillis: 30000,
      connectionTimeoutMillis: 2000,
    });
    
    // Read connections (replicas)
    this.readPools = [
      new Pool({ host: process.env.DB_READ_HOST_1, /* ... other config */ }),
      new Pool({ host: process.env.DB_READ_HOST_2, /* ... other config */ }),
    ];
  }
  
  getWriteConnection(): Pool {
    return this.writePool;
  }
  
  getReadConnection(): Pool {
    // Round-robin load balancing
    const pool = this.readPools[this.currentReadIndex];
    this.currentReadIndex = (this.currentReadIndex + 1) % this.readPools.length;
    return pool;
  }
}
```

##### Caching Strategy
```typescript
// Multi-Level Caching
class CacheManager {
  private redis: Redis;
  private localCache: NodeCache;
  
  constructor() {
    this.redis = new Redis(process.env.REDIS_URL);
    this.localCache = new NodeCache({ 
      stdTTL: 300, // 5 minutes
      checkperiod: 60 
    });
  }
  
  async get<T>(key: string): Promise<T | null> {
    // L1: Local cache
    const localValue = this.localCache.get<T>(key);
    if (localValue) {
      return localValue;
    }
    
    // L2: Redis cache
    const redisValue = await this.redis.get(key);
    if (redisValue) {
      const parsed = JSON.parse(redisValue) as T;
      this.localCache.set(key, parsed);
      return parsed;
    }
    
    return null;
  }
  
  async set<T>(key: string, value: T, ttl: number = 3600): Promise<void> {
    // Set in both caches
    this.localCache.set(key, value, ttl);
    await this.redis.setex(key, ttl, JSON.stringify(value));
  }
  
  async invalidate(key: string): Promise<void> {
    this.localCache.del(key);
    await this.redis.del(key);
  }
}
```

### Auto-Scaling Configuration

#### Kubernetes HPA
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: api-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: api-deployment
  minReplicas: 3
  maxReplicas: 50
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
  - type: Resource
    resource:
      name: memory
      target:
        type: Utilization
        averageUtilization: 80
  behavior:
    scaleUp:
      stabilizationWindowSeconds: 60
      policies:
      - type: Percent
        value: 100
        periodSeconds: 15
    scaleDown:
      stabilizationWindowSeconds: 300
      policies:
      - type: Percent
        value: 10
        periodSeconds: 60
```

---

## 🚀 Deployment Architecture

### Container Strategy

#### Multi-Stage Dockerfile
```dockerfile
# Frontend Dockerfile
FROM node:18-alpine AS builder

WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

FROM nginx:alpine AS production
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/nginx.conf

EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

#### Docker Compose for Development
```yaml
version: '3.8'

services:
  frontend:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - VITE_API_URL=http://localhost:3001
    depends_on:
      - backend

  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile.dev
    ports:
      - "3001:3001"
    volumes:
      - ./backend:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgresql://user:password@postgres:5432/boutique_to_box
      - REDIS_URL=redis://redis:6379
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:15-alpine
    environment:
      - POSTGRES_DB=boutique_to_box
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

### Kubernetes Production Deployment

#### Application Deployment
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: boutique-to-box-api
  labels:
    app: boutique-to-box-api
spec:
  replicas: 3
  selector:
    matchLabels:
      app: boutique-to-box-api
  template:
    metadata:
      labels:
        app: boutique-to-box-api
    spec:
      containers:
      - name: api
        image: boutique-to-box/api:latest
        ports:
        - containerPort: 3001
        env:
        - name: NODE_ENV
          value: "production"
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: url
        - name: REDIS_URL
          valueFrom:
            secretKeyRef:
              name: redis-secret
              key: url
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        livenessProbe:
          httpGet:
            path: /health
            port: 3001
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /ready
            port: 3001
          initialDelaySeconds: 5
          periodSeconds: 5
---
apiVersion: v1
kind: Service
metadata:
  name: boutique-to-box-api-service
spec:
  selector:
    app: boutique-to-box-api
  ports:
  - protocol: TCP
    port: 80
    targetPort: 3001
  type: ClusterIP
```

---

## 📊 Monitoring & Observability

### Metrics Collection

#### Prometheus Configuration
```yaml
# prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

rule_files:
  - "alert_rules.yml"

scrape_configs:
  - job_name: 'boutique-to-box-api'
    static_configs:
      - targets: ['api:3001']
    metrics_path: /metrics
    scrape_interval: 10s

  - job_name: 'boutique-to-box-frontend'
    static_configs:
      - targets: ['frontend:3000']
    metrics_path: /metrics

alerting:
  alertmanagers:
    - static_configs:
        - targets:
          - alertmanager:9093
```

#### Custom Metrics Implementation
```typescript
// Metrics Service
import { register, Counter, Histogram, Gauge } from 'prom-client';

class MetricsService {
  private httpRequestsTotal: Counter<string>;
  private httpRequestDuration: Histogram<string>;
  private activeConnections: Gauge<string>;
  private designGenerationTime: Histogram<string>;
  
  constructor() {
    this.httpRequestsTotal = new Counter({
      name: 'http_requests_total',
      help: 'Total number of HTTP requests',
      labelNames: ['method', 'route', 'status_code']
    });
    
    this.httpRequestDuration = new Histogram({
      name: 'http_request_duration_seconds',
      help: 'Duration of HTTP requests in seconds',
      labelNames: ['method', 'route'],
      buckets: [0.1, 0.5, 1, 2, 5, 10]
    });
    
    this.activeConnections = new Gauge({
      name: 'active_connections',
      help: 'Number of active connections'
    });
    
    this.designGenerationTime = new Histogram({
      name: 'design_generation_duration_seconds',
      help: 'Time taken to generate a design',
      buckets: [1, 5, 10, 30, 60, 120]
    });
    
    register.registerMetric(this.httpRequestsTotal);
    register.registerMetric(this.httpRequestDuration);
    register.registerMetric(this.activeConnections);
    register.registerMetric(this.designGenerationTime);
  }
  
  recordHttpRequest(method: string, route: string, statusCode: number, duration: number): void {
    this.httpRequestsTotal.inc({ method, route, status_code: statusCode.toString() });
    this.httpRequestDuration.observe({ method, route }, duration);
  }
  
  recordDesignGeneration(duration: number): void {
    this.designGenerationTime.observe(duration);
  }
  
  setActiveConnections(count: number): void {
    this.activeConnections.set(count);
  }
}

export const metricsService = new MetricsService();
```

### Logging Strategy

#### Structured Logging
```typescript
// Logger Service
import winston from 'winston';
import { ElasticsearchTransport } from 'winston-elasticsearch';

class LoggerService {
  private logger: winston.Logger;
  
  constructor() {
    const transports: winston.transport[] = [
      new winston.transports.Console({
        format: winston.format.combine(
          winston.format.colorize(),
          winston.format.simple()
        )
      })
    ];
    
    if (process.env.NODE_ENV === 'production') {
      transports.push(
        new ElasticsearchTransport({
          level: 'info',
          clientOpts: {
            node: process.env.ELASTICSEARCH_URL
          },
          index: 'boutique-to-box-logs'
        })
      );
    }
    
    this.logger = winston.createLogger({
      level: process.env.LOG_LEVEL || 'info',
      format: winston.format.combine(
        winston.format.timestamp(),
        winston.format.errors({ stack: true }),
        winston.format.json()
      ),
      defaultMeta: {
        service: 'boutique-to-box-api',
        version: process.env.APP_VERSION
      },
      transports
    });
  }
  
  info(message: string, meta?: any): void {
    this.logger.info(message, meta);
  }
  
  error(message: string, error?: Error, meta?: any): void {
    this.logger.error(message, { error: error?.stack, ...meta });
  }
  
  warn(message: string, meta?: any): void {
    this.logger.warn(message, meta);
  }
  
  debug(message: string, meta?: any): void {
    this.logger.debug(message, meta);
  }
}

export const logger = new LoggerService();
```

### Distributed Tracing

#### OpenTelemetry Setup
```typescript
// Tracing Configuration
import { NodeSDK } from '@opentelemetry/sdk-node';
import { getNodeAutoInstrumentations } from '@opentelemetry/auto-instrumentations-node';
import { JaegerExporter } from '@opentelemetry/exporter-jaeger';

const jaegerExporter = new JaegerExporter({
  endpoint: process.env.JAEGER_ENDPOINT || 'http://jaeger:14268/api/traces',
});

const sdk = new NodeSDK({
  traceExporter: jaegerExporter,
  instrumentations: [getNodeAutoInstrumentations()],
  serviceName: 'boutique-to-box-api',
  serviceVersion: process.env.APP_VERSION,
});

sdk.start();

// Custom Tracing
import { trace, SpanStatusCode } from '@opentelemetry/api';

const tracer = trace.getTracer('boutique-to-box-api');

export const traceFunction = <T>(
  name: string,
  fn: () => Promise<T>
): Promise<T> => {
  return tracer.startActiveSpan(name, async (span) => {
    try {
      const result = await fn();
      span.setStatus({ code: SpanStatusCode.OK });
      return result;
    } catch (error) {
      span.setStatus({
        code: SpanStatusCode.ERROR,
        message: error instanceof Error ? error.message : 'Unknown error'
      });
      throw error;
    } finally {
      span.end();
    }
  });
};
```

---

This architecture document provides a comprehensive foundation for building a scalable, maintainable, and observable system. The modular design allows for incremental implementation while maintaining flexibility for future enhancements.

**Next Steps**:
1. Review current implementation against target architecture
2. Prioritize architectural improvements based on immediate needs
3. Implement monitoring and observability first
4. Gradually migrate to microservices architecture
5. Establish performance benchmarks and SLAs