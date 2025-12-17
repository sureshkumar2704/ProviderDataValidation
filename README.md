# Healthcare Provider AI System

A comprehensive Agentic AI system for validating, enriching, and managing healthcare provider data using CrewAI, LangChain, Google Gemini, Streamlit, FastAPI, and SQLite/PostgreSQL.

## 🚀 Features

- **Data Validation**: Validate NPI, medical licenses, and addresses using external APIs
- **Data Enrichment**: Enhance provider information with AI-powered analysis
- **CrewAI Integration**: Multi-agent coordination for complex tasks
- **Web Interface**: Streamlit-based UI with interactive visualizations
- **REST API**: FastAPI backend for programmatic access
- **Database Storage**: SQLite (default) or PostgreSQL for reliable data persistence
- **Interactive Analytics**: Comprehensive dashboards with 20+ visualizations
- **Batch Processing**: Process multiple providers simultaneously

## 📋 Prerequisites

- Python 3.8 or higher
- pip (Python package manager)
- (Optional) PostgreSQL if you want to use PostgreSQL instead of SQLite

## 🔧 Installation

### Step 1: Clone or Navigate to the Project

```bash
cd "/Users/sureshkumar/Downloads/Techathon 6.0/ai_system"
```

### Step 2: Install Dependencies

Install all required Python packages:

   ```bash
   pip install -r requirements.txt
   ```

Or if you're using Python 3 specifically:

```bash
pip3 install -r requirements.txt
```

### Step 3: Set Up Environment Variables

Create a `.env` file in the project root (optional, but recommended for API keys):

```bash
# Database Configuration
# Using SQLite for easier setup (default)
DATABASE_URL=sqlite:///./healthcare.db

# API Keys (add your keys here if you have them)
GEMINI_API_KEY=your_gemini_api_key_here
OPENAI_API_KEY=your_openai_api_key_here
NPI_API_KEY=your_npi_api_key_here
GOOGLE_MAPS_API_KEY=your_google_maps_api_key_here

# Application Settings
DEBUG=False
```

**Note**: The system will work without API keys, but some features (like AI enrichment) may be limited.

### Step 4: Initialize the Database

Initialize the database tables:

```bash
python3 -c "from database.connection import engine; from database.models import Base; Base.metadata.create_all(bind=engine); print('Database initialized successfully')"
```

Or using Python:

```bash
python -c "from database.connection import engine; from database.models import Base; Base.metadata.create_all(bind=engine); print('Database initialized successfully')"
```

## 🏃 Running the Application

### Method 1: Run Everything Together (Recommended)

Run the main script which starts both the API server and Streamlit UI:

   ```bash
python3 main.py
   ```

Or:

```bash
python main.py
```

This will:
- Initialize the database (if not already done)
- Start the FastAPI server on port **8000**
- Start the Streamlit UI on port **8501**
- Automatically handle both services

### Method 2: Run Services Separately

#### Start the API Server

In one terminal:

```bash
python3 -m uvicorn api.main:app --host 0.0.0.0 --port 8000 --reload
```

#### Start the Streamlit UI

In another terminal:

```bash
streamlit run ui/app.py --server.port 8501 --server.address 0.0.0.0
```

## 🌐 Accessing the Application

Once the application is running:

- **Streamlit UI**: Open your browser and navigate to:
  ```
  http://localhost:8501
  ```
  or
  ```
  http://0.0.0.0:8501
  ```

- **FastAPI API**: Access the API at:
  ```
  http://localhost:8000
  ```

- **API Documentation**: Interactive API docs available at:
  ```
  http://localhost:8000/docs
  ```

- **API Health Check**:
  ```
  http://localhost:8000/health
  ```

## 📊 Using the Application

### Streamlit UI Features

1. **Dashboard**: Overview of provider statistics and metrics
2. **Data Management**: Upload CSV files and process providers
3. **Provider Search**: Search and filter providers
4. **Analytics**: Interactive visualizations including:
   - Geographic distribution (maps, bar charts, pie charts)
   - Validation status comparisons
   - Data completeness analysis
   - AI enrichment status
   - And more!
5. **AI Processing**: Manual validation and enrichment of providers

### API Endpoints

#### Provider Management
- `GET /providers` - Get all providers (with pagination)
- `GET /provider/{provider_id}` - Get specific provider details
- `GET /providers/search` - Search providers with filters
- `GET /providers/stats` - Get provider statistics
- `GET /providers/analytics` - Get analytics data

#### Processing
- `POST /process-provider` - Process a single provider (validate + enrich)
- `POST /batch-validate` - Batch process multiple providers
- `POST /validate-provider/{provider_id}` - Validate a specific provider
- `POST /enrich-provider/{provider_id}` - Enrich a specific provider

#### Other
- `GET /health` - Health check endpoint
- `GET /validation-logs` - Get validation logs
- `POST /qa/report` - Generate QA report
- `GET /directory/export` - Export provider directory

## 🗂️ Project Structure

