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
   - [Endpoints: marks](#marks)
   - [Endpoints: models](#models)
   - [Endpoints: vehicles](#vehicles)
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

## Endpoint `api/v1/users`: <a name="users"></a>
<details>

### GET all users
##### `GET api/users` 
<details>

This endpoint allows you to get a list of users.

* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json 
  [
    {
      "id": "1",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "password": "$2a$05$bWPQ0AXCJPdm8H4f1S6XeOCUJs8zfjNcsGU7/QjYqqKhUpbijk22y",
      "phonenumber": "123123",
      "role": "user"
    },
    {
      "id": "2",
      "name": "Jane Smith",
      "email": "jane.smith@example.com",
      "password": "$2a$05$bWPQ0AXCJPdm8H4f1S6XeOCUJs8zfjNcsGU7/QjYqqKhUpbijk22y",
      "phonenumber": "232323",
      "role": "admin"
    },
    ...
    ]
```
</details>

### GET one user
##### `GET api/v1/users/:id`
<details>
This endpoint allows you to retrieve a single user by ID.

- Query Parameters

| Parameter | Type   | Required | Description  |
|-----------|--------|----------|--------------|
| `id=[integer]`      | number | Yes      | The user ID. |

* **Success Response:**

```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
      "id": "1",
      "name": "John Doe",
      "email": "john.doe@example.com",
      "password": "$2a$05$bWPQ0AXCJPdm8H4f1S6XeOCUJs8zfjNcsGU7/QjYqqKhUpbijk22y",
      "phonenumber": "123123",
      "role": "user"
    }
```

* **Error Response:**
```
    HTTP/1.1 400 Bad Request
    Content-Type: application/json
    {
      error: "Invalid User ID. User ID must be a number."
    }

    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {
      error: "User not found." 
    }

    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json
    {
      error: "An error occurred while fetching the user."
    }
```
</details>

### POST Create a new  user
##### `POST api/v1/users`
<details>
This endpoint allows you to register a new user in the system.

Example request body:
```
    {
        "name": "johndoe",
        "email": "john.doe@example.com",
        "password": "securepassword",
        "phonenumber": "123123",
        "role": "user"
    }
```
* **Success Response:**
```
  HTTP/1.1 201 Created
  Content-Type: application/json
   {
        "name": "johndoe",
        "email": "john.doe@example.com",
        "password": "securepassword",
        "phonenumber": "123123",
        "role": "user"
    }
```

* **Error Response:**

```
  HTTP/1.1 400 Bad Request
  Content-Type: application/json

    {
        error: "Invalid user data. Please check the request data and try again."
    }

  HTTP/1.1 500 Internal Server Error
  Content-Type: application/json
    {
        error: "An error occurred while fetching the user."
    }

```
</details>

### PUT Update user
##### `PUT api/v1/users`
<details>
This endpoint allows you to update a user info in the system.

Example request body:
```
  {
    "id": 1,
    "name": "andrea",
    "email": "andrea@gmail.com",
    "password": "$2a$05$bWPQ0AXCJPdm8H4f1S6XeOCUJs8zfjNcsGU7/QjYqqKhUpbijk22y",
    "phonenumber": "1234567",
    "role": "User"
  }
```
* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    "message": "User successfully updated",
    "updatedUser": {
        "id": 1,
        "name": "andrea",
        "email": "andrea@gmail.com",
        "password": "$2a$05$bWPQ0AXCJPdm8H4f1S6XeOCUJs8zfjNcsGU7/QjYqqKhUpbijk22y",
        "phonenumber": "1234567",
        "role": "User"
    }
  }
```
* **Error Response:**
```
  HTTP/1.1 404 Not Found
  Content-Type: application/json
  {
    error: "User not found." 
  }

  HTTP/1.1 500 Internal Server Error
  Content-Type: application/json
  {
    error: "An error occurred while updating the user"
  }
```
</details>

### DELETE Delete user
##### `DELETE api/v1/users/:userId`
<details>
This endpoint allows you to delete a single user by ID.

- Query Parameters

| Parameter | Type   | Required | Description                     |
|-----------|--------|----------|---------------------------------|
| `id=[integer]`| number| Yes   | The user id for delete account. |


* **Success Response:**
```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
       "message": "User successfully deleted",
       "deletedUser": {
        "id": 3,
        "name": "andrea",
        "email": "andrea@gmail.com",
        "password": "andrea",
        "phonenumber": "1234567",
        "role": "User"
    }
}
```
* **Error Response:**

```
    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {
        error: "User not found"
    }

    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json
    {
      error: "An error occurred while deleting the user"
    }
```
</details>

</details>

## Endpoint `api/v1/parts`: <a name="parts"></a>

<details>

### GET Get all spare parts
##### `GET api/v1/parts`
<details>
This request allows you to get all spare parts.

* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json
    [
        {
            "id": 1,
            "part_name": "Engine oil",
            "price": "10.99",
            "availability": 55,
            "repair_cost": 20,
            "repair_time": 2,
            "vehicle_id": 2
        },
        {
            "id": 2,
            "part_name": "Engine oil filter",
            "price": "15.00",
            "availability": 12,
            "repair_cost": 10,
            "repair_time": 1,
            "vehicle_id": 1
        },
        {
            "id": 3,
            "part_name": "Engine air filter",
            "price": "8.00",
            "availability": 23,
            "repair_cost": 12,
            "repair_time": 3,
            "vehicle_id": 1
        }
    ]
```
</details>

### GET Get part
##### `GET api/v1/parts/:id`
<details>
This endpoint allows you to retrieve one spare part by ID.

* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    
    "id": 1,
    "part_name": "Engine oil",
    "price": "10.99",
    "availability": 55,
    "repair_cost": 12,
    "repair_time": 3,
    "vehicle_id": 1
  }
```

* **Error Response:**

```
  HTTP/1.1 404 Not Found
  Content-Type: application/json
    {
    error: "Part not found." 
    }

  HTTP/1.1 500 Internal Server Error
  Content-Type: application/json
    {
      error: "An error occurred while fetching the part."
    }
```
</details>

### POST Create new part
##### `POST api/parts`
<details>
This request allows to reate a new part, only admin can do this. The request
  body should contain the required information for creating a new spare part, such as
  part name, price, availability, repair cost, repair time, vehicle_id.

```
Header:
  {
      Authorization: Bearer your-jwt-token-here
  }

  {
    "id": 2,
    "part_name": "Engine oil filter",
    "price": "15.00",
    "availability": 12,
    "repair_cost": 10,
    "repair_time": 1,
    "vehicle_id": 1
  }
```
* **Success Response:**

```
  HTTP/1.1 201 Created
  Content-Type: application/json
  {
      part: {
          {
            "id": 2,
            "part_name": "Engine oil filter",
            "price": "15.00",
            "availability": 12,
            "repair_cost": 10,
            "repair_time": 1,
            "vehicle_id": 1
          },
      message: 'Part created successfully',
      }
  }
```

* **Error Response:**
```
  HTTP/1.1 400 Bad Request
  Content-Type: application/json
  {
    error: 'Invalid input. Please provide valid data and a positive price.'
  }

   HTTP/1.1 500 Internal Server Error
   Content-Type: application/json
  {
    error: "An error occurred while creating the part."
  }
```
</details>

### PUT Update part
##### `PUT api/v1/parts`

<details>
This request allows to update a part. The request
  body should contain the required information for updating a new spare part, such as
  part name, price, availability, repair cost, repair time, vehicle_id, id.

* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json

  {
    part: {
          {
            "id": 2,
            "part_name": "Engine oil filter",
            "price": "15.00",
            "availability": 12,
            "repair_cost": 10,
            "repair_time": 1,
            "vehicle_id": 1
          },
      message: 'Part updated successfully',
      }
  }
```
* **Error Response:**

```
  HTTP/1.1 400 Bad Request
  Content-Type: application/json
  {
    message: 'ID not specified'
  }

  HTTP/1.1 404 Not Found
  Content-Type: application/json
  {
    error: 'Part not found'
  }

   HTTP/1.1 500 Internal Server Error
  Content-Type: application/json
  {
    error: "An error occurred while updating the part."
  }
```
</details>

### DELETE Delete part
##### `DELETE api/parts/:id`

<details>
This endpoint allows you to delete a par in the system.

- Query Parameters

| Parameter    | Type   | Required | Description               |
|--------------|--------|----------|---------------------------|
| `id=[integer]`| number| Yes      | The part id for delete spare part. |

* **Success Response:**

```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
      part: {
          {
            "id": 2,
            "part_name": "Engine oil filter",
            "price": "15.00",
            "availability": 12,
            "repair_cost": 10,
            "repair_time": 1,
            "vehicle_id": 1
          },
      message: 'Part deleted successfully',
      }
  }
```

```
    HTTP/1.1 404 Not Found
    Content-Type: application/json

    {
      error: "Part not found"
    }

    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json
    {
      error: "An error occurred while deleting the part."
    }
```
</details>
</details>

## Endpoint `api/v1/marks`: <a name="marks"></a>
<details>

### GET all marks
##### `GET api/v1/marks`
<details>

This endpoint allows you to get a list of marks of vehicle.


* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json
  [
    {
        "id": 1,
        "mark": "Mercedes-Benz"
    },
    {
        "id": 2,
        "mark": "BMW"
    },
    {
        "id": 3,
        "mark": "Toyota"
    }
]
```
</details>

### GET one mark
##### `GET api/v1/marks/:id`

<details>
This endpoint allows you to retrieve a single mark by ID.

* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "id": 1,
    "mark": "Mercedes-Benz"
  }
