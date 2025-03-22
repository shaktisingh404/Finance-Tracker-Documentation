# Finance Management API Documentation ( M3 ) 

## Overview

The Personal Finance Tracker is a comprehensive API that allows users to manage their financial transactions, categorize expenses, and generate reports. It features user authentication, category management, transaction tracking, and reporting functionalities.

## Core Modules

### 1. Authentication Module

Handles user registration, login, logout, email verification, and password reset functionality.

### 2. User Module

Manages user profiles, allowing users to update their information, change passwords, and update email addresses.

### 3. Category Module

Enables the creation and management of transaction categories, supporting both user-specific and predefined categories.

### 4. Transaction Module

Provides functionality to create, read, update, and delete financial transactions, categorizing them as either credits (income) or debits (expenses).

### 5. Summary Report Module

Generates financial reports based on transaction data, including category breakdowns and time period summaries.

## API Endpoints

### Authentication Endpoints

#### Register a new user
- **URL**: `/api/auth/signup`
- **Method**: `POST`
- **Data**:
```json
{
  "name": "John Doe",
  "username": "johndoe",
  "email": "john.doe@example.com",
  "password": "SecureP@ssw0rd"
}
```
- **Response**: 
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "name": "John Doe",
  "username": "johndoe",
  "email": "john.doe@example.com",
  "created_at": "2025-03-10T12:00:00Z",
  "updated_at": "2025-03-10T12:00:00Z",
  "is_deleted": false,
  "is_staff": false
}
```

#### Login
- **URL**: `/api/auth/login`
- **Method**: `POST`
- **Data**:
```json
{
  "username": "johndoe",
  "password": "SecureP@ssw0rd"
}
```
- **Response**:
```json
{
  "message": "Welcome back! You have been successfully logged in.",
  "tokens": {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "abc"
  }
}
```

#### Logout
- **URL**: `/api/auth/logout`
- **Method**: `POST`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```json
{
  "message": "You have been successfully logged out. Thank you for using our service."
}
```

#### Verify Email
- **URL**: `/api/auth/verify-user/{token}`
- **Method**: `GET`
- **Response**:
```json
{
  "message": "Thank you! Your email has been successfully verified. You can now access all features of your account."
}
```

#### Request Password Reset
- **URL**: `/api/auth/reset-password`
- **Method**: `POST`
- **Data**:
```json
{
  "email": "john.doe@example.com"
}
```
- **Response**:
```json
{
  "message": "Password reset instructions have been sent to your email address. Please check your inbox and follow the provided link."
}
```

#### Reset Password
- **URL**: `/api/auth/reset-password-confirm/{token}`
- **Method**: `POST`
- **Data**:
```json
{
  "password": "NewSecureP@ssw0rd",
  "confirm_password": "NewSecureP@ssw0rd"
}
```
- **Response**:
```json
{
  "message": "Your password has been successfully updated. You can now log in with your new password."
}
```

#### Resend Verification Link
- **URL**: `/api/auth/resend-verification-link`
- **Method**: `POST`
- **Data**:
```json
{
  "email": "john.doe@example.com"
}
```
- **Response**:
```json
{
  "message": "A new verification link has been sent to your email address. Please check your inbox and verify your account."
}
```

### User Management Endpoints

#### Get User Details
- **URL**: `/api/users/{user_id}`
- **Method**: `GET`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "name": "John Doe",
  "username": "johndoe",
  "email": "john.doe@example.com",
  "created_at": "2025-03-10T12:00:00Z",
  "updated_at": "2025-03-10T12:00:00Z",
  "is_deleted": false,
  "is_staff": false
}
```

