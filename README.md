# Bank Management System

Bank Management System is a comprehensive application designed to manage various banking operations such as account creation, card management, investment handling, and user authentication through JWT.

## Table of Contents

- [Technologies Used](#technologies-used)
- [Project Structure](#project-structure)
- [Features](#features)
- [APIs](#apis)
- [Service Layer](#service-layer)
- [Repository Layer](#repository-layer)
- [Exception Handling](#exception-handling)
- [JWT Authentication](#jwt-authentication)
- [Security Configuration](#security-configuration)
- [Setup Instructions](#setup-instructions)

## Technologies Used

- Java 17
- Spring Boot 2.7.x
- Spring Security
- Spring Data JPA
- Hibernate
- JWT (JSON Web Token)
- Lombok
- MySQL Database

## Project Structure

The project follows a typical layered architecture:

- **Model Layer:** Contains entity classes for User, Account, Card, Investment, Nominee, and Role.
- **DTO Layer:** Data Transfer Objects for API communication.
- **Repository Layer:** JPA repositories for database operations.
- **Service Layer:** Business logic implementation.
- **Controller Layer:** REST APIs for handling requests.
- **Config Layer:** Security configuration with JWT.

## Features

- User registration with role-based access control.
- Account creation and management with unique account numbers.
- Card management, including new card application and settings update.
- Investment management tied to user accounts.
- JWT-based authentication and authorization.
- Admin APIs for user and account management.

## APIs

### UserController

**Public APIs**:

- **POST** `/user/register`: Registers a new User and assigns it the ROLE_CUSTOMER by default, encoding the password using BCryptPasswordEncoder.
- **POST** `/user/login`: Logs in the user after authenticating through the authService and assigns a JWT token.

### UserAccountController

**User APIs**:

- **POST** `/account/create/{userId}`: Creates an account for the user with a unique account number and assigns an associated card.
- **GET** `/account/all/{userId}`: Fetches the list of all accounts associated with the given user.
- **GET** `/account/balance`: Fetches the account balance for the given account number.
- **GET** `/account/nominee`: Fetches the nominee for the given account number.
- **PUT** `/account/updateNominee/{accountId}`: Updates the nominee for the given accountId.
- **GET** `/account/getKycDetails`: Fetches the User by account number, returning user details without account and investment lists.
- **PUT** `/account/updateKyc/{accountId}`: Updates the KYC details for the given accountId.
- **GET** `/account/getAccount/summary`: Fetches the Account by account number without user details.

### UserCardController

**User APIs**:

- **GET** `/card/block`: Blocks the card associated with the given account number if found, otherwise throws an exception.
- **POST** `/card/apply/new`: Applies for a new card if the account doesn't already have one, otherwise throws an exception.
- **PUT** `/card/setting`: Updates the card limit and PIN based on the card type.

### UserInvestmentController

**User APIs**:

- **POST** `/invest/now`: Creates an investment associated with the given account ID, validating the account balance.

### AdminController

**Admin APIs**:

- **POST** `/admin/add`: Registers a new User with ROLE_ADMIN, encoding the password using BCryptPasswordEncoder.
- **GET** `/admin/getAllUser`: Fetches all users in the database.
- **GET** `/admin/getUserByName/{username}`: Fetches a user by username.
- **DELETE** `/admin/deleteUser/{userId}`: Deletes a user by userId, if found.
- **PUT** `/admin/account/deactivate`: Deactivates an account for the given userId and accountId.
- **PUT** `/admin/account/activate`: Activates an account for the given userId and accountId.
- **GET** `/admin/account/getActiveAccountsList`: Fetches the list of active accounts.
- **GET** `/admin/account/getInActiveAccountsList`: Fetches the list of inactive accounts.
- **GET** `/admin/account/getActiveAccountsList`: Fetches the list of accounts by account type.
- **GET** `/admin/account/getInActiveAccountsList`: Fetches the list of accounts by branch type.

## Service Layer

The following service classes handle the business logic:

- `AccountService`
- `CardService`
- `InvestmentService`
- `NomineeService`
- `UserService`
- `AdminService`

## Repository Layer

The following JPA repositories are defined:

- `AccountRepository`
- `CardRepository`
- `InvestmentRepository`
- `NomineeRepository`
- `UserRepository`

## Exception Handling

Custom exceptions are defined for handling scenarios where a resource is not found or invalid operations are performed.

## JWT Authentication

The JWT functionality is implemented using:

- `JwtAuthenticationHelper`: Generates and validates JWT tokens.
- `JwtAuthenticationFilter`: Filters and validates JWT tokens in requests.

## Security Configuration

The security configuration is handled in the `BankSecurityConfig` class, which includes:

- Password encoding using `PasswordEncoder`.
- `AuthenticationManager` setup.
- Configuration of `securityFilterChain` to allow `/user/register` and `/user/login` as open endpoints.

## Setup Instructions

1. **Clone the Repository:**

```bash
git clone https://github.com/yourusername/BankManagementSystem.git
cd BankManagementSystem
```
2. **Configure the Database:**

    Update the database settings in `src/main/resources/application.yml` to match your local environment.

3. **Build the Project:**

    ```bash
    mvn clean install
    ```

4. **Run the Application:**

    ```bash
    mvn spring-boot:run
    ```
