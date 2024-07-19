# Automobile Catalog API

This project implements a RESTful API for managing an automobile catalog. The API provides endpoints for creating, reading, updating, and deleting automobile records, as well as filtering and paginating data.

## Endpoints

- GET /automobiles: Retrieve a list of automobiles with filtering and pagination capabilities.
  - Filter by any field using query parameters.
  - Paginate results using limit and offset query parameters.

- DELETE /automobiles/:id: Delete an automobile by its ID.

- PATCH /automobiles/:id: Update one or more fields of an automobile by its ID.

- POST /automobiles: Create a new automobile with the following payload:
  
  {
    "regNums": ["X123XX150"] // array of government numbers
  }
  
  Upon creation, the API will make a request to an external API (described below) to retrieve additional information.

## External API


```
openapi: 3.0.3
info:
  title: Car info
  version: 0.0.1
paths:
  /info:
    get:
      parameters:
        - in: query
          name: regNum
          required: true
          schema:
            type: string
      responses:
        200:
          description: Ok
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Car'
        400:
          description: Bad request
        500:
          description: Internal server error
components:
  schemas:
    Car:
      required:
        - regNum
        - brand
        - owner
      properties:
        regNum:
          type: string
        brand:
          type: string
        owner:
          $ref: '#/components/schemas/People'
    People:
      required:
        - name
        - surname
      properties:
        name:
          type: string
        surname:
          type: string
        patronymic:
          type: string
```


## Implementation Requirements

1. Implement the REST API endpoints described above.
2. Use the external API to retrieve additional information when creating a new automobile.
3. Store enriched information in a PostgreSQL database, creating the database structure using migrations at startup.
4. Log debug and info messages throughout the code.
5. Externalize configuration data to a .env file.
6. Generate Swagger documentation for the implemented API.