#### Update User Details
- **URL**: `/api/users/{user_id}`
- **Method**: `PATCH`
- **Headers**: `Authorization: Bearer {access_token}`
- **Data**:
```json
{
  "name": "John Smith",
  "username": "johnsmith"
}
```
- **Response**:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174000",
  "name": "John Smith",
  "username": "johnsmith",
  "email": "john.doe@example.com",
  "created_at": "2025-03-10T12:00:00Z",
  "updated_at": "2025-03-10T13:00:00Z",
  "is_deleted": false,
  "is_staff": false
}
```

#### Update Password
- **URL**: `/api/users/{user_id}/update-password`
- **Method**: `POST`
- **Headers**: `Authorization: Bearer {access_token}`
- **Data**:
```json
{
  "current_password": "SecureP@ssw0rd",
  "new_password": "EvenMoreSecureP@ssw0rd",
  "confirm_password": "EvenMoreSecureP@ssw0rd"
}
```
- **Response**:
```json
{
  "message": "Password has been updated successfully"
}
```

#### Request Email Update
- **URL**: `/api/users/{user_id}/update-email`
- **Method**: `POST`
- **Headers**: `Authorization: Bearer {access_token}`
- **Data**:
```json
{
  "new_email": "john.smith@example.com"
}
```
- **Response**:
```json
{
  "message": "We've sent one-time passwords to both your current and new email addresses. Please check both inboxes to verify your email change."
}
```

#### Confirm Email Update
- **URL**: `/api/users/{user_id}/update-email/confirm`
- **Method**: `POST`
- **Headers**: `Authorization: Bearer {access_token}`
- **Data**:
```json
{
  "current_email_otp": "123456",
  "new_email_otp": "789012"
}
```
- **Response**:
```json
{
  "message": "Great news! Your email address has been updated successfully. You can now use your new email to log in."
}
```

#### Delete User Account
- **URL**: `/api/users/{user_id}`
- **Method**: `DELETE`
- **Headers**: `Authorization: Bearer {access_token}`
- **Data**:
```json
{
  "password": "SecureP@ssw0rd"
}
```
- **Response**:
```
Empty response with 204 status code
```

### Category Endpoints

#### List Categories
- **URL**: `/api/categories`
- **Method**: `GET`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```json
{
  "count": 3,
  "items": [
    {
      "id": "123e4567-e89b-12d3-a456-426614174001",
      "name": "Groceries",
      "user_id": "123e4567-e89b-12d3-a456-426614174000",
      "is_predefined": false,
      "created_at": "2025-03-10T12:00:00Z",
      "updated_at": "2025-03-10T12:00:00Z",
      "is_deleted": false
    },
    {
      "id": "123e4567-e89b-12d3-a456-426614174002",
      "name": "Utilities",
      "user_id": "123e4567-e89b-12d3-a456-426614174000",
      "is_predefined": false,
      "created_at": "2025-03-10T12:01:00Z",
      "updated_at": "2025-03-10T12:01:00Z",
      "is_deleted": false
    },
    {
      "id": "123e4567-e89b-12d3-a456-426614174003",
      "name": "Salary",
      "user_id": "123e4567-e89b-12d3-a456-426614174000",
      "is_predefined": true,
      "created_at": "2025-03-10T12:02:00Z",
      "updated_at": "2025-03-10T12:02:00Z",
      "is_deleted": false
    }
  ]
}
```

#### Create Category
- **URL**: `/api/categories`
- **Method**: `POST`
- **Headers**: `Authorization: Bearer {access_token}`
- **Data**:
```json
{
  "name": "Entertainment",
  "user_id": "123e4567-e89b-12d3-a456-426614174000"
}
```
- **Response**:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174004",
  "name": "Entertainment",
  "user_id": "123e4567-e89b-12d3-a456-426614174000",
  "is_predefined": false,
  "created_at": "2025-03-10T14:00:00Z",
  "updated_at": "2025-03-10T14:00:00Z",
  "is_deleted": false
}
```

#### Get Category Details
- **URL**: `/api/categories/{category_id}`
- **Method**: `GET`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174004",
  "name": "Entertainment",
  "user_id": "123e4567-e89b-12d3-a456-426614174000",
  "is_predefined": false,
  "created_at": "2025-03-10T14:00:00Z",
  "updated_at": "2025-03-10T14:00:00Z",
  "is_deleted": false
}
```

#### Update Category
- **URL**: `/api/categories/{category_id}`
- **Method**: `PATCH`
- **Headers**: `Authorization: Bearer {access_token}`
- **Data**:
```json
{
  "name": "Movies & Entertainment"
}
```
- **Response**:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174004",
  "name": "Movies & Entertainment",
  "user_id": "123e4567-e89b-12d3-a456-426614174000",
  "is_predefined": false,
  "created_at": "2025-03-10T14:00:00Z",
  "updated_at": "2025-03-10T14:30:00Z",
  "is_deleted": false
}
```

