# AWS Database Communications

## Local AWS Database Setup

### Prerequisites
- Java Runtime Environment (for DynamoDB Local)
- AWS CLI installed and configured
- DynamoDB Local downloaded

### Setup Steps

1. **Configure Database Connection**
   - Add or uncomment `endpoint_url = "http://localhost:<port>"` in `/db_server/db_resource.py`

2. **Download DynamoDB Local**
   - Download the zip file `DynamoDBLocal.jar` from [AWS Documentation](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)
   - Extract to a local directory (e.g., `./dynamodb_local_latest`)

3. **Install AWS CLI**
   - Follow the [AWS CLI Installation Guide](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

4. **Configure AWS CLI**
   ```cmd
   aws configure
   ```
   - Match credentials in `server/db_server/settings.py`
   - Set default output format to `json`

5. **Start Local Database**
   ```cmd
   cd ./dynamodb_local_latest
   java -D"java.library.path=./DynamoDBLocal_lib" -jar DynamoDBLocal.jar
   ```
   
   **Custom Port Configuration:**
   ```cmd
   java -D"java.library.path=./DynamoDBLocal_lib" -jar DynamoDBLocal.jar -port <port-number>
   ```
   - Default port is 8000
   - Update `/db_server/db_resource.py` to match custom ports

### Useful CLI Commands

**List Tables:**
```cmd
aws dynamodb list-tables --endpoint-url http://localhost:8000
```

**Scan Table:**
```cmd
aws dynamodb scan --table-name <TableName> --endpoint-url http://localhost:8000
```

**Delete Item:**
```cmd
aws dynamodb delete-item --table-name Users --key '{"PrimaryKey": {"S":"PrimaryKeyValue"}}' --endpoint-url http://localhost:8000
```

**Add Bot Entry:**
```cmd
aws dynamodb put-item --table-name FollowBots --item '{
  "bot_id": {"S": "123"},
  "functional_key": {"S": "xyz"},
  "logs": {"L": []},
  "business_id": {"S": "001"},
  "assigned_user_id": {"NULL": true}
}' --endpoint-url http://localhost:8000
```

## Arduino Client Database Interactions (To be changed)

### Initial Bot Registration

On first boot-up, the Arduino client sends a persistent functional key to register with the server:

1. **Create Bot Entry**
   - Call `/api/createBotEntry` with functional key
   - Server returns JSON with `botId` field
   - Arduino stores this `botId` for future requests

2. **State Management**
   - Arduino tracks `IN_DATABASE` status
   - Updates status after successful registration
   - Prevents duplicate database entries

### Ongoing Data Transmission

All subsequent requests must include the `botId`:

```json
{
  "botId": "DATABASE_ID_FROM_REGISTRATION",
  "temperature": 55,
  "lastKnownLocation": {
    "latitude": 40.7128,
    "longitude": -74.0060
  },
  "battery": 78,
  "time": "14:32:15",
  "date": "2025-03-10"
}
```

## Website Client Database Interactions

### Bot-Specific Pages

For pages that interact with specific FollowBots:

1. **URL Structure**
   ```javascript
   <Route path="/bot-analytics/:botId" element={<Layout><BotAnalyticsPage /></Layout>} />
   ```

2. **Extract Bot ID**
   ```javascript
   const { botId } = useParams();
   ```

3. **API Calls with Bot ID**
   ```javascript
   // Get bot logs
   fetch(`/api/getBotLogs?bot_id=${botId}`)
   
   // Post target coordinates
   fetch(`/api/postTargetCoords?bot_id=${botId}`)
   ```

### User Session Management

For user-related operations in `server.py`:

```python
# Get current user ID from session
user_id = flask_login.current_user.id
```

## Database Schema

### Users Table
- Primary Key: `user_id`
- Attributes: username, email, password_hash, linked_bots

### FollowBots Table
- Primary Key: `bot_id`
- Attributes: functional_key, logs, business_id, assigned_user_id, location, battery, temperature

## API Endpoints Reference

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/createBotEntry` | POST | Register new FollowBot |
| `/api/getBotLogs` | GET | Retrieve bot activity logs |
| `/api/postBotLogs` | POST | Submit bot sensor data |
| `/api/linkBot` | POST | Link bot to user account |
| `/api/getUserBots` | GET | Get user's linked bots |
| `/api/postTargetCoords` | POST | Send target coordinates