```
* **Error Response:**

```
    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {
      error: "Mark not found." 
    }

    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json
    {
      error: "An error occurred while fetching the mark."
    }
```
</details>

### POST Create new mark
##### `POST api/v1/marks`

<details>
This endpoint allows you to register a new mark in the system.
The request body should contain the required information 
  for creating a new mark, such as mark name.

* **Request:**
```
  {
    "mark": "KIA"
  }
```
* **Success Response:**

```
  HTTP/1.1 201 Created
  Content-Type: application/json
  {
    "mark": {
        "id": 6,
        "mark": "KIA"
    },
    "message": "Mark created successfully"
}
```
* **Error Response:**

```
  HTTP/1.1 400 Bad Request
  Content-Type: application/json
  {
    error: 'Invalid input: mark is required.'
  }

   HTTP/1.1 500 Internal Server Error
   Content-Type: application/json
  {
    error: "An error occurred while creating a mark."
  }
```
</details>


### PUT Update mark
##### `PUT api/v1/marks`

<details>
This endpoint allows you to update a mark info in the system.

Example request body:
```
  {
    "id": 5,
    "mark": "Porsche",
  }
```

* **Success Response:**
```
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "message": "Mark updated successfully",
    "updatedMark": {
        "id": 5,
        "mark": "Porsche"
    }
}
```

* **Error Response:**

```
  HTTP/1.1 400 Bad Request
  Content-Type: application/json
  {
    message: 'ID not specified'
  }

  HTTP/1.1 404 Not Found
  Content-Type: application/json
  {
    error: 'Mark not found'
  }

   HTTP/1.1 500 Internal Server Error
  Content-Type: application/json
  {
    error: "An error occurred while updating the mark."
  }
