# Code Challenge - Flask Restaurant and Pizza API

This project provides a simple API to manage restaurants, pizzas, and their associations using Flask, SQLAlchemy, and Flask-Migrate. The app allows users to perform CRUD operations on restaurants, pizzas, and the many-to-many relationship between them (restaurant_pizzas). The API supports features such as adding new pizzas to restaurants, deleting restaurants, and retrieving information about restaurants and pizzas.

## Table of Contents

- [Installation](#installation)
- [Setup](#setup)
- [Endpoints](#endpoints)
  - [GET /restaurants](#get-restaurants)
  - [GET /restaurants/id](#get-restaurant)
  - [DELETE /restaurants/id](#delete-restaurant)
  - [GET /pizzas](#get-pizzas)
  - [POST /restaurant_pizzas](#create-restaurant-pizza)
- [Testing](#testing)
- [Contributing](#contributing)
- [License](#license)
- [Author](#author)

## Installation

To run this project locally, follow these steps:

1. Clone the repository:
    ```bash
    git clone https://github.com/JasperMunene/python-phase-4-code-challenge-pizza.git
    cd python-phase-4-code-challenge-pizza
    ```

2. Download the dependencies for the frontend and backend, run::
   
      ```bash
      pipenv install
      pipenv shell
      npm install --prefix client
      ```
You can run your Flask API on [`localhost:5555`](http://localhost:5555) by running:
  ```bash
    python server/app.py
  ```
You can run your React app on [`localhost:4000`](http://localhost:4000) by
running:

```sh
npm start --prefix client
```


3. The file server/models.py defines the model classes without relationships. Use the following commands to create the initial database app.db:
    ```bash
    export FLASK_APP=server/app.py
    flask db init
    flask db migrate
    flask db upgrade head
    ```
4. Run the migrations and seed the database:
    ```bash
    flask db revision --autogenerate -m 'message'
    flask db upgrade head
    python server/seed.py
    ```
## Setup

The application runs on **Flask** and uses **SQLAlchemy** for database management, **Flask-Migrate** for database migrations, and **Flask-RESTful** for building the API endpoints.

1. **SQLAlchemy** ORM is used to manage the relationships between models.
2. The `Restaurant` and `Pizza` models are connected via a many-to-many relationship using the `RestaurantPizza` table.
3. **Cascading deletes** have been implemented, so when a restaurant or pizza is deleted, the associated records in the `restaurant_pizzas` table are also removed.

## Endpoints

### GET /restaurants

This endpoint returns a list of all restaurants in the database.

**Response**:
```json
[
  {
    "id": 1,
    "name": "Pizza Place",
    "address": "123 Pizza St"
  },
  ...
]
```

### GET /restaurants/<int:id>

This endpoint returns detailed information about a specific restaurant, including its associated pizzas.

**Response**:
```json
{
  "id": 1,
  "name": "Pizza Place",
  "address": "123 Pizza St",
  "restaurant_pizzas": [
    {
      "id": 1,
      "pizza": {
        "id": 1,
        "name": "Margherita",
        "ingredients": "Tomato, Mozzarella, Basil"
      },
      "pizza_id": 1,
      "price": 10,
      "restaurant_id": 1
    }
  ]
}
```

### DELETE /restaurants/<int:id>

This endpoint deletes a restaurant and all associated restaurant_pizzas.

**Response**:
- Status code `204 No Content` for a successful deletion.
- Status code `404 Not Found` if the restaurant does not exist.

### GET /pizzas

This endpoint returns a list of all pizzas in the database.

**Response**:
```json
[
  {
    "id": 1,
    "ingredients": "Dough, Tomato Sauce, Cheese",
    "name": "Emma"
  },
  {
    "id": 2,
    "ingredients": "Dough, Tomato Sauce, Cheese, Pepperoni",
    "name": "Geri"
  },
  {
    "id": 3,
    "ingredients": "Dough, Sauce, Ricotta, Red peppers, Mustard",
    "name": "Melanie"
  }
]
```

### POST /restaurant_pizzas

This endpoint creates a new restaurant-pizza association. You need to provide the `price`, `pizza_id`, and `restaurant_id` in the request body.

**Request body**:
```json
{
  "price": 15,
  "pizza_id": 1,
  "restaurant_id": 1
}
```

**Response**:
```json
{
  "id": 1,
  "pizza": {
    "id": 1,
    "name": "Margherita",
    "ingredients": "Tomato, Mozzarella, Basil"
  },
  "pizza_id": 1,
  "price": 15,
  "restaurant": {
    "address": "123 Pizza St",
    "id": 1,
    "name": "Pizza Place"
  },
  "restaurant_id": 1
}
```

## Testing

You can run tests using `pytest`:
```bash
  pytest
```

## Contributing

If you'd like to contribute to this project, feel free to fork the repository and create a pull request with your changes. Please ensure that all tests pass before submitting your pull request.

## License

This project is licensed under the MIT License.

---

## Author
 **Jasper Munene**
