# Finance Management API Documentation ( M2 )

## Overview
This API enables users to manage their personal finances, including transactions, budgets, categories, and savings plans. Users can track their income and expenses, set budgets, and create savings goals.

## Base URL
```
/api/v1
```

## Authentication
Include JWT token in all requests:
```
Authorization: Bearer <token>
```

## 1. Authentication Endpoints

### Register User
```
POST /auth/register/
```
Create a new user account.

**Request:**
```json
{
  "email": "user@example.com",
  "username": "john_doe",
  "name": "John Doe",
  "password": "securepassword123@"
}
```

**Response:** `201 Created`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "email": "user@example.com",
  "username": "john_doe",
  "name": "John Doe"
}
```

### Login
```
POST /auth/login/
```
Authenticate user and receive token.

**Request:**
```json
{
  "username": "john_doe",
  "password": "securepassword123@"
}
```

**Response:** `200 OK`
```json
{
    "data": {
        "user": {
            "id": "c38605bf-4595-4a6f-897f-986e11a3d4e3",
            "username": "admin",
            "email": "admin@email.com",
            "name": "admin",
            "created_at": "2025-02-03T13:10:03.508906+05:30",
            "updated_at": "2025-02-03T13:10:03.509248+05:30",
            "is_active": true,
            "is_staff": true
        },
        "tokens": {
            "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
            "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
        }
    }
}
```

### Refresh Token
```
POST /auth/token/refresh/
```
Get new access token using refresh token.

**Request:**
```json
{
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response:** `200 OK`
```json
{
  "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

### Logout
```
POST /auth/logout/
```
Invalidate current token.

**Response:** `200 OK`
```json
{
  "message": "Successfully logged out"
}
```

## 2. Category Management

### List Categories
```
GET /categories/
```
Retrieve all categories for the authenticated user.

**Query Parameters:**
- `type`: "debit" | "credit"
- `is_predefined`: boolean
- `page`: integer
- `page_size`: integer

**Response:** `200 OK`
```json
{
  "count": 5,
  "next": "/api/v1/categories/?page=2",
  "previous": null,
  "results": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Groceries",
      "type": "debit",
      "is_predefined": false,
      "created_at": "2025-02-03T13:10:03.508906+05:30",
      "updated_at": "2025-02-03T13:10:03.509248+05:30"
    }
  ]
}
```

### Get Category Detail
```
GET /categories/{id}/
```
Retrieve details of a specific category.

**Response:** `200 OK`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Groceries",
  "type": "debit",
  "is_predefined": false,
  "created_at": "2025-02-03T13:10:03.508906+05:30",
  "updated_at": "2025-02-03T13:10:03.509248+05:30"
}
```

### Create Category
```
POST /categories/
```
Create a new transaction category.

**Request:**
```json
{
  "name": "Groceries",
  "type": "debit"
}
```

**Response:** `201 Created`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Groceries",
  "type": "debit",
  "is_predefined": false,
  "created_at": "2025-02-03T13:10:03.508906+05:30",
  "updated_at": "2025-02-03T13:10:03.509248+05:30"
}
```

### Update Category
```
PUT /categories/{id}/
```
Update an existing category.

**Request:**
```json
{
  "name": "Updated Groceries",
  "type": "debit"
}
```

**Response:** `200 OK`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Updated Groceries",
  "type": "debit",
  "is_predefined": false,
  "created_at": "2025-02-03T13:10:03.508906+05:30",
  "updated_at": "2025-02-07T13:10:03.509248+05:30"
}
```

### Delete Category
```
DELETE /categories/{id}/
```
Delete a category. Cannot delete if transactions exist or if category is predefined.

**Response:** `204 No Content`

## 3. Transaction Management

### List Transactions
```
GET /transactions/
```
Retrieve user's transactions with optional filtering.

**Query Parameters:**
- `category`: UUID
- `type`: "credit" | "debit"
- `start_date`: YYYY-MM-DD
- `end_date`: YYYY-MM-DD
- `min_amount`: decimal
- `max_amount`: decimal
- `page`: integer
- `page_size`: integer