```
</details>

### DELETE Delete mark
##### `DELETE api/marks/:id`

<details>
This endpoint allows you to delete a single mark by ID.

- Query Parameters

| Parameter    | Type   | Required | Description               |
|--------------|--------|----------|---------------------------|
| `id=[integer]`| number| Yes      | The mark id for delete mark. |


* **Success Response:**

```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
        "message": "Mark deleted successfully",
        "deletedMark": {
            "id": 5,
            "mark": "Porsche"
        }
    }
```

* **Error Response:**

```
    HTTP/1.1 404 Not Found
    Content-Type: application/json

    {
      error: "Mark not found"
    }

    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json
    {
      error: "An error occurred while deleting the mark."
    }
```
</details>
</details>

## Endpoint `api/v1/models`: <a name="models"></a>
<details>

### GET all models
##### `GET api/v1/models`
<details>

This endpoint allows you to get a list of models.


* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json
  [
    {
        "id": 1,
        "model": "E-class"
    },
    {
        "id": 2,
        "model": "C-class"
    },
    ....
]
```
</details>

### GET one model
##### `GET api/v1/models/:id`

<details>
This endpoint allows you to retrieve a single model by ID.

- Query Parameters

| Parameter    | Type   | Required | Description               |
|--------------|--------|----------|---------------------------|
| `id=[integer]`| number| Yes      | The model id to get the model. |

* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "id": 1,
    "model": "E-class"
  }
```
* **Error Response:**

```
    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {
      error: "Model not found." 
    }

    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json
    {
      error: "An error occurred while fetching the model."
    }
```
</details>

### POST Create new model
##### `POST api/v1/models`

<details>
This endpoint allows you to register a new model in the system.
The request body should contain the required information 
for creating a new model, such as model name.

* **Request:**
```
  {
    "model": "M-class"
  }
```
* **Success Response:**

```
  HTTP/1.1 201 Created
  Content-Type: application/json
  {
    "message": "Model created successfully",
    "model": {
        "id": 7,
        "model": "M-class"
    }
  }
```
* **Error Response:**

```
  HTTP/1.1 400 Bad Request
  Content-Type: application/json
  {
    error: 'Invalid or missing model data.'
  }

   HTTP/1.1 500 Internal Server Error
   Content-Type: application/json
  {
    error: "An error occurred while creating a mark."
  }
```
</details>


### PUT Update model
##### `PUT api/v1/models`

<details>
This endpoint allows you to update a model info in the system.

Example request body:
```
  {
    "id": 5,
    "model": "E-class",
  }
```

* **Success Response:**
```
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "message": "Mark updated successfully",
    "updatedModel": {
        "id": 5,
        "model": "E-class"
    }
}
```

* **Error Response:**

```
  HTTP/1.1 400 Bad Request
  Content-Type: application/json
  {
    message: 'ID not specified'
  }

  HTTP/1.1 404 Not Found
  Content-Type: application/json
  {
    error: 'Model not found'
  }

   HTTP/1.1 500 Internal Server Error
  Content-Type: application/json
  {
    error: "An error occurred while updating the model."
  }
```
</details>

### DELETE Delete model
##### `DELETE api/models/:id`

<details>
This endpoint allows you to delete a single model by ID.

- Query Parameters

| Parameter    | Type   | Required | Description               |
|--------------|--------|----------|---------------------------|
| `id=[integer]`| number| Yes      | The model id for delete model. |


* **Success Response:**

```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
        "message": "Model deleted successfully",
        "deletedModel": {
            "id": 5,
            "model": "E-class"
        }
    }
```

* **Error Response:**

```
    HTTP/1.1 404 Not Found
    Content-Type: application/json

    {
      error: "Model not found"
    }

    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json
    {
      error: "An error occurred while deleting the model."
    }
```
</details>
</details>

## Endpoint `api/v1/vehicles`: <a name="vehicles"></a>
<details>

### GET all vehicles
##### `GET api/v1/vehicles`
<details>
 id | mark_id | model_id | vehicle_year 
This endpoint allows you to get a list of vehicles.


* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json
  [
    {
        "id": 1,
        "mark_id": 2,
        "model_id": 3,
        "vehicle_year": 2016
    },
    {
        "id": 2,
        "mark_id": 3,
        "model_id": 3,
        "vehicle_year": 2016
    },
    ...
  ]
```
</details>

