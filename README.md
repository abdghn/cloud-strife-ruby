# Cloud Strife Ruby API

A modern REST API built with Ruby on Rails 8, featuring JWT authentication and product management capabilities. Named after the iconic Final Fantasy character, this API is designed to be powerful and reliable.

## 🚀 Tech Stack

- **Ruby**: 3.4.4
- **Rails**: 8.0.2
- **Database**: PostgreSQL
- **Authentication**: JWT (JSON Web Tokens)
- **Deployment**: Kamal
- **Containerization**: Docker
- **Version Control**: Git with rbenv

## 📋 Prerequisites

- Ruby 3.4.4 or higher
- PostgreSQL
- Bundler gem
- Docker (for deployment)
- Node.js (for asset compilation)

## 🛠️ Installation & Setup

### 1. Clone the Repository

```
git clone <repository-url>
cd cloud-strife-ruby
```


### 2. Install Ruby Dependencies

```
bundle install
```

### 3. Environment Configuration
Create and configure your environment file:

```
cp .env.example .env
```
Edit the `.env` file to set your database credentials and JWT secret key.

### 4. Database Setup

Ensure PostgreSQL is running and create the database:

```
# Create databases
rails db:create

# Run migrations
rails db:migrate

# Seed data (optional)
rails db:seed
```

### 5. Start the Rails Server

```
rails server
# or simply
rails s
```

The server will start on `http://localhost:3000`.

## 📚 API Documentation
### Health Check Endpoints


```
GET /up     - Rails built-in health check
GET /status - Custom health status endpoint
```


### Authentication
#### User Login
``` 
POST /login
```
**Request Body:**
``` json
{
  "email": "user@example.com",
  "password": "your_password"
}
```
**Success Response (200):**
``` json
{
  "token": "eyJhbGciOiJIUzI1NiJ9.eyJ1c2VyX2lkIjoxLCJleHAiOjE2..."
}
```
**Error Response (401):**
``` json
{
  "error": "Invalid credentials"
}
```
### Products (Protected Routes)
All product endpoints require authentication via Bearer token.
``` 
GET    /products       - List all products
POST   /products       - Create a new product
GET    /products/:id   - Get a specific product
PUT    /products/:id   - Update a product
PATCH  /products/:id   - Partially update a product
DELETE /products/:id   - Delete a product
```
**Authentication Header:**
``` 
Authorization: Bearer <your-jwt-token>
```
### Protected Data Routes
``` 
GET    /protected_data - Example protected endpoint
POST   /protected_data - Create protected data
PUT    /protected_data/:id - Update protected data
DELETE /protected_data/:id - Delete protected data
```
## 🧪 Testing the API
### Using Postman or Thunder Client
1. **Login to get JWT token:**
``` 
   Method: POST
   URL: http://localhost:3000/login
   Headers: Content-Type: application/json
   Body:
   {
     "email": "test@example.com",
     "password": "password123"
   }
```
1. **Access protected endpoints:**
``` 
   Method: GET
   URL: http://localhost:3000/products
   Headers: 
   - Authorization: Bearer <token-from-login-response>
   - Content-Type: application/json
```
### Using cURL
``` bash
# Login and get token
curl -X POST http://localhost:3000/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'

# Use token to access protected endpoint
curl -X GET http://localhost:3000/products \
  -H "Authorization: Bearer YOUR_JWT_TOKEN_HERE" \
  -H "Content-Type: application/json"

# Create a new product
curl -X POST http://localhost:3000/products \
  -H "Authorization: Bearer YOUR_JWT_TOKEN_HERE" \
  -H "Content-Type: application/json" \
  -d '{"product":{"name":"Test Product","price":29.99}}'
```
### Using HTTPie
``` bash
# Login
http POST localhost:3000/login email=test@example.com password=password123

# Get products
http GET localhost:3000/products Authorization:"Bearer YOUR_TOKEN"
```
## 🔧 Development Setup
### Adding Test Framework
For comprehensive testing, add these gems to your Gemfile:
``` ruby
group :development, :test do
  gem 'rspec-rails'
  gem 'factory_bot_rails'
  gem 'faker'
  gem 'shoulda-matchers'
end
```
Then run:
``` bash
bundle install
rails generate rspec:install
```
### Code Quality Tools
The project includes:
- **RuboCop**: For code style enforcement
- **Brakeman**: For security vulnerability scanning

