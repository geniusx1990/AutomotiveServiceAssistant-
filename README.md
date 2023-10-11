# AutomotiveServiceAssistant-

# API Documentation for Automative Service Assistant

## Description <a name="go-up"></a>

The project "Automotive Service Assistant" is being developed 
for automotive repair workshops. This system provides assistance 
to car owners when replacing specific parts, automatically checking 
the availability of spare parts, calculating the total cost 
(including labor costs), and providing an estimate of the completion 
time for the work.

Content:

1. [Technical requirements](#Technical-requirements)
2. [Implementation details](#Implementation-details)
3. [Endpoints:](#Endpoints)
   - [Endpoints: users](#users)
   - [Endpoints: parts](#parts)
   - [Endpoints: repair-requests](#repair-requests)
4. [Download and install App](#Install)
5. [Run App with Docker](#docker)

## Technical requirements <a name="Technical-requirements"></a>

- Task should be implemented on Typescript
- Framework - express
- Web framework - ?React?
- Database - Postgres
- Use 19.7 LTS version of Node.js
- Docker container

## Implementation details <a name="Implementation-details"></a>

Base URL

```
 http://localhost:3000/api/
```

# Endpoints: <a name="Endpoints"></a>

## Endpoint `api/users`: <a name="users"></a>

- Request: `GET api/users` - get all users
  - Response:

```
  HTTP/1.1 200 OK
    Content-Type: application/json [
    {
      "id": "1",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "role": "Customer"
    },
    {
      "id": "2",
      "name": "Jane Smith",
      "email": "jane.smith@example.com",
      "role": "Admin"
    },
    ...
    ]
```

- Request: `GET api/users/:userId` - get one user, user type will be determined automatically
  - Query Parameters

| Parameter | Type   | Required | Description  |
|-----------|--------|----------|--------------|
| `userId`  | string | Yes      | The user ID. |

- Response:

```
    If the record with id === userId exists:
    HTTP/1.1 200 OK
    Content-Type: application/json

    {
      "id": "{userId}",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "role": "Customer"
    }

    If userId is invalid (not uuid):
    HTTP/1.1 400 Bad Request
    Content-Type: application/json
    {
      "error": "Invalid user ID."
    }

    If the record with id === userId doesn't exist:
    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {
      "error": "User not found."
    }
```
- Query Parameters

| Parameter    | Type   | Required | Description                                                            |
|--------------|--------|----------|------------------------------------------------------------------------|
| `role `      | string | Yes      | The user role (Admin, Customer).<br/> This queryParam <br/> to search for filtering   |
| `email `     | string | Yes      | The user email.<br/> This queryParam <br/>to search for filtering.     |
| `firstName ` | string | Yes      | The user firstName.<br/> This queryParam <br/>to search for. |
| `lastName `  | string | Yes      | The user lastName.<br/> This queryParam <br/>to search for. |

- Request: `GET api/users?role=Customer` - filter for customer or admin
  - Response:
```
  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    "users": [
      {
        "id": "12345",
        "firstName": "John",
        "lastName": "Smith",
        "email": "john.smith@example.com",
        "role": "Customer",
      },
      {
        "id": "67890",
        "firstName": "Jane",
        "lastName": "Doe",
        "email": "jane.doe@example.com",
        "role": "Customer",
      }
    ]
  }
```
- Query Parameters

| Parameter | Type   | Required | Description                                                 |
|-----------|--------|----------|-------------------------------------------------------------|
| `userId`  | string | Yes      | The user ID for change properties<br/>in necessary account. |

- Request: `POST api/users` - This endpoint allows you to **register a new user** in the system.

Example request body:
```
    {
        "username": "johndoe",
        "password": "securepassword123",
        "role": "customer",
        "email": "johndoe@example.com"
    }
```
  - Response:

```
  HTTP/1.1 201 Created
  Content-Type: application/json

  {
    "message": "User account with ID 12345 has been created successfully"
  }

  HTTP/1.1 400 Bad Request
  Content-Type: application/json

  {
      "error": "Invalid input data. Please provide valid username, password, role, and email."
  }
```

- Request: `POST api/users` - This endpoint allows you to **authenticate** in the system.
    Example request body:
```    
    {
        "username": "johndoe",
        "password": "securepassword123",
    }
```

  - Response:

```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
       "user_id": "12345",
       "username": "johndoe",
       "role": "customer",
       "email": "johndoe@example.com",
       "token": "jwt.token.here"
    }

    If the request body does not contain required fields:
    HTTP/1.1 401 Unauthorized
    Content-Type: application/json
    {
      "error": "Authentication failed. Please check your username and password."

    }
```

- Request: `POST api/users` - This endpoint allows you to **logout and terminate your current session** in the system.

    Example request body:
```    
    {
      Authorization: Bearer your-jwt-token-here
    }
```

  - Response:

```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {

        "message": "Logout successful"

    }
```

- Request: `PUT api/users/:id` - This endpoint allows you to **update a user info** in the system.

Example request body:
```
    {
        "username": "johndoe",
        "email": "johndoe@example.com"
    }
```
  - Response:

```
  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    "message": "User information with ID 12345 has been updated successfully"
  }

```


- Query Parameters

| Parameter | Type   | Required | Description                     |
|-----------|--------|----------|---------------------------------|
| `userId ` | string | Yes      | The user id for delete account. |

- Request: `DELETE api/users/:userId` - DELETE user, only admin can do this
  - Response:

```
    HTTP/1.1 204 No Content
    Content-Type: application/json
    {

        "message": "User deleted successfully"

    }

    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {

        "error": "User not found"

    }

```

## Endpoint `api/parts`: <a name="parts"></a>
- Query Parameters

| Parameter | Type   | Required | Description                    |
|-----------|--------|----------|--------------------------------|
| `name `   | string | Yes      | The service name to searching. |

- Request: `GET api/parts` - get all spare parts
  - Response:

```
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "parts": [
        {
            "id": 1,
            "part_name": "Engine oil",
            "price": "10.99",
            "availability": 55
        },
        {
            "id": 2,
            "part_name": "Engine oil filter",
            "price": "15.00",
            "availability": 12
        },
        {
            "id": 3,
            "part_name": "Engine air filter",
            "price": "8.00",
            "availability": 23
        }
    ]
    }

    Example response:
    HTTP/1.1 204 No Content
    Content-Type: application/json
```

- Request: `GET api/parts/:id` - get necessary parts using params for filter
  - Response:

```
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "parts": [
        {
            "id": 1,
            "part_name": "Engine oil",
            "price": "10.99",
            "availability": 55
        },
        {
            "id": 2,
            "part_name": "Engine oil filter",
            "price": "15.00",
            "availability": 12
        },
        {
            "id": 3,
            "part_name": "Engine air filter",
            "price": "8.00",
            "availability": 23
        }
    ]
    }

    Example response:
    HTTP/1.1 204 No Content
    Content-Type: application/json
```

- Request: `POST api/parts` - create new part, only admin can do this. The request
  body should contain the required information for creating a new product, such as
  spare part name, price and availability.
  - Response:

```
  HTTP/1.1 201 Created
  Content-Type: application/json
  {
    "id": 4,
    "part_name": "Brake disc",
    "price": "23",
    "availability": 87
  }

  HTTP/1.1 400 Bad Request
  Content-Type: application/json

  {
    "error": "Missing required field: spare part name"
  }
```
- Query Parameters

| Parameter    | Type   | Required | Description               |
|--------------|--------|----------|---------------------------|
| `id `        | string | Yes      | The service id to delete. |
- Request: `DELETE api/parts/:id` - delete part by id
  - Response:

```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
      "message": "Spare part with ID 4 has been deleted"
    }

    HTTP/1.1 404 Not Found
    Content-Type: application/json

    {
      "error": "Spare part with ID 4 not found"
    }
```
- Query Parameters

| Parameter    | Type   | Required | Description     |
|--------------|--------|----------|-----------------|
| `id `        | string | Yes      | The service id. |
- Request: `PUT api/parts/:id` - update spare part, only admin can do this
  - Response:

```
  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    "id": 45,
    "part_name": "Tyre",
    "price": "100",
    "availability": 24
  }

  HTTP/1.1 404 Not Found
  Content-Type: application/json

  {
    "error": "Spare part with ID 45 not found"
  }
```

## Endpoint `api/repair-requests`: <a name="repair-requests"></a>
- Query Parameters

| Parameter         | Type    | Required | Description                                           |
|-------------------|---------|----------|-------------------------------------------------------|
| `user_id `        | string  | Yes      | The user id.                                          |
| `vehicle_info `   | string  | Yes      | The vehicle info (model).                             |
| `vehicle_year `   | string  | Yes      | The vehicle info (year).                              |
| `request_date `   | string  | Yes      | This date can help calculate estimated time of repair.|
| `parts`           | array   | Yes      | An array containing spare parts thant need to be replaced.|
| `status `         | boolean | Yes      | The status of the repair request. (true -> in progress, false -> finished)    |


- Request: `POST api/repair-requests'` - Creates a new request for vehicle repair.
```
{
  "user_id": "123",
  "vehicle_info": "Toyota Camry",
  "vehicle_year": "2020",
  "request_date": "2023-10-15",
  "status": true,
  "parts": [
    {
      "part_id": "1",
      "quantity": 2
    },
    {
      "part_id": "3",
      "quantity": 1
    }
  ]
}
```

  - Response:
```
    HTTP/1.1 201 Created
    Content-Type: application/json
    {
        "request_id": "456",
        "status": "true"
    }

    HTTP/1.1 400 Bad Request
    Content-Type: application/json
    {
      "error": "Required field 'user_id' is missing."
    }
    
    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {
      "error": "Spare part 'part_id': 3 not found."
    }


```

- Request: `GET api/repair-requests` - Returns a list of all repair requests.
  - Response:
```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
      "requests": [
            {
                "request_id": "456",
                "user_id": "123",
                "vehicle_info": "Toyota Camry",
                "request_date": "2023-10-15",
                "status": "true"
            },
             {
                "request_id": "457",
                "user_id": "124",
                "vehicle_info": "Ford Focus",
                "request_date": "2023-10-16",
                "status": "false"
            }
        ]
    }

```

- Request: `GET api/repair-requests/:id` - Returns detailed information about a repair request by ID. 
  - Response:
```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
        "request_id": "456",
        "user_id": "123",
        "vehicle_info": "Toyota Camry",
        "request_date": "2023-10-15",
        "status": "в процессе",
        "parts": [
                    {
                        "part_id": "1",
                        "quantity": 2
                    },
                    {
                        "part_id": "3",
                        "quantity": 1
                    }
                ]
    }

    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {
      "error": "Repair request not found."
    }

```


- Query Parameters

| Parameter        | Type   | Required | Description            |
|------------------|--------|----------|------------------------|
| `request_id `    | string | Yes      | The repair request id. |
- Request: `DELETE api/repair-request/:request_id` - delete selected appointmen
  - Response:
```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
      "message": "Repair request with ID 22 deleted successfully."
    }

    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {
      "error": "Repair request with ID 22 not found."
    }
```

## Install <a name="Install"></a>

Clone this repo with command

```
git clone git@github.com:geniusx1990/AutomotiveServiceAssistant.git
```

Go to project folder

```
cd AutomotiveServiceAssistant
```

Install dependencies

```
npm install
```

## Run in docker container <a name="docker"></a>

For running application in Docker container you should have docker installed on your machine

Run app

```
docker compose up
```

Stop App

```
docker compose down
```

## For Run App

Run command

```
ts-node index.ts
```

[⬆ Go Up ⬆](#go-up)