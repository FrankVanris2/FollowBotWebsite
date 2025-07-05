# FollowBotWebsite
A web application for managing and monitoring FollowBot devices. This platform allows users to register, authenticate, link FollowBot devices, and monitor their activity through real-time data visualization and analytics.

## What is BotServer?

BotServer is a full-stack web application that serves as the central hub for FollowBot management. It provides:

- **User Management**: Registration, authentication, and session management
- **Bot Linking**: Connect FollowBot devices to user accounts using functional keys
- **Real-time Monitoring**: Track bot location, temperature, battery status, and activity logs
- **Data Analytics**: Visualize bot performance and historical data
- **API Integration**: RESTful endpoints for Arduino/IoT device communication (*To be changed*)

The application uses Flask as the backend server with DynamoDB for data persistence, and React for the frontend interface.

# Configuration & Usage

## Prerequisites

- [Node.js](https://nodejs.org/en/download) (for frontend development)
- Python 3.x (for Flask backend)
- AWS CLI configured (for DynamoDB access)

## Backend Setup

Navigate to the `BotServer` directory and install Python dependencies:

```cmd
cd BotServer
pip install Flask
pip install -U flask-cors
pip install boto3
pip install flask-login
pip install werkzeug
pip install botocore
```

## Frontend Setup

Install Node.js dependencies for React development:

```cmd
npm install
npm install react-leaflet@4 leaflet
npm install chart.js react-chartjs-2
npm install -g npm
```

## Database Configuration

Our application uses AWS DynamoDB. Ensure you have:

1. AWS credentials configured
2. DynamoDB tables created (Users and FollowBots)
3. Local DynamoDB for development (optional)

For detailed database setup and API documentation, see [DatabaseDocument](./Documentation/DatabaseDocument.md).

## Building and Running

1. **Build the application:**
   ```cmd
   npm run build
   ```

2. **Start the server:**
   ```cmd
   npm run start
   ```

3. **Development mode (auto-reload):**
   ```cmd
   npm run watch
   ```

4. **Access the application:**
   - Open your browser to `http://localhost:5000`
   - The exact URL will be displayed in the server command window

## Testing

### Browser Testing
Run end-to-end browser tests:
```cmd
npm run test:browser
```

### Unit Testing
Run Jest unit tests:
```cmd
npm test
```

## Development Guidelines

### MANDATORY: Unit Testing Before Implementation

**Every developer MUST implement comprehensive unit tests before adding any new feature to the codebase.**

#### Testing Requirements:

1. **Write Tests First**: Follow Test-Driven Development (TDD) principles
   - Write failing tests that describe the expected behavior
   - Implement the minimum code to make tests pass
   - Refactor while keeping tests green

2. **Test Coverage Requirements**:
   - All new functions/methods must have corresponding unit tests
   - Minimum 80% code coverage for new features
   - Test both success and failure scenarios
   - Mock external dependencies (database calls, API requests)

3. **Testing Standards**:
   - Use Jest for JavaScript/React components
   - Use Python's unittest for backend testing
   - Follow naming convention: `test_[function_name]_[scenario]`
   - Include setup and teardown for test isolation

4. **Before Pull Request**:
   ```cmd
   # Run all tests to ensure nothing is broken
   npm test
   npm run test:browser
   
   # Verify build still works
   npm run build
   ```

5. **Test Documentation**:
   - Document test scenarios in commit messages
   - Update test documentation when adding new test patterns
   - Reference [Jest Unit Testing Guide](./Documentation/JestUnitTesting.md)

#### Example Test Structure:
```javascript
// For React components
describe('UserLogin Component', () => {
  test('should render login form correctly', () => {
    // Test implementation
  });
  
  test('should handle invalid credentials gracefully', () => {
    // Test implementation
  });
});

// For API endpoints
describe('POST /api/login', () => {
  test('should authenticate valid user', () => {
    // Test implementation
  });
  
  test('should reject invalid credentials', () => {
    // Test implementation
  });
});
```

**No feature will be merged without corresponding unit tests. This ensures code reliability and maintainability.**

## API Endpoints

Key endpoints include:
- `POST /api/signup` - User registration
- `POST /api/login` - User authentication
- `POST /api/linkBot` - Link FollowBot to user account
- `GET /api/getUserBots` - Retrieve user's linked bots
- `GET /api/getBotLogs` - Get bot activity logs
- `POST /api/postBotLogs` - Receive data from FollowBot devices

## Documentation

- [Database Schema & API Reference](./Documentation/DatabaseDocument.md)
- [Jest Unit Testing Guide](./Documentation/JestUnitTesting.md)

## Project Structure

```
BotServer/
├── server/               # Flask backend
│   ├── db_server/       # Database models
│   ├── server.py        # Main Flask application
│   └── sessionUser.py   # User session management
├── src/                 # React frontend
│   ├── pages/          # React components
│   └── ...
├── dist/               # Built frontend assets
└── package.json        # Node.js
```
