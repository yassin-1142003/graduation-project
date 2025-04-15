# Real Estate API Documentation

## Overview

This API provides endpoints for a real estate listing platform. It allows users to register, login, manage property listings, favorites, comments, and bookings.

## Base URL

```
http://localhost:8000/api
```

## Authentication

The API uses Sanctum for token-based authentication. Most endpoints require a valid Bearer token.

### Register

Register a new user and get an authentication token.

**URL:** `/register`
**Method:** `POST`
**Authentication:** None

**Request Body:**
```json
{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "password_confirmation": "password123",
    "phone": "1234567890"
}
```

**Response:** 
```json
{
    "message": "User registered successfully",
    "access_token": "1|xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "token_type": "bearer",
    "user": {
        "id": 1,
        "name": "John Doe",
        "email": "john@example.com",
        "phone": "1234567890",
        "role_id": 2,
        "is_active": true,
        "created_at": "2023-04-15T10:00:00.000000Z",
        "updated_at": "2023-04-15T10:00:00.000000Z"
    }
}
```

### Login

Login and get an authentication token.

**URL:** `/login`
**Method:** `POST`
**Authentication:** None

**Request Body:**
```json
{
    "email": "john@example.com",
    "password": "password123"
}
```

**Response:**
```json
{
    "access_token": "1|xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
    "token_type": "bearer",
    "user": {
        "id": 1,
        "name": "John Doe",
        "email": "john@example.com",
        "role_id": 2,
        "phone": "1234567890",
        "profile_image": null,
        "created_at": "2023-04-15T10:00:00.000000Z",
        "updated_at": "2023-04-15T10:00:00.000000Z"
    }
}
```

### Logout

Logout and invalidate the current token.

**URL:** `/logout`
**Method:** `POST`
**Authentication:** Bearer Token

**Response:**
```json
{
    "message": "Successfully logged out"
}
```

### Get User Profile

Get the authenticated user's profile.

**URL:** `/user`
**Method:** `GET`
**Authentication:** Bearer Token

**Response:**
```json
{
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "role_id": 2,
    "phone": "1234567890",
    "profile_image": null,
    "created_at": "2023-04-15T10:00:00.000000Z",
    "updated_at": "2023-04-15T10:00:00.000000Z"
}
```

## Listings

### Get All Listings

Get a list of all property listings.

**URL:** `/listings`
**Method:** `GET`
**Authentication:** None

**Response:**
```json
{
    "data": [
        {
            "id": 1,
            "title": "Modern Apartment",
            "description": "Beautiful modern apartment in downtown",
            "price": 250000,
            "location": "New York",
            "user_id": 1,
            "created_at": "2023-04-15T10:00:00.000000Z",
            "updated_at": "2023-04-15T10:00:00.000000Z",
            "images": [
                {
                    "id": 1,
                    "url": "http://localhost:8000/storage/listings/image1.jpg"
                }
            ],
            "user": {
                "id": 1,
                "name": "John Doe"
            }
        }
    ],
    "links": {
        "first": "http://localhost:8000/api/listings?page=1",
        "last": "http://localhost:8000/api/listings?page=1",
        "prev": null,
        "next": null
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 1,
        "path": "http://localhost:8000/api/listings",
        "per_page": 15,
        "to": 1,
        "total": 1
    }
}
```

### Get Featured Listings

Get a list of featured property listings.

**URL:** `/listings/featured`
**Method:** `GET`
**Authentication:** None

### Search Listings

Search for listings with filters.

**URL:** `/listings/search`
**Method:** `GET`
**Authentication:** None

**Query Parameters:**
- `query` - Search term
- `min_price` - Minimum price
- `max_price` - Maximum price
- `location` - Location name
- `bedrooms` - Number of bedrooms
- `bathrooms` - Number of bathrooms

### Create Listing

Create a new property listing.

**URL:** `/listings`
**Method:** `POST`
**Authentication:** Bearer Token

**Request Body (form-data):**
- `title` - Listing title
- `description` - Listing description
- `price` - Property price
- `location` - Property location
- `images[]` - Property images (multiple files)

### Update Listing

Update an existing listing.

**URL:** `/listings/{listing_id}`
**Method:** `PUT`
**Authentication:** Bearer Token

**Request Body:**
```json
{
    "title": "Updated Apartment Title",
    "description": "Updated description",
    "price": 300000,
    "location": "Updated location"
}
```

### Delete Listing

Delete a listing.

**URL:** `/listings/{listing_id}`
**Method:** `DELETE`
**Authentication:** Bearer Token

## Favorites

### Get User Favorites

Get the authenticated user's favorite listings.

**URL:** `/favorites`
**Method:** `GET`
**Authentication:** Bearer Token

### Toggle Favorite

Add or remove a listing from favorites.

**URL:** `/favorites/{listing_id}/toggle`
**Method:** `POST`
**Authentication:** Bearer Token

## Comments

### Get Listing Comments

Get comments for a specific listing.

**URL:** `/listings/{listing_id}/comments`
**Method:** `GET`
**Authentication:** None

### Add Comment

Add a comment to a listing.

**URL:** `/comments`
**Method:** `POST`
**Authentication:** Bearer Token

**Request Body:**
```json
{
    "listing_id": 1,
    "content": "Great property, I'm interested!"
}
```

### Update Comment

Update an existing comment.

**URL:** `/comments/{comment_id}`
**Method:** `PUT`
**Authentication:** Bearer Token

**Request Body:**
```json
{
    "content": "Updated comment text"
}
```

### Delete Comment

Delete a comment.

**URL:** `/comments/{comment_id}`
**Method:** `DELETE`
**Authentication:** Bearer Token

## Admin Routes

These routes require admin role privileges.

### Get All Users

Get a list of all users.

**URL:** `/admin/users`
**Method:** `GET`
**Authentication:** Bearer Token (Admin)

### Get All Listings (Admin)

Get a list of all listings with admin details.

**URL:** `/admin/listings`
**Method:** `GET`
**Authentication:** Bearer Token (Admin)

### Approve Listing

Approve a listing.

**URL:** `/admin/listings/{listing_id}/approve`
**Method:** `PUT`
**Authentication:** Bearer Token (Admin)

### Reject Listing

Reject a listing.

**URL:** `/admin/listings/{listing_id}/reject`
**Method:** `PUT`
**Authentication:** Bearer Token (Admin)

## Error Responses

The API returns appropriate HTTP status codes:

- `200` - OK
- `201` - Created
- `400` - Bad Request
- `401` - Unauthorized
- `403` - Forbidden
- `404` - Not Found
- `422` - Validation Error
- `500` - Server Error

**Example Error Response:**
```json
{
    "message": "The given data was invalid.",
    "errors": {
        "email": [
            "The email field is required."
        ],
        "password": [
            "The password field is required."
        ]
    }
}
``` 