Run code quality checks:
``` bash
bundle exec rubocop
bundle exec brakeman
```
## 🐳 Docker & Deployment
### Local Docker Setup
``` bash
# Build the Docker image
docker build -t cloud-strife-ruby .

# Run the container
docker run -p 3000:3000 cloud-strife-ruby
```
### Production Deployment with Kamal
``` bash
# Initial setup (first time only)
kamal setup

# Deploy application
kamal deploy

# Check deployment status
kamal app logs
```
## ⚙️ Configuration
### Environment Variables
Create a file with the following variables: `.env`
``` env
# Database
DATABASE_URL=postgresql://username:password@localhost/cloud_strife_ruby_development
POSTGRES_PASSWORD=your_postgres_password

# Rails
SECRET_KEY_BASE=your_secret_key_base_here
RAILS_ENV=development
RAILS_SERVE_STATIC_FILES=true

# JWT
JWT_SECRET_KEY=your_jwt_secret_key

# Other configurations
RAILS_LOG_LEVEL=debug
```
### Database Configuration
Update for your PostgreSQL setup: `config/database.yml`
``` yaml
development:
  adapter: postgresql
  encoding: unicode
  database: cloud_strife_ruby_development
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: <%= ENV.fetch("DATABASE_USERNAME", "postgres") %>
  password: <%= ENV.fetch("DATABASE_PASSWORD", "") %>
  host: <%= ENV.fetch("DATABASE_HOST", "localhost") %>
```
## 📁 Project Structure
``` 
cloud-strife-ruby/
├── app/
│   ├── controllers/
│   │   ├── api/v1/              # API versioning
│   │   │   └── auth_controller.rb
│   │   ├── concerns/            # Shared controller logic
│   │   ├── application_controller.rb
│   │   └── products_controller.rb
│   ├── models/
│   │   ├── concerns/            # Shared model logic
│   │   ├── user.rb
│   │   ├── product.rb
│   │   └── application_record.rb
│   ├── jobs/                    # Background jobs
│   ├── mailers/                 # Email templates
│   └── views/                   # View templates
├── config/
│   ├── environments/            # Environment configs
│   ├── initializers/           # App initializers
│   ├── routes.rb               # API routes
│   ├── database.yml            # Database config
│   └── application.rb          # Main app config
├── db/
│   ├── migrate/                # Database migrations
│   └── seeds.rb                # Seed data
├── lib/                        # Custom libraries
├── public/                     # Static assets
├── .kamal/                     # Kamal deployment configs
├── Dockerfile                  # Docker configuration
├── Gemfile                     # Ruby dependencies
├── docker-compose.yml          # Multi-container setup
└── README.md                   # This file
```
## 🔒 Security Features
- **JWT Authentication**: Secure token-based authentication
- **BCrypt Password Hashing**: Secure password storage
- **CORS Configuration**: Cross-origin request handling
- **Rate Limiting**: Protection against abuse (configurable)
- **Secure Headers**: Security-focused HTTP headers
- **Environment-based Secrets**: Secure configuration management

## 🚦 HTTP Status Codes

| Status Code | Description |
| --- | --- |
| 200 | OK - Request successful |
| 201 | Created - Resource created successfully |
| 204 | No Content - Successful deletion |
| 400 | Bad Request - Invalid request format |
| 401 | Unauthorized - Authentication required |
| 403 | Forbidden - Access denied |
| 404 | Not Found - Resource doesn't exist |
| 422 | Unprocessable Entity - Validation errors |
| 500 | Internal Server Error - Server error |
## 🔄 API Versioning
The API uses URL-based versioning:
- Current version: `/api/v1/`
- Future versions: `/api/v2/`, `/api/v3/`, etc.

## 📊 Monitoring & Logging
- **Health Checks**: Built-in health monitoring endpoints
- **Structured Logging**: JSON-formatted logs for production
- **Performance Monitoring**: Built-in Rails instrumentation
- **Error Tracking**: Configurable error reporting

## 🤝 Contributing
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Run the test suite (`bundle exec rspec`)
4. Run code quality checks (`bundle exec rubocop`)
5. Commit your changes (`git commit -m 'Add some amazing feature'`)
6. Push to the branch (`git push origin feature/amazing-feature`)
7. Create a Pull Request

### Development Guidelines
- Follow the existing code style (enforced by RuboCop)
- Write tests for new features
- Update documentation as needed
- Keep commits atomic and descriptive

## 📈 Performance Considerations
- **Database Indexing**: Proper indexes for query optimization
- **Caching**: Redis-based caching for improved performance
- **Background Jobs**: Solid Queue for async processing
- **Asset Optimization**: Compressed and minified assets

## 🔍 Troubleshooting
### Common Issues
1. **Database Connection Error**
``` bash
   # Check PostgreSQL is running
   brew services start postgresql
   # or
   sudo systemctl start postgresql
```
1. **Missing Secret Key**
``` bash
   # Generate new secret key
   rails secret
   # Add to your .env file
```
1. **Port Already in Use**
``` bash
   # Kill process on port 3000
   lsof -ti:3000 | xargs kill -9
```
## 📝 License
This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.
## 📞 Support
- Create an issue for bug reports
- Use discussions for questions
- Check the wiki for additional documentation

## 🙏 Acknowledgments
- Inspired by Final Fantasy VII's Cloud Strife
- Built with Ruby on Rails community best practices
- Follows RESTful API design principles