### GET one vehicle
##### `GET api/v1/vehicles/:id`

<details>
This endpoint allows you to retrieve a single vehicle by ID.

- Query Parameters

| Parameter    | Type   | Required | Description               |
|--------------|--------|----------|---------------------------|
| `id=[integer]`| number| Yes      | The vehicle id to get the vehicle. |

* **Success Response:**

```
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "id": 1,
    "mark_id": 2,
    "model_id": 3,
    "vehicle_year": 2016
  }
```
* **Error Response:**

```
    HTTP/1.1 404 Not Found
    Content-Type: application/json
    {
      error: "Vehicle not found." 
    }

    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json
    {
      error: "An error occurred while fetching the vehicle."
    }
```
</details>

### POST Create new vehicle
##### `POST api/v1/vehicles`

<details>
This endpoint allows you to register a new vehicle in the system.
The request body should contain the required information 
for creating a new vehicle, such as model, mark, vehicle year.

* **Request:**
```
  {
    "mark_id": 2,
    "model_id": 3,
    "vehicle_year": 2016
  }
```
* **Success Response:**

```
  HTTP/1.1 201 Created
  Content-Type: application/json
  {
    "message": "Vehicle created successfully",
    "vehicle": {
        "id": 4,
        "mark_id": 2,
        "model_id": 3,
        "vehicle_year": 2016
    }
  }
```
* **Error Response:**

```
  HTTP/1.1 400 Bad Request
  Content-Type: application/json
  {
    error: 'Invalid or missing data in the request.'
  }

   HTTP/1.1 500 Internal Server Error
   Content-Type: application/json
  {
    error: "An error occurred while creating a vehicle."
  }
```
</details>


### PUT Update vehicle
##### `PUT api/v1/vehicles`

<details>
This endpoint allows you to update a vehicle info in the system.

Example request body:
```
  { 
    "id": 4,
    "mark_id": 2,
    "model_id": 3,
    "vehicle_year": 2011
  }
```

* **Success Response:**
```
  HTTP/1.1 200 OK
  Content-Type: application/json
  {
    "message": "Vehicle updated successfully",
    "vehicle": {
        "id": 4,
        "mark_id": 2,
        "model_id": 3,
        "vehicle_year": 2011
    }
  }
```

* **Error Response:**

```
  HTTP/1.1 400 Bad Request
  Content-Type: application/json
  {
    message: 'ID not specified'
  }

  HTTP/1.1 404 Not Found
  Content-Type: application/json
  {
    error: 'Vehicle not found'
  }

   HTTP/1.1 500 Internal Server Error
  Content-Type: application/json
  {
    error: "An error occurred while updating the vehicle."
  }
```
</details>

### DELETE Delete vehicle
##### `DELETE api/vehicles/:id`

<details>
This endpoint allows you to delete a single vehicle by ID.

- Query Parameters

| Parameter    | Type   | Required | Description               |
|--------------|--------|----------|---------------------------|
| `id=[integer]`| number| Yes      | The vehicle id for delete vehicle. |


* **Success Response:**

```
    HTTP/1.1 200 OK
    Content-Type: application/json
    {
    "message": "Vehicle deleted successfully",
    "vehicle": {
        "id": 6,
        "mark_id": 3,
        "model_id": 3,
        "vehicle_year": 2011
      }
    }
```

* **Error Response:**

```
    HTTP/1.1 404 Not Found
    Content-Type: application/json

    {
      error: "Vehicle not found"
    }

    HTTP/1.1 500 Internal Server Error
    Content-Type: application/json
    {
      error: "An error occurred while deleting the vehicle."
    }
```
</details>
</details>



</details>



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