**Response:** `200 OK`
```json
{
  "count": 50,
  "next": "/api/v1/transactions/?page=2",
  "previous": null,
  "data": [
    {
      "id": "f822bc0c-8602-4ec3-b760-20e05baae610",
      "created_at": "2025-02-06T11:51:23.709087+05:30",
      "updated_at": "2025-02-06T11:51:23.709102+05:30",
      "amount": "910.00",
      "date": "2025-02-03T05:30:00+05:30",
      "description": "Sample transaction",
      "type": "debit",
      "category": {
        "id": "12080679-24a6-47ad-be2c-a52ac8076619",
        "name": "Groceries",
        "type": "debit"
      }
    }
  ]
}
```

### Get Transaction Detail
```
GET /transactions/{id}/
```
Retrieve details of a specific transaction.

**Response:** `200 OK`
```json
{
  "id": "f822bc0c-8602-4ec3-b760-20e05baae610",
  "created_at": "2025-02-06T11:51:23.709087+05:30",
  "updated_at": "2025-02-06T11:51:23.709102+05:30",
  "amount": "910.00",
  "date": "2025-02-03T05:30:00+05:30",
  "description": "Sample transaction",
  "type": "debit",
  "category": {
    "id": "12080679-24a6-47ad-be2c-a52ac8076619",
    "name": "Groceries",
    "type": "debit"
  }
}
```

### Create Transaction
```
POST /transactions/
```
Record a new transaction.

**Request:**
```json
{
  "category": "550e8400-e29b-41d4-a716-446655440000",
  "amount": 50.25,
  "date": "2025-02-07T10:30:00Z",
  "description": "Weekly groceries",
  "type": "debit"
}
```

**Response:** `201 Created`
```json
{
  "id": "f822bc0c-8602-4ec3-b760-20e05baae610",
  "created_at": "2025-02-07T11:51:23.709087+05:30",
  "updated_at": "2025-02-07T11:51:23.709102+05:30",
  "amount": "50.25",
  "date": "2025-02-07T10:30:00Z",
  "description": "Weekly groceries",
  "type": "debit",
  "category": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Groceries",
    "type": "debit"
  }
}
```

### Update Transaction
```
PUT /transactions/{id}/
```
Update an existing transaction.

**Request:**
```json
{
  "category": "550e8400-e29b-41d4-a716-446655440000",
  "amount": 55.25,
  "date": "2025-02-07T10:30:00Z",
  "description": "Updated grocery shopping",
  "type": "debit"
}
```

**Response:** `200 OK`
```json
{
  "id": "f822bc0c-8602-4ec3-b760-20e05baae610",
  "created_at": "2025-02-07T11:51:23.709087+05:30",
  "updated_at": "2025-02-07T12:51:23.709102+05:30",
  "amount": "55.25",
  "date": "2025-02-07T10:30:00Z",
  "description": "Updated grocery shopping",
  "type": "debit",
  "category": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Groceries",
    "type": "debit"
  }
}
```

### Delete Transaction
```
DELETE /transactions/{id}/
```
Remove a transaction from records.

**Response:** `204 No Content`



## 4. Budget Management

### List Budgets
```
GET /budgets/
```
Retrieve user's budgets with spending progress.

**Query Parameters:**
- `year`: integer
- `month`: integer
- `category`: UUID
- `page`: integer
- `page_size`: integer

**Response:** `200 OK`
```json
{
  "count": 5,
  "next": "/api/v1/budgets/?page=2",
  "previous": null,
  "results": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "category": {
        "id": "550e8400-e29b-41d4-a716-446655440000",
        "name": "Groceries"
      },
      "year": 2025,
      "month": 2,
      "amount": 500.00,
      "spent": 150.75,
      "remaining": 349.25,
      "created_at": "2025-02-03T13:10:03.508906+05:30",
      "updated_at": "2025-02-03T13:10:03.509248+05:30"
    }
  ]
}
```

### Get Budget Detail
```
GET /budgets/{id}/
```
Retrieve details of a specific budget.

