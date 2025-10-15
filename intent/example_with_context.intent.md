# User API with Database

Create a REST API for managing users.

## Context
```yaml
framework: dbbasic-web
database: dbbasic-tsv
storage_path: ./data/users.tsv
dependencies:
  - dbbasic-web
  - bcrypt
python_version: 3.10+
```

## Endpoints

GET /users
- Return all users
- Fields: id, name, email, created_at
- Exclude: password_hash

GET /users/:id
- Return single user by id
- 404 if not found

POST /users
- Accept: name, email, password
- Validate email format
- Hash password with bcrypt
- Store in TSV
- Return user_id

PUT /users/:id
- Update name or email
- Can't change password here
- Return updated user

DELETE /users/:id
- Soft delete (set deleted_at)
- Return success message

## Security
- Rate limit: 100 requests/minute per IP
- Validate all inputs
- Return proper HTTP status codes

## Error Handling
- 400 for bad input
- 404 for not found
- 500 for server errors
- Always return JSON error messages
