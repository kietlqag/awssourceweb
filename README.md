# CarRentalWeb

Spring Boot and Thymeleaf car rental web application used as the deployable source app for the AWS workshop. The codebase includes customer checkout, admin/staff dashboards, account flows, email OTP/reset workflows, order notifications, and several design-pattern implementations around ordering and payment selection.

## English Summary

| Area | Implementation in this repository |
|---|---|
| Runtime | Spring Boot 3.4.5, Java 21, Maven |
| UI | Thymeleaf templates with static CSS/JavaScript assets |
| Data layer | Spring Data JPA with MySQL configuration and H2 test/runtime dependency |
| Main domains | Cars, accounts, orders, promotions, services, notifications, pending accounts, password reset tokens |
| Checkout | Customer checkout calculates rental days, optional services, promotions, and persists orders |
| Payments | Strategy pattern selects cash or wallet payment behavior through `PaymentContextService`; no external payment gateway callback is implemented here |
| Notifications | Observer pattern updates car/order state and creates staff/customer notifications |
| Pricing extension | Decorator pattern adds optional service price to the base rental total |
| Email flows | Registration OTP and password reset email use Spring Mail |
| Security note | Passwords are encoded with BCrypt; the current `SecurityFilterChain` permits all requests and disables CSRF, so route-level authorization is not enforced by Spring Security yet |
| Secrets | Mail username/password and database credentials must be supplied through environment variables |

## Requirements

- Java 21
- Maven 3.9+
- MySQL for normal runtime
- SMTP account when OTP/reset email needs to be sent

## Configuration

The application reads runtime values from environment variables:

| Variable | Purpose | Default |
|---|---|---|
| `SERVER_PORT` | HTTP port | `5000` |
| `DB_HOST` | MySQL host and port | `localhost:3306` |
| `DB_NAME` | Database name | `carrentalweb` |
| `DB_USERNAME` | Database username | `root` |
| `DB_PASSWORD` | Database password | `root` |
| `DDL_AUTO` | Hibernate DDL mode | `update` |
| `MAIL_HOST` | SMTP host | `smtp.gmail.com` |
| `MAIL_PORT` | SMTP port | `587` |
| `MAIL_USERNAME` | SMTP username | empty |
| `MAIL_PASSWORD` | SMTP password/app password | empty |

Do not commit real `.env` files, Gmail app passwords, database passwords, AWS keys, or deployment credentials.

## Run Locally

```bash
mvn spring-boot:run
```

Then open:

```text
http://localhost:5000
```

For email features, provide `MAIL_USERNAME` and `MAIL_PASSWORD` through your shell, IDE run configuration, or hosting platform environment variables.
