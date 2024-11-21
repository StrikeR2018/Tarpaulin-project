# Tarpaulin API

https://docs.google.com/document/d/1A-i0wsm3nTcKpkzOjcajy1OpDV1ZXcUPYJeo1vJ9tjQ/edit

## Setup
* run `npm i` to install dependencies
* run `npm run dev` to develop
* run `npm start` to view

* run following command to setup mysql server in Docker container
```bash
docker  run -d --name tarpaulin-server `
-p "3306:3306" `
-e "MYSQL_RANDOM_ROOT_PASSWORD=yes" `
-e "MYSQL_DATABASE=tarpaulin" `
-e "MYSQL_USER=team58" `
-e "MYSQL_PASSWORD=58" `
mysql
```

* setting up redis server in Docker
```bash
docker run -d --name redis-server -p "6379:6379" redis:latest
```

* When the database server is running in Docker, use `docker exec -it tarpaulin-server /bin/bash` to enter bash
    * run `mysql -u team58 -p tarpaulin` to access the database

* A Redis server must be setup to receive requests on port 6379. Run the following command to automatically setup and run a docker container containing a Redis server:
```bash
docker run -d --name redis-server -p "6379:6379" redis:latest
```


Data Entities
The Tarpaulin API will manage the following data entities:

Users

Attributes: id, name, email, password, role (admin, instructor, student)
Description: Users can assume three roles, each with distinct permissions.
Relationships:
Many-to-many relationship with Courses (users can teach or enroll in multiple courses).



Courses

Attributes: id, subject, number, title, term, instructorId
Description: Represents a course in the system.
Relationships:
Contains multiple Assignments.
Has a many-to-many relationship with Users, stored via the usercourse table.



Assignments

Attributes: id, courseId, title, points, due
Description: Each assignment belongs to a specific course and is linked to multiple submissions from students.
Relationships:
One-to-many relationship with Submissions.
Belongs to a Course.
Submissions

Attributes: id, assignmentId, studentId, timestamp, grade, file
Description: Represents a single submission by a student for an assignment.



Relationships:
Belongs to an Assignment.
Associated with a User through studentId.





Services
The API leverages the following technologies:

Node.js + Express.js: Backend framework for RESTful APIs.
MySQL: Database managed via Sequelize ORM, containerized with Docker.
Redis: For rate-limiting logic.
Multer: For file upload and management.
JWT: Role-based authentication.
json2csv: CSV generation for course rosters.
GitHub: Repository management and version control.
Key Features
Rate Limiting

Implemented via Redis.
Limits:
Unauthenticated: 10 requests/min.
Authenticated: 30 requests/min.
Authentication

Role-based access using JWT.
Custom middleware ensures secure endpoints.
Pagination

Available for GET /courses and GET /assignments/{id}/submissions.
File Management

Submissions uploaded via Multer with unique file paths for retrieval.
HATEOAS Links

Dynamically generated links for paginated endpoints.
Validation

Schema-based validation for incoming requests using utility functions (validateAgainstSchema, containsAtLeastOneSchemaField).
Division of Labor

Database Design: Define and manage MySQL schema.
API Implementation: Develop endpoints for all entities.
Authentication: Enforce JWT-based access control.
Rate Limiting: Configure Redis for request throttling.
File Handling: Implement upload and retrieval logic for submissions.
Testing: Write and execute Postman test cases.
Documentation: Create API guides and response samples.
Development Plan



Setup

Start Docker containers for MySQL and Redis.

API Development

CRUD operations for Users, Courses, Assignments, and Submissions.
Add features like pagination, authentication, and rate limiting.
Testing

