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
        "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoiYWNjZXNzIiwiZXhwIjoxNzM4OTU2MDg3LCJpYXQiOjE3Mzg5NTI0ODcsImp0aSI6IjlmZmVjZTg5ZmQwMjRmMDVhMjViMzUyZTZlOTdhZjQxIiwidXNlcl9pZCI6IjI2YTg0ZTVjLTZjNzgtNDk1NC04YmFmLWFlNWZjYzhiNDNmNiJ9.v0LH38ulQu_ftJ38tbhtu6UHS9oA56_C0XA4hYTBZvQ",
        "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ0b2tlbl90eXBlIjoicmVmcmVzaCIsImV4cCI6MTczOTM4NDQ4NywiaWF0IjoxNzM4OTUyNDg3LCJqdGkiOiJmZmY4ODRiZDZiZmM0ZjEzYjhjMmIyMjA2OTFhMTYwYSIsInVzZXJfaWQiOiIyNmE4NGU1Yy02Yzc4LTQ5NTQtOGJhZi1hZTVmY2M4YjQzZjYifQ.Q-6BAjzME_5RTQAXXCsOXVvfJcCUYHSJ8B4AGz_V2GQ"
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
    "message": "Logged out successfully"
}
```

## 2. Category Management

### List Categories
```
GET /categories/
```
Retrieve all categories for the authenticated user.


**Response:** `200 OK`
```json
{
  "count": 5,
  "next": "/api/v1/categories/?page=2",
  "previous": null,
  "data": [
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


**Response:** `200 OK`
```json
{
  "count": 10,
  "next": "/api/v1/transactions/?page=2",
  "previous": null,
    "data": [
        {
            "id": "b62f80c9-695e-4bbe-bdf3-f9dba50ac7ea",
            "created_at": "2025-02-07T23:58:43.310845+05:30",
            "updated_at": "2025-02-07T23:58:43.310859+05:30",
            "is_deleted": false,
            "amount": "10.00",
            "date": "2025-02-04T00:00:00+05:30",
            "description": "Sample transaction",
            "type": "debit",
            "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
            "category": "38c73786-d676-4d97-8fb9-78f187a50ae3"
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
        "id": "b62f80c9-695e-4bbe-bdf3-f9dba50ac7ea",
        "created_at": "2025-02-07T23:58:43.310845+05:30",
        "updated_at": "2025-02-07T23:58:43.310859+05:30",
        "is_deleted": false,
        "amount": "10.00",
        "date": "2025-02-04T00:00:00+05:30",
        "description": "Sample transaction",
        "type": "debit",
        "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
        "category": "38c73786-d676-4d97-8fb9-78f187a50ae3"
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
    "user":"3d24fe11-cb09-450d-a601-b16592afcfc2",
    "date": "2025-02-04",
    "amount": 10.00,
    "type": "debit",
    "category":"38c73786-d676-4d97-8fb9-78f187a50ae3",
    "description": "Sample transaction"
}
```

**Response:** `201 Created`
```json
{
    "data": {
        "id": "b62f80c9-695e-4bbe-bdf3-f9dba50ac7ea",
        "created_at": "2025-02-07T23:58:43.310845+05:30",
        "updated_at": "2025-02-07T23:58:43.310859+05:30",
        "is_deleted": false,
        "amount": "10.00",
        "date": "2025-02-04T00:00:00+05:30",
        "description": "Sample transaction",
        "type": "debit",
        "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
        "category": "38c73786-d676-4d97-8fb9-78f187a50ae3"
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
    "amount":"999"
}
```

**Response:** `200 OK`
```json
{
    "data": {
        "id": "b62f80c9-695e-4bbe-bdf3-f9dba50ac7ea",
        "created_at": "2025-02-07T23:58:43.310845+05:30",
        "updated_at": "2025-02-08T00:03:27.226104+05:30",
        "is_deleted": false,
        "amount": "999.00",
        "date": "2025-02-04T00:00:00+05:30",
        "description": "Sample transaction",
        "type": "debit",
        "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
        "category": "38c73786-d676-4d97-8fb9-78f187a50ae3"
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
- `month_year`: integer
- `category`: UUID

**Response:** `200 OK`
```json
{
  "count": 5,
  "next": "/api/v1/budgets/?page=2",
  "previous": null,
    "data": [
        {
            "id": "e2c62488-b97c-41d4-bd12-219469c44a0f",
            "amount": "1000.00",
            "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
            "category": "38c73786-d676-4d97-8fb9-78f187a50ae3",
            "spent_amount": "0.00",
            "year": 2025,
            "month": 3,
            "is_deleted": false
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
    "id": "e2c62488-b97c-41d4-bd12-219469c44a0f",
    "amount": "1000.00",
    "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
    "category": "38c73786-d676-4d97-8fb9-78f187a50ae3",
    "spent_amount": "0.00",
    "year": 2025,
    "month": 3,
    "is_deleted": false
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
        "user":"3d24fe11-cb09-450d-a601-b16592afcfc2",
        "category":"38c73786-d676-4d97-8fb9-78f187a50ae3",
        "amount": "1000",
        "month_year":"3-2025"
}
```

**Response:** `201 Created`
```json
{
    "id": "e2c62488-b97c-41d4-bd12-219469c44a0f",
    "amount": "1000.00",
    "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
    "category": "38c73786-d676-4d97-8fb9-78f187a50ae3",
    "spent_amount": "0.00",
    "year": 2025,
    "month": 3,
    "is_deleted": false
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
  "amount": 600.00
}
```

**Response:** `200 OK`
```json
{
    "id": "e2c62488-b97c-41d4-bd12-219469c44a0f",
    "amount": "600.00",
    "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
    "category": "38c73786-d676-4d97-8fb9-78f187a50ae3",
    "spent_amount": "0.00",
    "year": 2025,
    "month": 3,
    "is_deleted": false
}
```

### Delete Budget
```
DELETE /budgets/{id}/
```
Remove a budget allocation.

**Response:** `204 No Content`

## 5. Savings Plans

### List Savings Plans
```
GET /savings/plans/
```
Retrieve all savings plans with progress.


**Response:** `200 OK`
```json
{
  "count": 3,
  "next": null,
  "previous": null,
    "data": [
        {
            "id": "be9e68c6-702e-4bc0-8e0a-cddd2dd34a63",
            "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
            "name": "MANALI",
            "target_amount": "10000.00",
            "deadline": "2026-08-24",
            "priority": "HIGH",
            "frequency": "WEEKLY",
            "is_deleted": false,
            "progress_percentage": 0.0,
            "amount_saved": 0,
            "remaining_amount": 10000.0,
            "time_remaining": "19 months left"
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
    "id": "be9e68c6-702e-4bc0-8e0a-cddd2dd34a63",
    "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
    "name": "MANALI",
    "target_amount": "10000.00",
    "deadline": "2026-08-24",
    "priority": "HIGH",
    "frequency": "WEEKLY",
    "is_deleted": false,
    "progress_percentage": 0.0,
    "amount_saved": 0,
    "remaining_amount": 10000.0,
    "time_remaining": "19 months left"
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
    "user":"3d24fe11-cb09-450d-a601-b16592afcfc2",
    "name":"MANALI",
    "target_amount":"10000",
    "deadline": "2026-08-24",
    "priority": "HIGH",
    "frequency": "WEEKLY"
}
```

**Response:** `201 Created`
```json
{
    "id": "be9e68c6-702e-4bc0-8e0a-cddd2dd34a63",
    "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
    "name": "MANALI",
    "target_amount": "10000.00",
    "deadline": "2026-08-24",
    "priority": "HIGH",
    "frequency": "WEEKLY",
    "is_deleted": false,
    "progress_percentage": 0.0,
    "amount_saved": 0,
    "remaining_amount": 10000.0,
    "time_remaining": "19 months left"
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
    "target_amount":"50000"
}
```

**Response:** `200 OK`
```json
{
    "id": "be9e68c6-702e-4bc0-8e0a-cddd2dd34a63",
    "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
    "name": "MANALI",
    "target_amount": "50000.00",
    "deadline": "2026-08-24",
    "priority": "HIGH",
    "frequency": "WEEKLY",
    "is_deleted": false,
    "progress_percentage": 0.0,
    "amount_saved": 0,
    "remaining_amount": 50000.0,
    "time_remaining": "19 months left"
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


**Response:** `200 OK`
```json
{
    "data": [
        {
            "id": "2eb25565-b0f6-40a7-ac84-362fe3eeea5e",
            "savings_plan": "be9e68c6-702e-4bc0-8e0a-cddd2dd34a63",
            "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
            "amount_contributed": "500.00",
            "transaction_date": "2025-02-08T00:19:44.706015+05:30",
            "is_deleted": false
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
  "savings_plan": "be9e68c6-702e-4bc0-8e0a-cddd2dd34a63",
  "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
  "amount_contributed": "500"
}
```

**Response:** `201 Created`
```json
{
    "id": "2eb25565-b0f6-40a7-ac84-362fe3eeea5e",
    "savings_plan": "be9e68c6-702e-4bc0-8e0a-cddd2dd34a63",
    "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
    "amount_contributed": "500.00",
    "transaction_date": "2025-02-08T00:19:44.706015+05:30",
    "is_deleted": false
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
  "amount_contributed": 2000.00,
}
```

**Response:** `200 OK`
```json
{
    "id": "2eb25565-b0f6-40a7-ac84-362fe3eeea5e",
    "savings_plan": "be9e68c6-702e-4bc0-8e0a-cddd2dd34a63",
    "user": "3d24fe11-cb09-450d-a601-b16592afcfc2",
    "amount_contributed": "2000.00",
    "transaction_date": "2025-02-08T00:19:44.706015+05:30",
    "is_deleted": false
}
```

### Delete Savings Transaction
```
DELETE /savings/transactions/{id}/
```
Remove a savings contribution record.

**Response:** `204 No Content`

## Full Source Code  

Please find the full source code [here](https://github.com/shakti505/Income-Expense-Tracker).





## Recurring Transactions

### List Recurring Transactions
```
GET /transactions/recurring/
```
Retrieve all recurring transactions.

**Response:** `200 OK`
```json
{
  "count": 2,
  "next": null,
  "previous": null,
  "data": [
    {
      "id": "550e8400-e29b-41d4-a716-446655440000",
      "name": "Monthly Rent",
      "amount": "1500.00",
      "category": "38c73786-d676-4d97-8fb9-78f187a50ae3",
      "frequency": "MONTHLY",
      "start_date": "2025-02-01",
      "end_date": "2025-12-31",
      "next_occurrence": "2025-03-01",
      "description": "Apartment rent payment",
      "is_active": true,
      "created_at": "2025-02-08T00:19:44.706015+05:30",
      "updated_at": "2025-02-08T00:19:44.706015+05:30"
    }
  ]
}
```

### Create Recurring Transaction
```
POST /transactions/recurring/
```

**Request:**
```json
{
  "name": "Monthly Rent",
  "amount": "1500.00",
  "category": "38c73786-d676-4d97-8fb9-78f187a50ae3",
  "frequency": "MONTHLY",
  "start_date": "2025-02-01",
  "end_date": "2025-12-31",
  "description": "Apartment rent payment"
}
```

**Response:** `201 Created`
```json
{
  "data": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "name": "Monthly Rent",
    "amount": "1500.00",
    "category": "38c73786-d676-4d97-8fb9-78f187a50ae3",
    "frequency": "MONTHLY",
    "start_date": "2025-02-01",
    "end_date": "2025-12-31",
    "next_occurrence": "2025-03-01",
    "description": "Apartment rent payment",
    "is_active": true,
    "created_at": "2025-02-08T00:19:44.706015+05:30",
    "updated_at": "2025-02-08T00:19:44.706015+05:30"
  }
}
```

## Savings Plan Deadline Extension

### Extend Savings Plan Deadline
```
PATCH /savings/plans/{id}/extend-deadline/
```

**Request:**
```json
{
  "new_deadline": "2026-12-31",
  "reason": "Need more time to reach savings goal"
}
```

**Response:** `200 OK`
```json
{
  "data": {
    "id": "be9e68c6-702e-4bc0-8e0a-cddd2dd34a63",
    "name": "MANALI",
    "target_amount": "50000.00",
    "original_deadline": "2026-08-24",
    "new_deadline": "2026-12-31",
    "extension_history": [
      {
        "date_extended": "2025-02-08T00:19:44.706015+05:30",
        "old_deadline": "2026-08-24",
        "new_deadline": "2026-12-31",
        "reason": "Need more time to reach savings goal"
      }
    ],
    "progress_percentage": 4.0,
    "amount_saved": 2000.0,
    "remaining_amount": 48000.0,
    "time_remaining": "23 months left"
  }
}
```

## Transaction Summary and Trends

### Get Transaction Summary
```
GET /transactions/summary/
```

**Query Parameters:**
- `start_date`: date (YYYY-MM-DD)
- `end_date`: date (YYYY-MM-DD)
- `category`: UUID (optional)

**Response:** `200 OK`
```json
{
  "data": {
    "period": {
      "start_date": "2025-01-01",
      "end_date": "2025-02-08"
    },
    "total_income": "5000.00",
    "total_expenses": "3500.00",
    "net_savings": "1500.00",
    "category_breakdown": [
      {
        "category": "Groceries",
        "total_amount": "800.00",
        "percentage": 22.86
      }
    ],
    "monthly_averages": {
      "income": "2500.00",
      "expenses": "1750.00",
      "savings": "750.00"
    }
  }
}
```

### Get Transaction Trends
```
GET /transactions/trends/
```

**Query Parameters:**
- `period`: string (WEEKLY, MONTHLY, YEARLY)
- `start_date`: date (YYYY-MM-DD)
- `end_date`: date (YYYY-MM-DD)

**Response:** `200 OK`
```json
{
  "data": {
    "trend_period": "MONTHLY",
    "trends": [
      {
        "period": "2025-01",
        "income": "2500.00",
        "expenses": "1800.00",
        "savings": "700.00",
        "top_expense_categories": [
          {
            "category": "Groceries",
            "amount": "500.00"
          }
        ],
        "comparison_previous_period": {
          "income_change": "+5.2%",
          "expense_change": "-2.1%",
          "savings_change": "+15.3%"
        }
      }
    ],
    "overall_trend": {
      "income_trend": "INCREASING",
      "expense_trend": "STABLE",
      "savings_trend": "INCREASING"
    }
  }
}
```

## Export Functionality

### Export Transactions to PDF
```
GET /transactions/export/pdf/
```

**Query Parameters:**
- `start_date`: date (YYYY-MM-DD)
- `end_date`: date (YYYY-MM-DD)
- `categories`: array of UUIDs (optional)

**Response:** `200 OK`
```json
{
  "data": {
    "file_url": "https://api.example.com/downloads/transactions_2025_01_02.pdf",
    "expires_at": "2025-02-09T00:19:44.706015+05:30"
  }
}
```

### Export Transactions to CSV
```
GET /transactions/export/csv/
```

**Query Parameters:**
- `start_date`: date (YYYY-MM-DD)
- `end_date`: date (YYYY-MM-DD)
- `categories`: array of UUIDs (optional)

**Response:** `200 OK`
```json
{
  "data": {
    "file_url": "https://api.example.com/downloads/transactions_2025_01_02.csv",
    "expires_at": "2025-02-09T00:19:44.706015+05:30"
  }
}
```
