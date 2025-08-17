# Web-Based Mathematical Services via API: Quadratic Roots Example

## Overview

This project demonstrates how mathematical services can be offered through a web-based API. The primary goal is to show the mathematics community a practical pathway to make algorithms accessible online via standard web protocols. As a Proof of Concept (PoC), we implement a service that computes the real roots of a quadratic equation.

The service is built using **FastAPI**, a modern Python framework for creating APIs quickly, and demonstrates both **POST** and **GET** request handling.

---

## Purpose

1. **Expose mathematical computations as a service**: Users can access the computation of quadratic roots from anywhere, without installing software.
2. **Educate mathematicians about APIs**: Show how simple mathematical functions can be made accessible to other applications, fostering integration and reuse.
3. **Demonstrate modern Python web development**: Use of `FastAPI` and `pydantic` for type-safe request handling.

---

## API Design

The API provides two endpoints for computing the roots of a quadratic equation $$ax^2 + bx + c = 0$$ [Technical Documentation of Quadratic API](https://documenter.getpostman.com/view/46925086/2sB3BHjoZo#5f7a0d47-5b1b-46e0-b44c-bd7d85d6a355)]


### 1. POST `/roots`

- Accepts a JSON payload representing the coefficients `a`, `b`, and `c`.
- Returns real roots if they exist, or an error message if there are no real roots.

#### Example Request

```json
POST /roots
Content-Type: application/json

{
  "a": 1,
  "b": -3,
  "c": 2
}
```

#### Example Response

```json
{
  "root1": 2.0,
  "root2": 1.0
}
```

---

### 2. GET `/roots`

- Accepts query parameters `a`, `b`, and `c`.
- Returns real roots or an error message if the discriminant is negative or `a` is zero.

#### Example Request

```
GET /roots?a=1&b=-3&c=2
```

#### Example Response

```json
{
  "root1": 2.0,
  "root2": 1.0,
  "Service": "CASPAM Compute Services"
}
```

---

## Implementation Details

- **Python Framework**: `FastAPI` for high-performance web APIs.
- **Data Validation**: `pydantic.BaseModel` ensures that incoming POST requests contain valid numeric coefficients.
- **Mathematical Logic**:
  - Compute the **discriminant** $D = b^2 - 4ac$ 
  - If $D < 0$, return an error: "No real roots"
  - Else, compute roots using the quadratic formula:$$x = \frac{-b \pm \sqrt{D}}{2a}$$
- **Error Handling**:
  - Coefficient `a` cannot be zero.
  - No real roots if discriminant is negative.

---

## How to Use

1. Install dependencies:

````bash
pip install fastapi uvicorn pydantic
