# API Documentation

## Base URL
```
Production: https://api.agribilling.com/v1
Staging: https://staging-api.agribilling.com/v1
Development: http://localhost:3000/api/v1
```

## Authentication

All API requests require authentication using JWT tokens.

### Login
```http
POST /auth/login
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "password123"
}

Response:
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "refresh_token_here",
    "user": {
      "id": "uuid",
      "email": "user@example.com",
      "name": "User Name",
      "role": "admin"
    }
  }
}
```

### Headers
```
Authorization: Bearer <token>
Content-Type: application/json
```

## Billing APIs

### Create Invoice
```http
POST /billing/invoices
Authorization: Bearer <token>

{
  "customerId": "uuid",
  "items": [
    {
      "productId": "uuid",
      "quantity": 10,
      "unitPrice": 266,
      "discount": 0,
      "taxRate": 18
    }
  ],
  "discount": 100,
  "paymentMethod": "cash",
  "notes": "Optional notes"
}

Response:
{
  "success": true,
  "data": {
    "id": "uuid",
    "invoiceNumber": "INV-2024-001",
    "subtotal": 2660,
    "discount": 100,
    "taxAmount": 460.80,
    "total": 3020.80,
    "createdAt": "2024-12-31T10:00:00Z"
  }
}
```

### Get Invoice
```http
GET /billing/invoices/:id
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "id": "uuid",
    "invoiceNumber": "INV-2024-001",
    "customer": {...},
    "items": [...],
    "subtotal": 2660,
    "total": 3020.80,
    "paymentStatus": "paid"
  }
}
```

### List Invoices
```http
GET /billing/invoices?page=1&limit=20&status=paid
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "invoices": [...],
    "pagination": {
      "page": 1,
      "limit": 20,
      "total": 150,
      "pages": 8
    }
  }
}
```

## Inventory APIs

### List Products
```http
GET /inventory/products?category=fertilizers&search=urea
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "products": [
      {
        "id": "uuid",
        "name": "Urea Fertilizer",
        "category": "Fertilizers",
        "stock": 500,
        "unit": "kg",
        "price": 266,
        "gstRate": 5
      }
    ]
  }
}
```

### Add Product
```http
POST /inventory/products
Authorization: Bearer <token>

{
  "name": "Urea Fertilizer",
  "categoryId": "uuid",
  "sku": "FERT-001",
  "barcode": "1234567890",
  "hsnCode": "31021000",
  "unit": "kg",
  "purchasePrice": 250,
  "sellingPrice": 266,
  "mrp": 280,
  "gstRate": 5,
  "stockQuantity": 500,
  "minStockLevel": 100,
  "expiryDate": "2025-06-30",
  "manufacturer": "ABC Fertilizers"
}

Response:
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Urea Fertilizer",
    "createdAt": "2024-12-31T10:00:00Z"
  }
}
```

### Update Stock
```http
POST /inventory/stock-transaction
Authorization: Bearer <token>

{
  "productId": "uuid",
  "transactionType": "purchase",
  "quantity": 100,
  "unitPrice": 250,
  "notes": "Stock received from supplier"
}

Response:
{
  "success": true,
  "data": {
    "newStock": 600,
    "transactionId": "uuid"
  }
}
```

### Low Stock Alert
```http
GET /inventory/low-stock
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "products": [
      {
        "id": "uuid",
        "name": "Corn Seeds",
        "currentStock": 45,
        "minStockLevel": 100,
        "status": "critical"
      }
    ]
  }
}
```

## Customer APIs

### List Customers
```http
GET /customers?search=ramesh&page=1&limit=20
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "customers": [
      {
        "id": "uuid",
        "name": "Ramesh Kumar",
        "phone": "+91 98765 43210",
        "email": "ramesh@example.com",
        "balance": 2340,
        "loyaltyPoints": 450
      }
    ]
  }
}
```

### Add Customer
```http
POST /customers
Authorization: Bearer <token>

{
  "name": "Ramesh Kumar",
  "phone": "+91 98765 43210",
  "email": "ramesh@example.com",
  "address": "123 Main St, City",
  "gstin": "29ABCDE1234F1Z5",
  "creditLimit": 50000,
  "customerType": "retail"
}

Response:
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Ramesh Kumar",
    "createdAt": "2024-12-31T10:00:00Z"
  }
}
```

### Customer Purchase History
```http
GET /customers/:id/history?from=2024-01-01&to=2024-12-31
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "purchases": [
      {
        "invoiceId": "uuid",
        "invoiceNumber": "INV-2024-001",
        "date": "2024-12-30",
        "amount": 3020.80,
        "items": 3
      }
    ],
    "summary": {
      "totalPurchases": 45,
      "totalAmount": 125000,
      "averageOrderValue": 2777.78
    }
  }
}
```

## Payment APIs

### Initiate Payment
```http
POST /payments/initiate
Authorization: Bearer <token>

{
  "invoiceId": "uuid",
  "amount": 3020.80,
  "paymentMethod": "upi",
  "upiId": "customer@upi"
}

Response:
{
  "success": true,
  "data": {
    "paymentId": "uuid",
    "status": "pending",
    "qrCode": "data:image/png;base64,...",
    "expiresAt": "2024-12-31T10:15:00Z"
  }
}
```

### Verify Payment
```http
POST /payments/verify
Authorization: Bearer <token>

{
  "paymentId": "uuid",
  "transactionId": "TXN123456"
}

Response:
{
  "success": true,
  "data": {
    "status": "success",
    "amount": 3020.80,
    "transactionId": "TXN123456",
    "completedAt": "2024-12-31T10:10:00Z"
  }
}
```

