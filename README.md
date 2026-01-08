# Home Library Management System – Project Overview

## Project Summary

This project is a full-featured home library management system called Homebrary built with Django. It enables users to organize, track, and share their personal book collections, manage multiple libraries and bookshelves, and interact with friends for social reading and lending. The system is designed for individuals and families who want a robust, user-friendly solution for managing their physical books. See website here: https://automind.pythonanywhere.com/


## Main Features & Functionality
- **Multi-library and bookshelf support:** Organize books by location (e.g., rooms, shelves).
- **Book management:** Add, move, and delete books; fetch book details by ISBN from Open Library.
- **Borrowing and lending (BorrowRequests app):**
   - Request to borrow books from friends (only friends can request).
   - Owners can accept or decline requests.
   - Borrow status is tracked (pending, accepted, declined).
   - Only one accepted borrow per book at a time.
   - Robust permission checks and error handling.
- **Friend system:** Send, accept, and reject friend requests; view friends’ libraries.
- **Account management:** Email-based authentication and account management using django-allauth, with custom templates.
- **AJAX-powered book info lookup:** Fast, user-friendly book entry.
- **Modern frontend:** Uses custom HTML, CSS, and JavaScript for a responsive, interactive user experience, including client-side search and dynamic forms.

## Python Best Practices
- **Separation of features:** The project is split into distinct Django apps for core functionality:
   - **Management app:** Handles all book, library, and bookshelf management.
   - **Friendship app:** Manages the friend system, friend requests, and related social features.
   - **BorrowRequests app:** Handles all borrow/lend request workflows, status tracking, and permissions.
   - This modular approach makes the codebase easier to maintain, extend, and test.
- **Custom user model:** Extends Django’s user system for future flexibility.
- **Error handling:** Graceful error messages and robust exception handling throughout the codebase.
- **Reusable components:** Helper functions and modular forms and styles for easily reusable code.
- **External API integration:** Clean, well-documented code for fetching and storing book data from Open Library.

## Design & Architecture
- **Django backend:** Handles all business logic, user management, and data storage.
- **MySQL database:** Used in production for reliability and scalability.
- **PythonAnywhere deployment:** The project is deployed on PythonAnywhere, with a deployment script included for automation.
- **Static files:** HTML, CSS, and JavaScript are organized for maintainability and performance, providing a modern, interactive UI.
- **Client-side library search:** Book searching is performed in the browser using JavaScript for instant results, as the expected data size is small for home users.
## Security
- **Ownership checks:** Every CRUD operation (create, read, update, delete) verifies that the requesting user owns the object or has permission (e.g., is a friend for shared libraries). This prevents unauthorized access or modification.
- **CSRF protection:** All forms and AJAX endpoints use Django's CSRF token to protect against cross-site request forgery attacks.
- **Authentication:** Relies on Django AllAuth's robust authentication system, with email verification required for all accounts.
- **Environment variables:** Sensitive settings are managed via environment files for security.

## System Design Highlights
- **Efficient data storage:** Minimal book metadata, user-specific copies only when needed.
- **User-centric UX:** Fast, simple, and intuitive for non-technical users.
- **Extensible:** Modular codebase allows for easy addition of new features (e.g., notifications, richer metadata, mobile support).
- **BorrowRequests app:** Clean separation of borrow/lend logic, robust tests, and permission checks.
- **Frontend/Backend synergy:** The combination of Django backend and custom frontend code enables a seamless, interactive experience while maintaining security and scalability.
- **Static files:** Managed and served efficiently for fast page loads.
- **Client-side search:** Book searching is performed in the browser using JavaScript for instant results, as the expected data size is small for home users.

## Key Design Choices
1. **Book Data Fetching:**
   - When a user enters an ISBN, the system first checks the local database for book info (previously fetched from Open Library). If not found, it fetches from Open Library, saves the result, and only stores minimal metadata (title, authors, ISBN) to conserve space. If a user edits the title/authors, a personal copy is saved, preserving the original data.
2. **No Pagination for Library:**
   - Since the system targets home users with relatively small collections, all books and libraries are loaded at once for simplicity and speed. Pagination would be added for larger datasets.
3. **Client-side Search:**
   - Searching is handled in the browser with JavaScript, providing instant feedback and reducing server load, which is practical for small datasets.
4. **Custom Authentication with allauth:**
   - Uses django-allauth for robust email-based authentication, but customizes templates for a seamless user experience. Only email is used for login and verification, avoiding SMS for cost and simplicity.
5. **Automatic creation of first Library and bookshelf:**
    - Use Django signals to automatically create a user their first library and bookshelf once they have verified their email address. Greatly reducing user onboarding friction.

---

## Testing & Continuous Integration

All major features, are covered by automated tests. Tests are run automatically for every pull request using GitHub Actions CI. To run tests locally:

```
python manage.py test
```

---

## Scaling & Production-Readiness (for Larger Teams/Users)
If this project were to grow into a larger, high-traffic system, the following architectural and coding improvements would be made:

### Architecture & Infrastructure
- **AWS Deployment:** Migrate to AWS for robust, scalable infrastructure. Use EC2 for app hosting, S3 for static/media files, and Route 53 for DNS.
- **Load Balancing:** Deploy behind an AWS Application Load Balancer for high availability and zero-downtime deployments.
- **Database Scaling:** Continue with MySQL, but split into read/write replicas and enable Multi-AZ redundancy for failover and performance.
- **Monitoring & Observability:** Integrate Grafana with the LGTM (Loki, Grafana, Tempo, Mimir) stack for comprehensive logging, metrics, and tracing. Use AWS CloudWatch for additional monitoring and alerting.
- **CI/CD Pipelines:** Use GitHub Actions for automated testing and deployment.
- **Secrets Management:** Store secrets in AWS Secrets Manager.
- **Backups & Disaster Recovery:** Automated, regular database and file backups with tested restore procedures.
- **Containerization:** Use Docker for consistent local and production environments.

### Coding & Application Improvements
- **Caching:** Add Redis for caching frequent queries (e.g. user dashboards) to reduce database load and improve response times.
- **Preloading Popular Data:** Preload popular books by ISBN into the database to speed up user experience and reduce API calls.
- **Asynchronous Tasks:** Use AWS SQS/Lambda for background jobs (e.g., sending emails, syncing with external APIs).
- **Testing:** Expand automated test coverage (unit, integration, and end-to-end tests).
- **Documentation:** Maintain up-to-date developer and API documentation for onboarding and collaboration.
- **Feature Flags:** Use feature flags for safe, incremental rollouts of new features.

---

These improvements would ensure the system is robust, scalable, and maintainable for a larger team and userbase, following industry best practices for modern web applications.
