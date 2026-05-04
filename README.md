# Meal Planner Microservices System

A lightweight microservices-based meal planning system built with FastAPI.

The system consists of three independent services that communicate using JSON over HTTP:

- User Preference Service (port 8001)
- Recipe Service (port 8010)
- Meal Planner Service (port 8011)

Each service runs independently and can be started, tested, and deployed on its own.

---

## Project Structure

meal-planner-system/
  user-preference-service/
    app.py
    requirements.txt
  recipe-service/
    app.py
    requirements.txt
  meal-planner-service/
    app.py
    requirements.txt

---

## Getting Started

### 1. Install Dependencies

Each service has its own requirements.txt.  
Install dependencies inside each folder:

```bash
pip install -r requirements.txt
```
## Running the Services

### User Preference Service (Port 8001)
cd user-preference-service
py -m uvicorn app:app --port 8001

### Recipe Service (Port 8010)
cd recipe-service
py -m uvicorn app:app --port 8010

### Meal Planner Service (Port 8011)
cd meal-planner-service
py -m uvicorn app:app --port 8011

Each service must run in its own terminal window.

---

## API Endpoints

### User Preference Service (8001)

POST /users?userId=<id>  
Stores user preferences.

Example body:
{
  "preferredCuisine": "Mexican",
  "minCalories": 800,
  "minProtein": 50
}

GET /users/<userId>  
Returns stored preferences.

---

### Recipe Service (8010)

GET /recipes

Example:
http://localhost:8010/recipes?cuisine=Mexican&minCalories=800&minProtein=50

Returns filtered recipes.

---

### Meal Planner Service (8011)

POST /plan  
Generates a meal plan using preferences and recipes.

Example body:
{
  "userId": "james"
}

---

## Testing the JSON Flow

A simple end-to-end test:

1. POST preferences to:
   http://localhost:8001/users?userId=james

2. GET recipes from:
   http://localhost:8010/recipes?cuisine=Mexican&minCalories=800&minProtein=50

3. POST a meal plan request to:
   http://localhost:8011/plan

With body:
{
  "userId": "james"
}

This demonstrates JSON flowing through all three services.

---

## Design Notes

### Microservice Architecture
- Each service has a single responsibility.
- Services communicate using HTTP and JSON.
- FastAPI provides automatic OpenAPI documentation at /docs.

### Stateless Services
- No database is used.
- Data is stored in memory and resets when services restart.
- This keeps the project simple and easy to run.

### FastAPI
- Automatic docs and schema generation.
- Easy JSON request/response handling.
- Lightweight and suitable for microservices.

---

## Known Issues

### Port Conflicts
Ports 8001, 8010, or 8011 may already be in use.  
If you see an error like:
[Errno 10048] Only one usage of each socket address is normally permitted

Fix by:
- Closing old terminals
- Killing stuck processes
- Or changing to a different port

### No Persistent Storage
- Preferences and recipes are stored in memory only.
- Data is lost when services restart.

### No Authentication
- Any client can access any user’s preferences.
- This is acceptable for a class project but not for production.

### Manual Startup
- Each service must be started manually in its own terminal.
- There is no Docker or orchestration yet.

---

## Future Improvements
- Add a real database for persistence.
- Add authentication and authorization.
- Add Docker Compose to start all services together.
- Integrate a real external recipe API.
- Build a simple frontend UI for users.
