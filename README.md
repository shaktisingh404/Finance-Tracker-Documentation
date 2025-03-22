# Flask Finance Tracker Documentation

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [User Story](#user-story)
- [Modules](#modules)
  - [Authentication Module](#authentication-module)
  - [User Module](#user-module)
  - [Transaction Module](#transaction-module)
  - [Budget Module](#budget-module)
  - [Saving Plan Module](#saving-plan-module)
  - [Category Module](#category-module)
  - [Recurring Transaction Module](#recurring-transaction-module)
  - [Transaction Summary Report Module](#transaction-summary-report-module)
- [Core Components of Transaction Reports](#core-components-of-transaction-reports)
- [Authentication & Security](#authentication--security)
- [Key Relationships](#key-relationships)
- [Implementation Details for Transaction Reports](#implementation-details-for-transaction-reports)
- [Initialization Script](#initialization-script)
- [Testing](#testing)

---

## Overview

The **Flask Finance Tracker** is a robust personal finance management application built with Flask, SQLAlchemy, and JWT-based authentication. It integrates Celery for asynchronous task processing, enabling users to efficiently track transactions, manage budgets, set savings goals, and generate detailed financial reports. Designed for security and scalability, this tool caters to individual users, parents managing child accounts, and administrators overseeing the system.

**Version**: 1.0  
**Release Date**: March 2025

---

## Prerequisites

To run the Flask Finance Tracker, ensure the following requirements are met:

- **Python**: Version 3.9 or higher
- **Flask**: Version 2.3 or higher
- **Database**: PostgreSQL 15+ (configurable for other SQLAlchemy-supported databases)
- **Dependencies**: Listed in `requirements.txt`
- **Celery**: Requires a message broker (e.g., Redis)

### Setup Instructions
1. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   cd flask-finance-tracker
   ```
2. **Configure Environment Variables**:
   Create a `.env` file with the following keys:
   ```
   DATABASE_URL=postgresql://user:password@localhost:5432/finance_tracker
   JWT_SECRET=<your-secret-key>
   EMAIL_API_KEY=<your-email-service-key>
   CELERY_BROKER_URL=redis://localhost:6379/0
   ```
3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
4. **Run the Application**:
   ```bash
   flask run
   ```

---

## User Story

As a user of the Flask Finance Tracker, I want to:

1. **Track Financial Transactions**:
   - Log income and expenses with details.
   - Categorize transactions for better organization.
   - Review my transaction history.

2. **Manage My Account**:
   - Register with email verification.
   - Log in securely using credentials.
   - Update my profile and password.
   - Create and manage child accounts (for parent users).

3. **Budget Effectively**:
   - Set monthly budgets per category.
   - Monitor spending against budget limits.
   - Get alerts as I near budget thresholds.

4. **Plan Savings**:
   - Define savings goals with target amounts and deadlines.
   - Track progress toward my goals.
   - Adjust plans as my needs evolve.

5. **Automate Financial Tasks**:
   - Schedule recurring transactions (e.g., rent, subscriptions).
   - Generate and export financial reports.
   - Receive reports via email or download.

6. **Ensure Security and Role-Based Access**:
   - Regular users: Manage personal finances.
   - Parent users: Oversee child accounts.
   - Admin users: Administer the system.

---

## Modules

The application is modular, with each component leveraging SQLAlchemy for database operations and Flask-RESTful for API endpoints. Below are the key modules and their functionalities.

### Authentication Module

Handles user authentication and session management.

#### Key Features
- User registration with email verification.
- Secure login with JWT tokens.
- Password reset via email.
- Protected admin registration.

#### API Endpoints

- **POST /api/auth/signup**  
  Registers a new user.  
  **Request:**
  ```json
  {
    "username": "johndoe",
    "email": "john.doe@example.com",
    "password": "SecurePassword123!",
    "name": "John Doe",
    "gender": "MALE",
    "date_of_birth": "1990-01-15"
  }
  ```
  **Response (200 OK):**
  ```json
  {
    "message": "Registration initiated, please check your email for verification"
  }
  ```

- **POST /api/auth/login**  
  Authenticates a user and issues tokens.  
  **Request:**
  ```json
  {
    "username": "johndoe",
    "password": "SecurePassword123!"
  }
  ```
  **Response (200 OK):**
  ```json
  {
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
  ```

- **PATCH /api/auth/reset-password**  
  Resets a user's password (requires JWT and email verification).  
  **Request:**
  ```json
  {
    "new_password": "NewSecurePassword123!"
  }
  ```
  **Response (200 OK):**
  ```json
  {
    "message": "Password reset successfully"
  }
  ```

#### Notes
- Access tokens expire in 15 minutes; refresh tokens last 30 days.
- Account deletion is managed via the User Module.

---

### User Module

Manages user profiles and relationships.

#### Key Features
- Profile creation and updates.
- Password and email management.
- Parent-child account linking.
- Soft deletion of accounts.

#### API Endpoints

- **POST /api/users**  
  Creates a new user (admin or parent-child creation).  
  **Request:**
  ```json
  {
    "username": "newuser",
    "email": "newuser@example.com",
    "password": "SecurePassword123!",
    "name": "New User",
    "gender": "OTHER",
    "date_of_birth": "1995-01-01",
    "role": "USER"
  }
  ```
  **Response (201 Created):**
  ```json
  {
    "id": "4c2748c4-a31a-4d06-a034-1e5dfb0b167c",
    "username": "newuser",
    "email": "newuser@example.com",
    "name": "New User",
    "gender": "OTHER",
    "date_of_birth": "1995-01-01",
    "role": "USER",
    "created_at": "2025-03-22T10:00:00Z",
    "updated_at": "2025-03-22T10:00:00Z"
  }
  ```

- **GET /api/users/{user_id}**  
  Retrieves user details.  
  **Response (200 OK):**
  ```json
  {
    "id": "4c2748c4-a31a-4d06-a034-1e5dfb0b167c",
    "username": "newuser",
    "email": "newuser@example.com",
    "name": "New User",
    "gender": "OTHER",
    "date_of_birth": "1995-01-01",
    "role": "USER",
    "created_at": "2025-03-22T10:00:00Z",
    "updated_at": "2025-03-22T10:00:00Z"
  }
  ```

- **PATCH /api/users/{user_id}**  
  Updates user profile.  
  **Request:**
  ```json
  {
    "name": "Updated User",
    "username": "updateduser",
    "date_of_birth": "1995-05-15",
    "gender": "FEMALE"
  }
  ```
  **Response (200 OK):**
  ```json
  {
    "id": "4c2748c4-a31a-4d06-a034-1e5dfb0b167c",
    "username": "updateduser",
    "email": "newuser@example.com",
    "name": "Updated User",
    "gender": "FEMALE",
    "date_of_birth": "1995-05-15",
    "role": "USER",
    "created_at": "2025-03-22T10:00:00Z",
    "updated_at": "2025-03-22T12:00:00Z"
  }
  ```

- **DELETE /api/users/{user_id}**  
  Soft deletes a user account.  
  **Response (204 No Content):**  
  (No body)

---

### Transaction Module

Tracks financial activities.

#### Key Features
- Record income (credits) and expenses (debits).
- Categorize transactions.
- Paginated transaction history.
- Link transactions to savings plans.

#### API Endpoints

- **POST /api/users/{user_id}/transactions**  
  Creates a new transaction.  
  **Request:**
  ```json
  {
    "title": "Grocery Shopping",
    "amount": 75.25,
    "transaction_date": "2025-03-01",
    "transaction_type": "DEBIT",
    "description": "Weekly grocery shopping",
    "category_id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5"
  }
  ```
  **Response (201 Created):**
  ```json
  {
    "id": "aaba763e-1558-4fe2-93a4-b89e0e986921",
    "user_id": "4c2748c4-a31a-4d06-a034-1e5dfb0b167c",
    "title": "Grocery Shopping",
    "amount": "75.25",
    "transaction_date": "2025-03-01",
    "transaction_type": "DEBIT",
    "description": "Weekly grocery shopping",
    "category": {
      "id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
      "name": "Groceries",
      "is_predefined": true
    },
    "saving_plan": null,
    "created_at": "2025-03-01T14:32:22Z",
    "updated_at": "2025-03-01T14:32:22Z"
  }
  ```

- **GET /api/users/{user_id}/transactions**  
  Lists transactions with pagination.  
  **Query Parameters:**
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)  
  **Response (200 OK):**
  ```json
  {
    "items": [
      {
        "id": "aaba763e-1558-4fe2-93a4-b89e0e986921",
        "title": "Grocery Shopping",
        "amount": "75.25",
        "transaction_date": "2025-03-01",
        "transaction_type": "DEBIT",
        "description": "Weekly grocery shopping",
        "category": {
          "id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
          "name": "Groceries",
          "is_predefined": true
        },
        "created_at": "2025-03-01T14:32:22Z",
        "updated_at": "2025-03-01T14:32:22Z"
      }
    ],
    "count": 1,
    "next": null,
    "previous": null
  }
  ```

- **PATCH /api/users/{user_id}/transactions/{transaction_id}**  
  Updates a transaction.  
  **Request:**
  ```json
  {
    "title": "Updated Grocery Shopping",
    "amount": 80.50,
    "description": "Weekly grocery shopping with extra items"
  }
  ```
  **Response (200 OK):**
  ```json
  {
    "id": "aaba763e-1558-4fe2-93a4-b89e0e986921",
    "title": "Updated Grocery Shopping",
    "amount": "80.50",
    "transaction_date": "2025-03-01",
    "transaction_type": "DEBIT",
    "description": "Weekly grocery shopping with extra items",
    "category": {
      "id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
      "name": "Groceries",
      "is_predefined": true
    },
    "saving_plan": null,
    "created_at": "2025-03-01T14:32:22Z",
    "updated_at": "2025-03-22T10:15:30Z"
  }
  ```

- **DELETE /api/users/{user_id}/transactions/{transaction_id}**  
  Deletes a transaction.  
  **Response (204 No Content):**  
  (No body)

---

### Budget Module

Manages budgeting by category and time period.

#### Key Features
- Create monthly budgets per category.
- Track spending against budget limits.
- Review budget history and performance.

#### API Endpoints

- **POST /api/users/{user_id}/budgets**  
  Creates a new budget.  
  **Request:**
  ```json
  {
    "category_id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
    "amount": 500.00,
    "month": 3,
    "year": 2025
  }
  ```
  **Response (201 Created):**
  ```json
  {
    "id": "67e55044-10b1-426f-9247-bb680e5fe0c8",
    "user_id": "4c2748c4-a31a-4d06-a034-1e5dfb0b167c",
    "category": {
      "id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
      "name": "Groceries",
      "is_predefined": true
    },
    "amount": "500.00",
    "spent_amount": "0.00",
    "month": 3,
    "year": 2025,
    "created_at": "2025-03-01T00:00:00Z",
    "updated_at": "2025-03-01T00:00:00Z"
  }
  ```

- **GET /api/users/{user_id}/budgets**  
  Lists budgets with pagination.  
  **Query Parameters:**
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)  
  **Response (200 OK):**
  ```json
  {
    "items": [
      {
        "id": "67e55044-10b1-426f-9247-bb680e5fe0c8",
        "category": {
          "id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
          "name": "Groceries",
          "is_predefined": true
        },
        "amount": "500.00",
        "spent_amount": "325.75",
        "month": 3,
        "year": 2025,
        "created_at": "2025-03-01T00:00:00Z",
        "updated_at": "2025-03-17T10:35:12Z"
      }
    ],
    "count": 1,
    "next": null,
    "previous": null
  }
  ```

- **PATCH /api/users/{user_id}/budgets/{budget_id}**  
  Updates a budget.  
  **Request:**
  ```json
  {
    "amount": 600.00
  }
  ```
  **Response (200 OK):**
  ```json
  {
    "id": "67e55044-10b1-426f-9247-bb680e5fe0c8",
    "category": {
      "id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
      "name": "Groceries",
      "is_predefined": true
    },
    "amount": "600.00",
    "spent_amount": "325.75",
    "month": 3,
    "year": 2025,
    "created_at": "2025-03-01T00:00:00Z",
    "updated_at": "2025-03-22T12:00:00Z"
  }
  ```

- **DELETE /api/users/{user_id}/budgets/{budget_id}**  
  Deletes a budget.  
  **Response (204 No Content):**  
  (No body)

---

### Saving Plan Module

Facilitates goal-based savings.

#### Key Features
- Define savings goals with deadlines.
- Monitor progress toward goals.
- Adjust frequency and deadlines.

#### API Endpoints

- **POST /api/users/{user_id}/saving_plans**  
  Creates a new saving plan.  
  **Request:**
  ```json
  {
    "name": "Vacation Fund",
    "amount": 2000.00,
    "current_deadline": "2025-12-31",
    "frequency": "MONTHLY"
  }
  ```
  **Response (201 Created):**
  ```json
  {
    "id": "b8f0e372-c79a-48c7-a46b-b3a789c8e94b",
    "user_id": "4c2748c4-a31a-4d06-a034-1e5dfb0b167c",
    "name": "Vacation Fund",
    "amount": "2000.00",
    "saved_amount": "0.00",
    "current_deadline": "2025-12-31",
    "original_deadline": "2025-12-31",
    "frequency": "MONTHLY",
    "status": "ACTIVE",
    "created_at": "2025-03-18T14:15:22Z",
    "updated_at": "2025-03-18T14:15:22Z"
  }
  ```

- **GET /api/users/{user_id}/saving_plans**  
  Lists saving plans with pagination.  
  **Query Parameters:**
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)  
  **Response (200 OK):**
  ```json
  {
    "items": [
      {
        "id": "b8f0e372-c79a-48c7-a46b-b3a789c8e94b",
        "name": "Vacation Fund",
        "amount": "2000.00",
        "saved_amount": "750.00",
        "current_deadline": "2025-12-31",
        "original_deadline": "2025-12-31",
        "frequency": "MONTHLY",
        "status": "ACTIVE",
        "created_at": "2025-01-15T09:30:00Z",
        "updated_at": "2025-03-17T10:35:12Z"
      }
    ],
    "count": 1,
    "next": null,
    "previous": null
  }
  ```

- **PATCH /api/users/{user_id}/saving_plans/{saving_plan_id}**  
  Updates a saving plan.  
  **Request:**
  ```json
  {
    "name": "Updated Vacation Fund",
    "current_deadline": "2026-01-15"
  }
  ```
  **Response (200 OK):**
  ```json
  {
    "id": "b8f0e372-c79a-48c7-a46b-b3a789c8e94b",
    "name": "Updated Vacation Fund",
    "amount": "2000.00",
    "saved_amount": "750.00",
    "current_deadline": "2026-01-15",
    "original_deadline": "2025-12-31",
    "frequency": "MONTHLY",
    "status": "ACTIVE",
    "created_at": "2025-01-15T09:30:00Z",
    "updated_at": "2025-03-22T12:00:00Z"
  }
  ```

- **DELETE /api/users/{user_id}/saving_plans/{saving_plan_id}**  
  Deletes a saving plan.  
  **Response (204 No Content):**  
  (No body)

---

### Category Module

Organizes transactions and budgets.

#### Key Features
- Predefined and custom categories.
- Category management (create, update, delete).
- Used for reporting and grouping.

#### API Endpoints

- **POST /api/users/{user_id}/categories**  
  Creates a new category.  
  **Request:**
  ```json
  {
    "name": "Entertainment"
  }
  ```
  **Response (201 Created):**
  ```json
  {
    "id": "a8d97b43-b358-45d7-b109-6d7c9ce610c3",
    "name": "Entertainment",
    "user_id": "4c2748c4-a31a-4d06-a034-1e5dfb0b167c",
    "is_predefined": false,
    "created_at": "2025-03-18T14:30:22Z",
    "updated_at": "2025-03-18T14:30:22Z"
  }
  ```

- **GET /api/users/{user_id}/categories**  
  Lists categories with pagination.  
  **Query Parameters:**
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)  
  **Response (200 OK):**
  ```json
  {
    "items": [
      {
        "id": "a8d97b43-b358-45d7-b109-6d7c9ce610c3",
        "name": "Entertainment",
        "is_predefined": false,
        "created_at": "2025-03-18T14:30:22Z",
        "updated_at": "2025-03-18T14:30:22Z"
      }
    ],
    "count": 1,
    "next": null,
    "previous": null
  }
  ```

- **PATCH /api/users/{user_id}/categories/{category_id}**  
  Updates a category.  
  **Request:**
  ```json
  {
    "name": "Updated Entertainment"
  }
  ```
  **Response (200 OK):**
  ```json
  {
    "id": "a8d97b43-b358-45d7-b109-6d7c9ce610c3",
    "name": "Updated Entertainment",
    "is_predefined": false,
    "created_at": "2025-03-18T14:30:22Z",
    "updated_at": "2025-03-22T12:00:00Z"
  }
  ```

- **DELETE /api/users/{user_id}/categories/{category_id}**  
  Deletes a category.  
  **Response (204 No Content):**  
  (No body)

---

### Recurring Transaction Module

Automates repetitive transactions.

#### Key Features
- Schedule recurring credits or debits.
- Support for multiple frequencies (daily, weekly, monthly).
- Automatic transaction creation.

#### API Endpoints

- **POST /api/users/{user_id}/recurring-transactions**  
  Creates a recurring transaction.  
  **Request:**
  ```json
  {
    "title": "Rent Payment",
    "amount": 1200.00,
    "type": "DEBIT",
    "description": "Monthly apartment rent",
    "category_id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
    "frequency": "MONTHLY",
    "starts_at": "2025-03-01T10:10:10Z",
    "ends_at": "2025-12-31T10:10:10Z"
  }
  ```
  **Response (201 Created):**
  ```json
  {
    "id": "c5f37587-e939-4da3-8b3a-141e78d9db12",
    "title": "Rent Payment",
    "amount": "1200.00",
    "type": "DEBIT",
    "description": "Monthly apartment rent",
    "category": {
      "id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
      "name": "Housing",
      "is_predefined": true
    },
    "frequency": "MONTHLY",
    "starts_at": "2025-03-01T10:10:10Z",
    "ends_at": "2025-12-31T10:10:10Z",
    "next_transaction_at": "2025-04-01T10:10:10Z",
    "created_at": "2025-03-18T14:45:12Z",
    "updated_at": "2025-03-18T14:45:12Z"
  }
  ```

- **GET /api/users/{user_id}/recurring-transactions**  
  Lists recurring transactions with pagination.  
  **Query Parameters:**
  - `page`: Page number (default: 1)
  - `per_page`: Items per page (default: 10)  
  **Response (200 OK):**
  ```json
  {
    "items": [
      {
        "id": "c5f37587-e939-4da3-8b3a-141e78d9db12",
        "title": "Rent Payment",
        "amount": "1200.00",
        "type": "DEBIT",
        "description": "Monthly apartment rent",
        "category": {
          "id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
          "name": "Housing",
          "is_predefined": true
        },
        "frequency": "MONTHLY",
        "starts_at": "2025-03-01T10:10:10Z",
        "ends_at": "2025-12-31T10:10:10Z",
        "next_transaction_at": "2025-04-01T10:10:10Z",
        "created_at": "2025-03-18T14:45:12Z",
        "updated_at": "2025-03-18T14:45:12Z"
      }
    ],
    "count": 1,
    "next": null,
    "previous": null
  }
  ```

- **PATCH /api/users/{user_id}/recurring-transactions/{recurring_transaction_id}**  
  Updates a recurring transaction.  
  **Request:**
  ```json
  {
    "title": "Updated Rent Payment",
    "amount": 1300.00
  }
  ```
  **Response (200 OK):**
  ```json
  {
    "id": "c5f37587-e939-4da3-8b3a-141e78d9db12",
    "title": "Updated Rent Payment",
    "amount": "1300.00",
    "type": "DEBIT",
    "description": "Monthly apartment rent",
    "category": {
      "id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
      "name": "Housing",
      "is_predefined": true
    },
    "frequency": "MONTHLY",
    "starts_at": "2025-03-01T10:10:10Z",
    "ends_at": "2025-12-31T10:10:10Z",
    "next_transaction_at": "2025-04-01T10:10:10Z",
    "created_at": "2025-03-18T14:45:12Z",
    "updated_at": "2025-03-22T12:00:00Z"
  }
  ```

- **DELETE /api/users/{user_id}/recurring-transactions/{recurring_transaction_id}**  
  Deletes a recurring transaction.  
  **Response (204 No Content):**  
  (No body)

---

### Transaction Summary Report Module

Provides financial insights and reporting.

#### Key Features
- Generate categorized transaction summaries.
- Analyze spending trends over time.
- Export reports in CSV or PDF formats.
- Email reports asynchronously via Celery.

#### API Endpoints

- **GET /api/users/{user_id}/transaction-reports/summary**  
  Generates a transaction summary.  
  **Query Parameters:**
  - `start_date`: Start date (YYYY-MM-DD)
  - `end_date`: End date (YYYY-MM-DD)  
  **Response (200 OK):**
  ```json
  {
    "start_date": "2025-01-01",
    "end_date": "2025-03-31",
    "total_credit": 7500.00,
    "total_debit": 5250.75,
    "summary": {
      "credit_by_category": [
        {
          "category_id": "a7c86a32-a257-44c6-a10e-5b5e15681cc5",
          "category_name": "Salary",
          "total_amount": 7500.00,
          "transaction_count": 3
        }
      ],
      "debit_by_category": [
        {
          "category_id": "b8d97b43-c358-45d7-b109-6d7c9ce610c3",
          "category_name": "Groceries",
          "total_amount": 950.25,
          "transaction_count": 12
        }
      ]
    }
  }
  ```

- **GET /api/users/{user_id}/transaction-reports/trends**  
  Analyzes spending trends.  
  **Query Parameters:**
  - `start_date`: Start date (YYYY-MM-DD)
  - `end_date`: End date (YYYY-MM-DD)  
  **Response (200 OK):**
  ```json
  {
    "start_date": "2025-01-01",
    "end_date": "2025-03-31",
    "total_credit": 7500.00,
    "total_debit": 5250.75,
    "categories": [
      {
        "id": "b8d97b43-c358-45d7-b109-6d7c9ce610c3",
        "name": "Groceries",
        "credit": 0.00,
        "debit": 950.25,
        "credit_percentage": 0.00,
        "debit_percentage": 18.10
      }
    ]
  }
  ```

- **GET /api/users/{user_id}/transaction-reports/export**  
  Exports and emails a transaction report.  
  **Query Parameters:**
  - `start_date`: Start date (YYYY-MM-DD)
  - `end_date`: End date (YYYY-MM-DD)
  - `file_format`: Format (csv or pdf)  
  **Response (200 OK):**
  ```json
  {
    "message": "Transaction history report will be sent to your email"
  }
  ```

#### Notes
- This module is read-only; it generates reports but does not support CRUD operations.

---

## Core Components of Transaction Reports

### TransactionReportService
- **get_transaction_report**: Generates detailed transaction summaries.
- **get_trends_report**: Analyzes trends.

### TransactionReport
- **generate_csv**: Exports data to CSV format.
- **generate_pdf**: Creates styled PDFs using ReportLab.

### Background Tasks
- **email_transaction_history**: Asynchronously emails reports with retry logic using Celery.

---

## Authentication & Security

- **JWT Tokens**:
  - Access tokens: 15-minute expiry.
  - Refresh tokens: 30-day expiry, stored in HTTP-only cookies.
- **Security Practices**: Enforce HTTPS, hash passwords with bcrypt.
- **Role-Based Access**:
  - `USER`: Manage personal finances.
  - `CHILD_USER`: Manage personal finances.
  - `ADMIN`: Full system control.
- **Parent Oversight**: Parents can view child resources but not edit them.

---

## Key Relationships

| Entity             | Relationships                          |
|--------------------|----------------------------------------|
| **Users**          | Owns transactions, budgets, etc.       |
| **Parent Users**   | Manage a child user                    |
| **Transactions**   | Linked to a category or savings        |
| **Budgets**        | Associated with categories and months  |
| **Recurring Trans**| Generates periodic transactions        |

---

## Implementation Details for Transaction Reports

### Report Generation Logic
1. **Transaction Summary**: Filters by user and date range, computes totals by category.
2. **Trends Analysis**: Calculates percentage distributions of spending.
3. **Report Export**: Formats data into CSV or PDF.

### Data Workflow
1. API request received with query parameters.
2. Parameters validated.
3. Service processes data from the database.
4. Response returned to the client.
5. Export tasks queued via Celery for asynchronous delivery.

---

## Initialization Script

To streamline the setup process, an initialization script has been added to create an admin user and predefined categories.

### Purpose
- Creates a default admin user with full system privileges.
- Populates the database with a set of predefined categories for immediate use.


### Running the Script
1. Ensure the database is configured and migrations are applied:
   ```bash
   python scripts/create_admin_user.py
   ```

### Notes
- The admin user's credentials (`username: admin`, `password: AdminPassword123!`) should be changed after the first login for security.
- Predefined categories are marked with `is_predefined=True` to distinguish them from user-created categories.
- The script checks for existing records to avoid duplicates.

---

## Testing

- **Unit Tests**:  
  Run with:
  ```bash
  pytest
  ```
- **API Testing**:  
  Example using `curl`:
  ```bash
  curl -X POST http://localhost:5000/api/auth/login \
    -H "Content-Type: application/json" \
    -d '{"username": "johndoe", "password": "SecurePassword123!"}'
  ```
  Alternatively, use Postman for interactive testing.
