# Full Stack Developer Assessment - Country Data Explorer (3-Day Challenge)

## Project Scope
Create a web app to explore country/state/city data using React frontend and Node.js/MongoDB backend.

## Technical Stack
- Frontend: React
- Backend: Node.js, Express
- Database: MongoDB
- Extra: Bootstrap for styling

## Backend Requirements

### MongoDB Schema
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