**Response:** `200 OK`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "category": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Groceries"
  },
  "year": 2025,
  "month": 2,
  "amount": 500.00,
  "spent": 150.75,
  "remaining": 349.25,
  "created_at": "2025-02-03T13:10:03.508906+05:30",
  "updated_at": "2025-02-03T13:10:03.509248+05:30"
}
```

### Create Budget
```
POST /budgets/
```
Set a new budget for a category.

**Request:**
```json
{
  "category": "550e8400-e29b-41d4-a716-446655440000",
  "year": 2025,
  "month": 2,
  "amount": 500.00
}
```

**Response:** `201 Created`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "category": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Groceries"
  },
  "year": 2025,
  "month": 2,
  "amount": 500.00,
  "spent": 0.00,
  "remaining": 500.00,
  "created_at": "2025-02-07T13:10:03.508906+05:30",
  "updated_at": "2025-02-07T13:10:03.509248+05:30"
}
```

### Update Budget
```
PUT /budgets/{id}/
```
Modify an existing budget.

**Request:**
```json
{
  "category": "550e8400-e29b-41d4-a716-446655440000",
  "year": 2025,
  "month": 2,
  "amount": 600.00
}
```

**Response:** `200 OK`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "category": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Groceries"
  },
  "year": 2025,
  "month": 2,
  "amount": 600.00,
  "spent": 150.75,
  "remaining": 449.25,
  "created_at": "2025-02-03T13:10:03.508906+05:30",
  "updated_at": "2025-02-07T13:10:03.509248+05:30"
}
```

### Delete Budget
```
DELETE /budgets/{id}/
```
Remove a budget allocation.

**Response:** `204 No Content`

### Copy Previous Month's Budgets
```
POST /budgets/copy-previous/
```
Copy all budgets from the previous month to the current month.

**Request:**
```json
{
  "year": 2025,
  "month": 2
}
```

**Response:** `201 Created`
```json
{
  "message": "Successfully copied budgets",
  "copied_count": 5,
  "budgets": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "category": {
        "id": "550e8400-e29b-41d4-a716-446655440000",
        "name": "Groceries"
      },
      "amount": 500.00
    }
  ]
}
```

## 5. Savings Plans

### List Savings Plans
```
GET /savings/plans/
```
Retrieve all savings plans with progress.

**Query Parameters:**
- `priority`: "HIGH" | "MEDIUM" | "LOW"
- `status`: "active" | "completed"
- `page`: integer
- `page_size`: integer

**Response:** `200 OK`
```json
{
  "count": 3,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Emergency Fund",
      "target_amount": 5000.00,
      "saved_amount": 2500.00,
      "deadline": "2025-12-31",
      "priority": "HIGH",
      "frequency": "MONTHLY",
      "progress_percentage": 50.00,
      "created_at": "2025-02-03T13:10:03.508906+05:30",
      "updated_at": "2025-02-03T13:10:03.509248+05:30",
      "status": "active"
    }
  ]
}
```

### Get Savings Plan Detail
```
GET /savings/plans/{id}/
```
Retrieve details of a specific savings plan.

**Response:** `200 OK`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Emergency Fund",
  "target_amount": 5000.00,
  "saved_amount": 2500.00,
  "deadline": "2025-12-31",
  "priority": "HIGH",
  "frequency": "MONTHLY",
  "progress_percentage": 50.00,
  "created_at": "2025-02-03T13:10:03.508906+05:30",
  "updated_at": "2025-02-03T13:10:03.509248+05:30",
  "status": "active",
  "transactions": [
    {
      "id": "660e8400-e29b-41d4-a716-446655440000",
      "amount_contributed": 500.00,
      "date": "2025-02-01T10:30:00Z"
    }
  ]
}
```

### Create Savings Plan
```
POST /savings/plans/
```
Create a new savings goal.

**Request:**
```json
{
  "name": "Emergency Fund",
  "target_amount": 5000.00,
  "deadline": "2025-12-31",
  "priority": "HIGH",
  "frequency": "MONTHLY"
}
```

