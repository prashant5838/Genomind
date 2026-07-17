*-# Genomind: Intelligent Health Forecasting Through Genes and Legacy

Genomind is a full-stack, AI-powered web application designed to forecast individual health risks by analyzing hereditary and genetic data. The system uses machine learning models to provide personalized predictions for genetic disorders and hereditary traits.

## Features

- **Genetic Disorder Prediction**: Predicts risk for Cancer, Cystic Fibrosis, and Leigh Syndrome using SNP markers
- **Hereditary Trait Prediction**: Predicts risk for Obesity, Diabetes, Cardiovascular Disease, and Hair Loss
- **AI Assistant**: Chatbot powered by Google Gemini API for genetic health information
- **User Authentication**: JWT-based authentication system
- **Data Storage**: MongoDB for persistent data storage

## Tech Stack

### Frontend
- React 18 with TypeScript
- Vite
- Tailwind CSS
- shadcn-ui components
- Axios for API calls

### Backend
- Flask (Python)
- MongoDB
- scikit-learn & XGBoost for ML models
- Google Gemini API for AI assistant
- JWT for authentication

## Prerequisites

- Node.js 18+ and npm
- Python 3.8+
- MongoDB (local or cloud instance)
- Google Gemini API key

## Setup Instructions

### 1. Clone the Repository

```bash
git clone <YOUR_GIT_URL>
cd abstract-artisan-engine
```

### 2. Frontend Setup

```bash
# Install dependencies
npm install

# Create environment file (optional, defaults to http://localhost:5000)
# Create .env file in root directory:
# VITE_API_URL=http://localhost:5000
```

### 3. Backend Setup

```bash
# Navigate to backend directory
cd backend

# Create virtual environment (recommended)
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install Python dependencies
pip install -r requirements.txt

# Create .env file from .env.example
# Copy backend/.env.example to backend/.env and fill in your values:
cp .env.example .env
```

Edit `backend/.env` with your configuration:

```env
MONGODB_URI=mongodb://localhost:27017
MONGODB_DB=genomind
GEMINI_API_KEY=your-gemini-api-key-here
JWT_SECRET=your-secret-key-change-in-production
FLASK_ENV=development
FLASK_PORT=5000
```

### 4. MongoDB Setup

#### Option A: Local MongoDB

1. Install MongoDB from [mongodb.com](https://www.mongodb.com/try/download/community)
2. Start MongoDB service:
   ```bash
   # Windows
   net start MongoDB
   
   # macOS (with Homebrew)
   brew services start mongodb-community
   
   # Linux
   sudo systemctl start mongod
   ```

#### Option B: MongoDB Atlas (Cloud)

1. Create a free account at [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Create a cluster and get your connection string
3. Update `MONGODB_URI` in `backend/.env` with your Atlas connection string

### 5. Train ML Models

Before running the application, you need to train the machine learning models:

```bash
# Make sure you're in the backend directory
cd backend

# Train disorder prediction model
python training/train_disorder_model.py

# Train trait prediction models
python training/train_trait_models.py
```

The trained models will be saved in `backend/models/` directory:
- `genetic_disorder_model.joblib`
- `target_encoder.joblib`
- `obesity_model.joblib`
- `diabetes_model.joblib`
- `cvd_model.joblib`
- `hairloss_model.joblib`
- And their corresponding encoders

### 6. Get Google Gemini API Key

1. Visit [Google AI Studio](https://makersuite.google.com/app/apikey)
2. Create a new API key
3. Add it to `backend/.env` as `GEMINI_API_KEY`

### 7. Run the Application

#### Start Backend Server

```bash
# From backend directory
python app.py
```

The backend will run on `http://localhost:5000`

#### Start Frontend Development Server

```bash
# From root directory
npm run dev
```

The frontend will run on `http://localhost:3000` (or the port Vite assigns)

## Project Structure

```
abstract-artisan-engine/
├── backend/
│   ├── app.py                 # Main Flask application
│   ├── auth.py                # JWT authentication utilities
│   ├── database.py            # MongoDB connection utilities
│   ├── requirements.txt       # Python dependencies
│   ├── .env                   # Environment variables (create from .env.example)
│   ├── models/                # Trained ML models (.joblib files)
│   └── training/              # Training scripts
│       ├── train_disorder_model.py
│       └── train_trait_models.py
├── src/
│   ├── components/            # React components
│   ├── integrations/
│   │   └── api/
│   │       └── client.ts      # API client for Flask backend
│   └── pages/                 # Page components
├── genomind_disorder_specific.csv    # Disorder dataset
├── train_updated_for_genomind.csv    # Trait dataset
└── package.json               # Node.js dependencies
```

## API Endpoints

### Authentication
- `POST /auth/register` - Register new user
- `POST /auth/login` - Login user
- `GET /auth/verify` - Verify JWT token (requires auth)

### Predictions
- `POST /predict-disorder` - Predict genetic disorder (requires auth)
- `POST /predict-traits` - Predict hereditary traits (requires auth)

### Chat
- `POST /chat` - Chat with AI assistant (requires auth)

### Health Check
- `GET /health` - API health check

## Usage

1. **Register/Login**: Create an account or login to access the application
2. **Fill Assessment Form**: 
   - Enter personal information (Age, Gender)
   - Provide family medical history
   - Optionally enter genetic test results (SNP markers)
3. **Get Predictions**: Submit the form to receive personalized health risk predictions
4. **Chat with Assistant**: Use the chatbot to ask questions about genetics and health

## Development

### Training Models

The ML models use:
- **VotingClassifier** combining Random Forest and XGBoost
- **Pipeline** with ColumnTransformer for preprocessing
- Separate models for each trait/disorder

To retrain models with updated data:
1. Update the CSV files in the root directory
2. Run the training scripts again
3. The new models will replace the old ones in `backend/models/`

### Environment Variables

**Frontend (.env in root):**
- `VITE_API_URL` - Backend API URL (default: http://localhost:5000)

**Backend (backend/.env):**
- `MONGODB_URI` - MongoDB connection string
- `MONGODB_DB` - Database name
- `GEMINI_API_KEY` - Google Gemini API key
- `JWT_SECRET` - Secret key for JWT tokens
- `FLASK_ENV` - Flask environment (development/production)
- `FLASK_PORT` - Flask server port

## Troubleshooting

### Models Not Found Error
- Make sure you've run the training scripts
- Check that model files exist in `backend/models/`

### MongoDB Connection Error
- Verify MongoDB is running
- Check `MONGODB_URI` in `backend/.env`
- For Atlas, ensure your IP is whitelisted

### Gemini API Error
- Verify your API key is correct
- Check API quota/limits in Google AI Studio

### CORS Errors
- Ensure backend is running on the correct port
- Check `VITE_API_URL` matches backend URL

## License

This project is for educational purposes.

## Support

For issues or questions, please open an issue in the repository.
