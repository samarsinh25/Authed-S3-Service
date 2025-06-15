## Key Subdirectories and Their Roles

| Directory      | Description                                                                                  |
|----------------|----------------------------------------------------------------------------------------------|
| **cmd/**       | Contains your application's entry point (`main.go`).                                         |
| **config/**    | Holds configuration files, such as `.env`.                                                   |
| **internal/**  | Stores private application logic not intended for external import.                           |
| **migrations/**| Contains database migration scripts.                                                         |
| **models/**    | Defines Go structs that map to PostgreSQL tables.                                            |
| **services/**  | Houses the core business logic.                                                              |
| **handlers/**  | Exposes your APIs as HTTP routes.                                                            |
| **middlewares/**| Custom middleware functions for authentication and validation.                              |
| **redis/**     | Contains logic related to Redis queues and background workers.                               |