**Response:** `201 Created`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Emergency Fund",
  "target_amount": 5000.00,
  "saved_amount": 0.00,
  "deadline": "2025-12-31",
  "priority": "HIGH",
  "frequency": "MONTHLY",
  "progress_percentage": 0.00,
  "created_at": "2025-02-07T13:10:03.508906+05:30",
  "updated_at": "2025-02-07T13:10:03.509248+05:30",
  "status": "active"
}
```

### Update Savings Plan
```
PUT /savings/plans/{id}/
```
Modify an existing savings plan.

**Request:**
```json
{
  "name": "Updated Emergency Fund",
  "target_amount": 6000.00,
  "deadline": "2025-12-31",
  "priority": "HIGH",
  "frequency": "MONTHLY"
}
```

**Response:** `200 OK`
```json
{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "name": "Updated Emergency Fund",
  "target_amount": 6000.00,
  "saved_amount": 2500.00,
  "deadline": "2025-12-31",
  "priority": "HIGH",
  "frequency": "MONTHLY",
  "progress_percentage": 41.67,
  "created_at": "2025-02-03T13:10:03.508906+05:30",
  "updated_at": "2025-02-07T13:10:03.509248+05:30",
  "status": "active"
}
```

### Delete Savings Plan
```
DELETE /savings/plans/{id}/
```
Remove a savings plan. Cannot delete if transactions exist.

**Response:** `204 No Content`

### List Savings Transactions
```
GET /savings/transactions/
```
Retrieve all savings contributions.

**Query Parameters:**
- `savings_plan`: UUID
- `start_date`: YYYY-MM-DD
- `end_date`: YYYY-MM-DD
- `page`: integer
- `page_size`: integer

**Response:** `200 OK`
```json
{
  "count": 10,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": "660e8400-e29b-41d4-a716-446655440000",
      "savings_plan": {
        "id": "550e8400-e29b-41d4-a716-446655440000",
        "name": "Emergency Fund"
      },
      "amount_contributed": 500.00,
      "date": "2025-02-01T10:30:00Z",
      "created_at": "2025-02-01T13:10:03.508906+05:30",
      "updated_at": "2025-02-01T13:10:03.509248+05:30"
    }
  ]
}
```

### Record Savings Transaction
```
POST /savings/transactions/
```
Record a contribution to a savings plan.

**Request:**
```json
{
  "savings_plan": "550e8400-e29b-41d4-a716-446655440000",
  "amount_contributed": 500.00,
  "date": "2025-02-07T10:30:00Z"
}
```

**Response:** `201 Created`
```json
{
  "id": "660e8400-e29b-41d4-a716-446655440000",
  "savings_plan": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Emergency Fund"
  },
  "amount_contributed": 500.00,
  "date": "2025-02-07T10:30:00Z",
  "created_at": "2025-02-07T13:10:03.508906+05:30",
  "updated_at": "2025-02-07T13:10:03.509248+05:30"
}
```

### Update Savings Transaction
```
PUT /savings/transactions/{id}/
```
Update a savings contribution record.

**Request:**
```json
{
  "amount_contributed": 600.00,
  "date": "2025-02-07T10:30:00Z"
}
```

**Response:** `200 OK`
```json
{
  "id": "660e8400-e29b-41d4-a716-446655440000",
  "savings_plan": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Emergency Fund"
  },
  "amount_contributed": 600.00,
  "date": "2025-02-07T10:30:00Z",
  "created_at": "2025-02-07T13:10:03.508906+05:30",
  "updated_at": "2025-02-07T14:10:03.509248+05:30"
}
```

### Delete Savings Transaction
```
DELETE /savings/transactions/{id}/
```
Remove a savings contribution record.

**Response:** `204 No Content`

## Error Responses

All endpoints may return these standard error responses:

### 400 Bad Request
```json
{
  "error": {
    "code": "validation_error",
    "message": "Invalid input data",
    "details": {
      "field_name": ["error message"]
    }
  }
}
```

### 401 Unauthorized
```json
{
  "error": {
    "code": "authentication_failed",
    "message": "Invalid or expired token"
  }
}
```

### 403 Forbidden
```json
{
  "error": {
    "code": "permission_denied",
    "message": "You do not have permission to perform this action"
  }
}
```

### 404 Not Found
```json
{
  "error": {
    "code": "not_found",
    "message": "Requested resource not found"
  }
}
```

### 409 Conflict
```json
{
  "error": {
    "code": "resource_conflict",
    "message": "Resource cannot be modified or deleted due to existing dependencies"
  }
}
```

### 429 Too Many Requests
```json
{
  "error": {
    "code": "rate_limit_exceeded",
    "message": "Too many requests. Please try again later.",
    "retry_after": 60
  }
}
```

### 500 Internal Server Error
```json
{
  "error": {
    "code": "internal_error",
    "message": "An unexpected error occurred"
  }
}
```