#### Delete Category
- **URL**: `/api/categories/{category_id}`
- **Method**: `DELETE`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```
Empty response with 204 status code
```

### Transaction Endpoints

#### List Transactions
- **URL**: `/api/transactions`
- **Method**: `GET`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```json
{
  "count": 2,
  "items": [
    {
      "id": "123e4567-e89b-12d3-a456-426614174005",
      "type": "DEBIT",
      "amount": "50.00",
      "category_id": "123e4567-e89b-12d3-a456-426614174001",
      "user_id": "123e4567-e89b-12d3-a456-426614174000",
      "date": "2025-03-09T15:30:00",
      "description": "Weekly grocery shopping",
      "created_at": "2025-03-09T16:00:00Z",
      "updated_at": "2025-03-09T16:00:00Z",
      "is_deleted": false
    },
    {
      "id": "123e4567-e89b-12d3-a456-426614174006",
      "type": "CREDIT",
      "amount": "2000.00",
      "category_id": "123e4567-e89b-12d3-a456-426614174003",
      "user_id": "123e4567-e89b-12d3-a456-426614174000",
      "date": "2025-03-01T09:00:00",
      "description": "Monthly salary",
      "created_at": "2025-03-01T09:30:00Z",
      "updated_at": "2025-03-01T09:30:00Z",
      "is_deleted": false
    }
  ]
}
```

#### Create Transaction
- **URL**: `/api/transactions`
- **Method**: `POST`
- **Headers**: `Authorization: Bearer {access_token}`
- **Data**:
```json
{
  "type": "DEBIT",
  "amount": "120.50",
  "category_id": "123e4567-e89b-12d3-a456-426614174004",
  "user_id": "123e4567-e89b-12d3-a456-426614174000",
  "date": "2025-03-10T19:30:00",
  "description": "Movie night with friends"
}
```
- **Response**:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174007",
  "type": "DEBIT",
  "amount": "120.50",
  "category_id": "123e4567-e89b-12d3-a456-426614174004",
  "user_id": "123e4567-e89b-12d3-a456-426614174000",
  "date": "2025-03-10T19:30:00",
  "description": "Movie night with friends",
  "created_at": "2025-03-10T19:45:00Z",
  "updated_at": "2025-03-10T19:45:00Z",
  "is_deleted": false
}
```

#### Get Transaction Details
- **URL**: `/api/transactions/{transaction_id}`
- **Method**: `GET`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174007",
  "type": "DEBIT",
  "amount": "120.50",
  "category_id": "123e4567-e89b-12d3-a456-426614174004",
  "user_id": "123e4567-e89b-12d3-a456-426614174000",
  "date": "2025-03-10T19:30:00",
  "description": "Movie night with friends",
  "created_at": "2025-03-10T19:45:00Z",
  "updated_at": "2025-03-10T19:45:00Z",
  "is_deleted": false
}
```

#### Update Transaction
- **URL**: `/api/transactions/{transaction_id}`
- **Method**: `PATCH`
- **Headers**: `Authorization: Bearer {access_token}`
- **Data**:
```json
{
  "amount": "135.75",
  "description": "Movie night with friends including dinner"
}
```
- **Response**:
```json
{
  "id": "123e4567-e89b-12d3-a456-426614174007",
  "type": "DEBIT",
  "amount": "135.75",
  "category_id": "123e4567-e89b-12d3-a456-426614174004",
  "user_id": "123e4567-e89b-12d3-a456-426614174000",
  "date": "2025-03-10T19:30:00",
  "description": "Movie night with friends including dinner",
  "created_at": "2025-03-10T19:45:00Z",
  "updated_at": "2025-03-10T20:15:00Z",
  "is_deleted": false
}
```

#### Delete Transaction
- **URL**: `/api/transactions/{transaction_id}`
- **Method**: `DELETE`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```
Empty response with 204 status code
```

### Summary Report Endpoints

