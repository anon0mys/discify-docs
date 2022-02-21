---
title: API Reference

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Discify API
---

# Introduction

Welcome to the Discify API! You can use this API to access Discify API endpoints, which can get information on courses, and manage players and rounds.

## POST and PATCH Requests

> Example:

```json
{
  "resource": {
    "attribute": "Test"
  }
}
```

All POST/PATCH request payloads require a top level key which represents the resource that is being created or updated.

# Courses

## Get All Courses

This endpoint retrieves a list of disc golf courses.

### HTTP Request

`GET  https://discify-api.herokuapp.com/api/v1/courses`

### Query Parameters

> Example Response:

```json
[
  {
        "id": 1,
        "name": "Village Greens",
        "description": "This is a course with a description",
        "city": "Greenwood Village",
        "state": "CO",
        "location": null,
        "rating": 4.0,
        "holes": 9,
        "availability": "Year Round",
        "tees": "Concrete",
        "targets": "Mach V",
        "property": "Public Park",
        "services": "Dogs Allowed, Restrooms Available",
        "established": "2013"
    },
    {
        "id": 2,
        "name": "Peace with Christ Lutheran Church",
        "description": null,
        "city": "Aurora",
        "state": "CO",
        "location": null,
        "rating": 3.1,
        "holes": 9,
        "availability": "Year Round - It is at a church. Sundays are hit or miss.",
        "tees": null,
        "targets": null,
        "property": "Mixed Use, Church",
        "services": "Dogs Allowed",
        "established": "2000"
    }
]
```

Parameter | Type | Description
--------- | ---- | -----------
name | string | Fuzzy search for courses by name.
city | string | Fuzzy search for courses in a city. 
state | string | Search for courses in a state. 
holes_min | number | Search for courses that have at least this number of holes. 
rating_min | number | Search for courses that have at least this rating. 


## Get a Specific Course

This endpoint retrieves a specific course.

### HTTP Request

`GET  https://discify-api.herokuapp.com/api/v1/courses/:id`

### URL Parameters

> Example Response:

```json
{
    "id": 15,
    "name": "Paco Sanchez",
    "description": "Signs not labeled, some missing. Challenging course. Hole 1 under construction, tee off from sidewalk.",
    "city": "Denver",
    "state": "CO",
    "location": null,
    "rating": 3.5,
    "holes": 21,
    "availability": "Year Round",
    "tees": "Concrete",
    "targets": "Mach V",
    "property": "Mixed Use, Public Park",
    "services": "",
    "established": "2006",
    "layouts": [
        {
            "name": "Main Layout 21 Holes Current 10/15/2020",
            "description": "Note: Hole 1 sidewalk tee off. Additional Note: Hole 8, MANDO right of both transmission towers. Par is up to date.",
            "total_par": 70,
            "total_distance": 8450,
            "holes": [
                {
                    "hole_number": 1,
                    "par": 4,
                    "distance": 575
                },
                {
                    "hole_number": 2,
                    "par": 4,
                    "distance": 615
                }
                // ... holes
            ]
        },
        {
            "name": "Front 9 Loop",
            "description": "1-8, 21 coming back ",
            "total_par": 31,
            "total_distance": 4483,
            "holes": [
                // ... holes
            ]
        }
        // ... layouts
    ]
}
```

Parameter | Type | Description
--------- | ---- | -----------
ID | number | The ID of the course to retrieve

# Players

## Get All Players

This endpoint retrieves a list of players.

### HTTP Request
> Example Response:

```json
[
  {
    "id": 1,
    "name": "Name"
  },
  {
    "id": 2,
    "name": "Other Name"
  }
]
```

`GET  https://discify-api.herokuapp.com/api/v1/players`

## Get a Specific Player

This endpoint retrieves a specific player.

### HTTP Request

`GET  https://discify-api.herokuapp.com/api/v1/players/:id`

### URL Parameters

> Example Response:

```json
{
  "id": 1,
  "name": "Name"
}
```

