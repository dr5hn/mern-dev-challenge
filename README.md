# Full Stack Developer Assessment - Country Data Explorer (3-Day Challenge)

## Assessment Overview
This 3-day challenge tasks you with creating a web application for exploring geographical data - countries, states, and cities. You'll build both frontend and backend components, demonstrating your full-stack development capabilities.

Technical Requirements
Backend Development


## Technical Stack
You'll build a Node.js/Express backend connected to MongoDB. The database will store hierarchical data of countries, their states, and cities. Your API should support efficiently searching, filtering, and retrieving this geographical data.
- Frontend: React
- Backend: Node.js, Express
- Database: MongoDB
- Extra: Bootstrap for styling

## Backend Requirements

### MongoDB Schema
The MongoDB schema keeps the structure simple yet effective:
- Countries store basic information plus their states
- States maintain a list of cities as strings

```javascript
const CountrySchema = {
  name: { type: String, required: true },
  code: { type: String, required: true },
  region: String,
  states: [StateSchema]
}

const StateSchema = {
  name: { type: String, required: true },
  cities: [String]
}
```

### API Endpoints
```
GET /api/countries 
GET /api/countries/:code
GET /api/countries/search?q=term
GET /api/states/:countryCode
GET /api/cities/:stateCode
```

## Frontend Requirements
Using React, you'll create an intuitive interface for exploring geographical data. The application should feature two main views:
- A countries list page with search and filtering capabilities
- A detailed view of each country showing its states and cities

The interface should be responsive and user-friendly, utilizing Bootstrap for consistent styling and layout.
### Pages
1. **Countries List**
   - Search countries
   - Filter by region
   - Display as grid/list

2. **Country Details**
   - Show states list
   - Display cities
   - Back navigation

### UI Features
- Mobile responsive
- Loading states
- Error handling
- Basic styling

## Project Structure
```
/client
  /src
    /components
    /pages
    /services
/server
  /models
  /routes
  /controllers
```

## Timeline

### Day 1: Backend
- Setup Express & MongoDB
- Create schemas
- Implement API endpoints

### Day 2: Frontend Core
- Create React components
- Implement routing
- API integration

### Day 3: Polish & Submit
- Add search/filter
- Style components
- Documentation
- GitHub submission

## Submission
- GitHub repository
- README with setup steps
- Working demo links
- Screenshots optional

## Evaluation
- Working features (40%)
- Code quality (30%)
- UI/UX (20%)
- Git usage (10%)

Need any clarification on the requirements?