## AI APIs

### Voice Search
```http
POST /ai/voice-search
Authorization: Bearer <token>
Content-Type: multipart/form-data

{
  "audio": <audio_file>,
  "language": "en"
}

Response:
{
  "success": true,
  "data": {
    "transcript": "add 10 kg urea fertilizer",
    "intent": "add_product",
    "entities": {
      "quantity": 10,
      "unit": "kg",
      "product": "urea fertilizer"
    },
    "products": [
      {
        "id": "uuid",
        "name": "Urea Fertilizer",
        "confidence": 0.95
      }
    ]
  }
}
```

### Demand Forecast
```http
POST /ai/forecast
Authorization: Bearer <token>

{
  "productId": "uuid",
  "period": "next_month"
}

Response:
{
  "success": true,
  "data": {
    "productId": "uuid",
    "productName": "Urea Fertilizer",
    "currentStock": 500,
    "predictedDemand": 750,
    "recommendedOrder": 250,
    "confidence": 0.87,
    "factors": [
      "Seasonal increase",
      "Historical trend",
      "Weather forecast"
    ]
  }
}
```

### Get AI Insights
```http
GET /ai/insights
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "insights": [
      {
        "type": "demand_forecast",
        "priority": "high",
        "message": "Fertilizer demand expected to increase by 25% next week",
        "action": "Increase stock"
      },
      {
        "type": "expiry_alert",
        "priority": "medium",
        "message": "5 products will expire in the next 7 days",
        "action": "Promotional pricing"
      }
    ]
  }
}
```

### Chatbot
```http
POST /ai/chatbot
Authorization: Bearer <token>

{
  "message": "What is the price of urea fertilizer?",
  "sessionId": "uuid"
}

Response:
{
  "success": true,
  "data": {
    "response": "Urea Fertilizer is priced at ₹266 per kg. We currently have 500 kg in stock.",
    "suggestions": [
      "Add to cart",
      "Check similar products",
      "View details"
    ]
  }
}
```

## Reports APIs

### Sales Report
```http
GET /reports/sales?from=2024-12-01&to=2024-12-31&groupBy=day
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "period": {
      "from": "2024-12-01",
      "to": "2024-12-31"
    },
    "summary": {
      "totalSales": 876340,
      "totalInvoices": 234,
      "averageOrderValue": 3744.62
    },
    "data": [
      {
        "date": "2024-12-01",
        "sales": 45230,
        "invoices": 12
      }
    ]
  }
}
```

### Profit & Loss
```http
GET /reports/profit-loss?from=2024-12-01&to=2024-12-31
Authorization: Bearer <token>

Response:
{
  "success": true,
  "data": {
    "revenue": 876340,
    "costOfGoodsSold": 650000,
    "grossProfit": 226340,
    "expenses": {
      "rent": 20000,
      "salaries": 50000,
      "utilities": 5000,
      "other": 10000,
      "total": 85000
    },
    "netProfit": 141340,
    "profitMargin": 16.13
  }
}
```

### Export Report
```http
POST /reports/export
Authorization: Bearer <token>

{
  "reportType": "sales",
  "format": "pdf",
  "from": "2024-12-01",
  "to": "2024-12-31"
}

Response:
{
  "success": true,
  "data": {
    "downloadUrl": "https://cdn.example.com/reports/sales-2024-12.pdf",
    "expiresAt": "2024-12-31T23:59:59Z"
  }
}
```

## Error Responses

### Standard Error Format
```json
{
  "success": false,
  "error": {
    "code": "INVALID_INPUT",
    "message": "Validation failed",
    "details": [
      {
        "field": "email",
        "message": "Invalid email format"
      }
    ]
  }
}
```

### Error Codes
- `UNAUTHORIZED` (401): Invalid or expired token
- `FORBIDDEN` (403): Insufficient permissions
- `NOT_FOUND` (404): Resource not found
- `INVALID_INPUT` (400): Validation error
- `CONFLICT` (409): Duplicate resource
- `INTERNAL_ERROR` (500): Server error
- `RATE_LIMIT` (429): Too many requests

## Rate Limiting

- **Standard**: 100 requests per minute
- **AI APIs**: 20 requests per minute
- **Reports**: 10 requests per minute

Headers:
```
X-RateLimit-Limit: 100
X-RateLimit-Remaining: 95
X-RateLimit-Reset: 1704024000
```

## Webhooks

### Invoice Created
```json
POST <your_webhook_url>
{
  "event": "invoice.created",
  "data": {
    "invoiceId": "uuid",
    "invoiceNumber": "INV-2024-001",
    "total": 3020.80
  },
  "timestamp": "2024-12-31T10:00:00Z"
}
```

### Payment Received
```json
POST <your_webhook_url>
{
  "event": "payment.received",
  "data": {
    "paymentId": "uuid",
    "invoiceId": "uuid",
    "amount": 3020.80,
    "method": "upi"
  },
  "timestamp": "2024-12-31T10:10:00Z"
}
```

### Low Stock Alert
```json
POST <your_webhook_url>
{
  "event": "stock.low",
  "data": {
    "productId": "uuid",
    "productName": "Corn Seeds",
    "currentStock": 45,
    "minStockLevel": 100
  },
  "timestamp": "2024-12-31T10:00:00Z"
}
```