#### Get Overall Summary Report
- **URL**: `/api/transaction-reports?start_date=2025-03-01&end_date=2025-03-31`
- **Method**: `GET`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```json
{
  "period": {
    "start_date": "2025-03-01",
    "end_date": "2025-03-31"
  },
  "total_transactions": 3,
  "total_income": 2000.0,
  "total_expense": 306.25,
  "net_balance": 1693.75,
  "by_type": {
    "CREDIT": 2000.0,
    "DEBIT": 306.25
  },
  "categories": [
    {
      "name": "Groceries",
      "period": {
        "start_date": "2025-03-01",
        "end_date": "2025-03-31"
      },
      "total_transactions": 1,
      "total_income": 0.0,
      "total_expense": 50.0,
      "net_balance": -50.0,
      "by_type": {
        "CREDIT": 0.0,
        "DEBIT": 50.0
      }
    },
    {
      "name": "Movies & Entertainment",
      "period": {
        "start_date": "2025-03-01",
        "end_date": "2025-03-31"
      },
      "total_transactions": 1,
      "total_income": 0.0,
      "total_expense": 135.75,
      "net_balance": -135.75,
      "by_type": {
        "CREDIT": 0.0,
        "DEBIT": 135.75
      }
    },
    {
      "name": "Salary",
      "period": {
        "start_date": "2025-03-01",
        "end_date": "2025-03-31"
      },
      "total_transactions": 1,
      "total_income": 2000.0,
      "total_expense": 0.0,
      "net_balance": 2000.0,
      "by_type": {
        "CREDIT": 2000.0,
        "DEBIT": 0.0
      }
    }
  ]
}
```

#### Get Category-specific Summary Report
- **URL**: `/api/transaction-reports?start_date=2025-03-01&end_date=2025-03-31&category_id=123e4567-e89b-12d3-a456-426614174004`
- **Method**: `GET`
- **Headers**: `Authorization: Bearer {access_token}`
- **Response**:
```json
{
  "period": {
    "start_date": "2025-03-01",
    "end_date": "2025-03-31"
  },
  "total_transactions": 1,
  "total_income": 0.0,
  "total_expense": 135.75,
  "net_balance": -135.75,
  "by_type": {
    "CREDIT": 0.0,
    "DEBIT": 135.75
  },
  "category": {
    "id": "123e4567-e89b-12d3-a456-426614174004",
    "name": "Movies & Entertainment",
    "is_predefined": false
  }
}
```

## Module Descriptions

### Authentication Module

The Authentication Module handles user registration, authentication, and access control. It provides:
- Secure user registration with email verification
- Login functionality with JWT token issuance
- Password reset capabilities via email
- Session management and logout functionality

### User Module

The User Module manages user profiles and account settings:
- User profile management (name, username)
- Password updates with security validation
- Email address changes with dual-verification
- Account deletion with proper cleanup

### Category Module

The Category Module enables transaction categorization:
- Creation of custom categories for organizing transactions
- Support for both user-specific and system-wide predefined categories
- Category management operations (create, read, update, delete)
- Validation to prevent duplicate category names

### Transaction Module

The Transaction Module is the core financial tracking component:
- Records financial transactions (both income and expenses)
- Associates transactions with categories and users
- Supports full CRUD operations on transaction records
- Implements validation for transaction data integrity

### Summary Report Module

The Summary Report Module provides financial insights:
- Generates financial summaries over specified time periods
- Calculates income, expenses, and net balance
- Provides category-specific breakdowns of financial activity
- Supports filtering by categories and date ranges

## Authentication

All API endpoints (except registration, login, and password reset) require authentication using JWT tokens. Include the access token in the Authorization header as follows:

```
Authorization: Bearer {access_token}
```

## Error Handling

The API returns appropriate HTTP status codes and error messages:

- `200 OK`: Request succeeded
- `201 Created`: Resource successfully created
- `204 No Content`: Request succeeded with no response body
- `400 Bad Request`: Invalid request data
- `401 Unauthorized`: Authentication required or failed
- `403 Forbidden`: Permission denied
- `404 Not Found`: Resource not found
- `500 Internal Server Error`: Server-side error

Error responses include detailed messages to help troubleshoot issues:

```json
{
  "error": {
    "field_name": "Error message specific to the field"
  }
}
```

## Security Features

The application implements several security measures:
- Password hashing using Flask-Bcrypt
- JWT-based authentication
- Rate limiting for sensitive operations
- Email verification for new accounts
- Two-factor verification for email changes
- Session invalidation on password changes

## Summary

The Personal Finance Tracker API provides a comprehensive platform for financial management with secure user authentication, transaction tracking, and financial reporting. The modular design allows for easy maintenance and future expansion of features.