Parameter | Type | Description
--------- | ---- | -----------
ID | number | The ID of the player to retrieve

## Create a Player

This endpoint creates a player.

### HTTP Request

`POST  https://discify-api.herokuapp.com/api/v1/players`

### Request Body

> Example Request Body:

```json
{
  "player": {
    "name": "Test"
  }
}
```

Parameter | Type | Description
--------- | ---- | -----------
name | string | The name of the player

> Example Response:

```json
{
  "id": 1,
  "name": "Name"
}
```

## Delete a Player

This endpoint deletes a player.

### HTTP Request

`DELETE  https://discify-api.herokuapp.com/api/v1/players/:id`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
ID | number | The ID of the player to delete

# Rounds

## Create a Round

This endpoint creates a round at a specific course and layout.

### HTTP Request

`POST  https://discify-api.herokuapp.com/api/v1/rounds`

### Request Body

> Example Request Body:

```json
{
  "round": {
    "course_id": 1,
    "layout_id": 2,
    "player_ids": [1, 2, 3]
  }
}
```

Parameter | Type | Description
--------- | ---- | -----------
course_id | number | The id of the Course where the round is being played
layout_id | number | The id of the Layout of the course that is being played
player_ids | array[numbers] | The ids of the Players playing

> Example Response:

```json
{
  "id": 1,
  "course_name": "Name",
  "layout_name": "Main",
  "date": "01/20/2022",
  "holes": [
    {
      "hole_number": 1,
      "par": 4,
      "distance": 575
    },
    {
      "hole_number": 2,
      "par": 4,
      "distance": 615
    }
  ]
}
```

## End a Round

This endpoint ends a round. There are two ways to track scores. The `player_scores` param for the body of this post request can be used to end the round and set final scores for the players. Alternately, there is a per-hole tracking endpoint to track as the game goes on. If per-hole tracking is used, the body is not required for this endpoint. Scores in the body will override calculated scores.

### HTTP Request

`POST  https://discify-api.herokuapp.com/api/v1/rounds/:id/end_round`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
ID | number | The ID of the round to end

### Request Body

> Example Request Body:

```json
{
  "round": {
    "player_scores": [
      {
        "player_id": 1,
        "total_score": 54
      },
      {
        "player_id": 2,
        "total_score": 53
      }
    ]
  }
}
```

Top Level Parameter | Nested Parameter | Type | Description
------------------- | ---------------- | ---- | -----------
player_scores [Optional] | | array[player_scores] | The list of final scores for each player
  | player_id | number | The id of the Player
  | total_score | number | A Player's total score for this round

> Example Response:

```json
{
  "id": 1,
  "course_name": "Name",
  "layout_name": "Main",
  "total_par": 54,
  "date": "01/20/2022",
  "scores": [
    {
      "player_id": 1,
      "total_score": 52,
      "score": -2
    },
    {
      "player_id": 2,
      "total_score": 56,
      "score": 2
    }
  ]
}
```

## Score a Hole

This endpoint tracks player scores for a single hole.

### HTTP Request

`POST  https://discify-api.herokuapp.com/api/v1/rounds/:id/score_hole`

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
ID | number | The ID of the round that the hole belongs to

### Request Body

> Example Request Body:

```json
{
  "hole": {
    "hole_number": 1,
    "player_scores": [
      {
        "player_id": 1,
        "strokes": 3
      },
      {
        "player_id": 2,
        "strokes": 4
      }
    ]
  }
}
```

Top Level Parameter | Nested Parameter | Type | Description
------------------- | ---------------- | ---- | -----------
hole_number | | number | The hole number to add scores to
player_scores | | array[player_scores] | A list of scores for each player on this hole
  | player_id | number | The id of the Player
  | strokes | number | A Player's count of strokes for this hole

> Example Response:

```json
{
  "hole_number": 1,
  "scores": [
    {
      "player_id": 1,
      "strokes": 2,
      "score": -1
    },
    {
      "player_id": 2,
      "strokes": 5,
      "score": 2
    }
  ]
}
```