```
ai_system/
├── agents/              # AI agents for validation and enrichment
│   ├── crew_manager.py      # Main crew orchestration
│   ├── validation_agent.py  # NPI and license validation
│   ├── enrichment_agent.py  # Data enrichment
│   ├── qa_agent.py          # Quality assurance
│   └── directory_agent.py    # Directory export
├── api/                 # FastAPI backend
│   └── main.py             # API endpoints
├── config/              # Configuration settings
│   └── settings.py         # Environment and app settings
├── database/           # Database models and connection
│   ├── connection.py       # Database connection setup
│   └── models.py           # SQLAlchemy models
├── ui/                  # Streamlit user interface
│   └── app.py              # Main UI application
├── utils/               # Utility functions
│   ├── confidence.py       # Confidence scoring
│   ├── data_loader.py      # CSV data loading
│   ├── data_validation.py  # Validation utilities
│   ├── google_maps.py      # Google Maps integration
│   └── scraper.py          # Web scraping utilities
├── data/                # Data files (CSV, etc.)
├── main.py              # Main entry point (runs both API and UI)
├── requirements.txt     # Python dependencies
├── healthcare.db        # SQLite database (created automatically)
└── README.md            # This file
```

## 🔑 Environment Variables

| Variable | Description | Required | Default |
|----------|-------------|----------|---------|
| `DATABASE_URL` | Database connection string | No | `sqlite:///./healthcare.db` |
| `GEMINI_API_KEY` | Google Gemini API key for AI features | No | - |
| `OPENAI_API_KEY` | OpenAI API key (optional) | No | - |
| `NPI_API_KEY` | NPI Registry API key | No | - |
| `GOOGLE_MAPS_API_KEY` | Google Maps API key for address validation | No | - |
| `DEBUG` | Enable debug mode | No | `False` |

## 🗄️ Database

### SQLite (Default)

The application uses SQLite by default, which requires no additional setup. The database file (`healthcare.db`) will be created automatically in the project root.

### PostgreSQL (Optional)

To use PostgreSQL instead, update your `.env` file:

```env
DATABASE_URL=postgresql://username:password@localhost:5432/healthcare_db
```

Then ensure PostgreSQL is running and the database exists.

## 🧪 Testing the Installation

1. **Check if services are running**:
   ```bash
   curl http://localhost:8000/health
   ```
   Should return: `{"status":"healthy"}`

2. **Check database**:
   ```bash
   python3 -c "from database.connection import SessionLocal; from database.models import Provider; db = SessionLocal(); print(f'Providers: {db.query(Provider).count()}'); db.close()"
   ```

3. **Access the UI**: Open `http://localhost:8501` in your browser

## 🐛 Troubleshooting

### Port Already in Use

If port 8000 or 8501 is already in use:

1. **Find the process**:
   ```bash
   lsof -ti:8000  # For API
   lsof -ti:8501  # For Streamlit
   ```

2. **Kill the process**:
   ```bash
   kill -9 <PID>
   ```

3. **Or change ports** in `main.py`:
   - API: Change `--port 8000` to another port
   - Streamlit: Change `--server.port 8501` to another port

### Import Errors

If you encounter `ModuleNotFoundError`:

1. Ensure you're in the project root directory
2. Verify all dependencies are installed: `pip install -r requirements.txt`
3. Check Python path: The script should automatically set PYTHONPATH

### Database Errors

If you see database connection errors:

1. **SQLite**: Ensure you have write permissions in the project directory
2. **PostgreSQL**: Verify the connection string and that PostgreSQL is running
3. **Reinitialize**: Run the database initialization command again

### API Not Responding

1. Check if the API server is running: `curl http://localhost:8000/health`
2. Check the logs in the terminal
3. Verify no firewall is blocking the ports

## 📝 Example Usage

### Upload and Process Providers via UI

1. Open the Streamlit UI at `http://localhost:8501`
2. Navigate to "Data Management" tab
3. Upload a CSV file with provider data
4. Click "Process & Upload Data"
5. View results in "Analytics" tab

### Process Provider via API

```bash
curl -X POST "http://localhost:8000/process-provider" \
  -H "Content-Type: application/json" \
  -d '{
    "provider_id": "P0001",
    "first_name": "John",
    "last_name": "Doe",
    "specialty": "Cardiology",
    "state": "CA",
    "city": "Los Angeles"
  }'
```

### Get All Providers

```bash
curl "http://localhost:8000/providers?limit=10"
```

## 🛠️ Technologies Used

- **CrewAI**: Multi-agent coordination framework
- **LangChain**: Language processing and AI integration
- **Google Gemini**: AI model for enrichment and validation
- **Streamlit**: Web UI framework
- **FastAPI**: Modern REST API framework
- **SQLAlchemy**: Database ORM
- **SQLite/PostgreSQL**: Database storage
- **Plotly**: Interactive visualizations
- **Pandas**: Data manipulation
- **Pydantic**: Data validation

## 📄 License

This project is part of Techathon 6.0.

## 🤝 Contributing

This is a project for Techathon 6.0. For questions or issues, please refer to the project documentation.

## 📞 Support

For issues or questions:
1. Check the troubleshooting section above
2. Review the API documentation at `http://localhost:8000/docs`
3. Check application logs in the terminal

---

**Happy Coding! 🚀**
