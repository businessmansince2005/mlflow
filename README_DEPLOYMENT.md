# Deployment Guide

## Docker Deployment

### Build the Docker image locally:
```bash
docker build -t telco-churn-api .
```

### Run the Docker container:
```bash
docker run -p 8000:8000 telco-churn-api
```

### Test locally:
```bash
curl http://localhost:8000/health
```

## Render.com Deployment

### Prerequisites:
1. Create an account on [Render.com](https://render.com)
2. Connect your GitHub repository
3. Ensure your code is pushed to the `master` branch

### Deployment Steps:

1. **Push your code to GitHub** (master branch)

2. **Create a new Web Service on Render:**
   - Go to Render Dashboard
   - Click "New +" → "Web Service"
   - Connect your GitHub repository
   - Render will automatically detect `render.yaml`

3. **Configuration:**
   - Service name: `telco-churn-api` (already set in render.yaml)
   - Auto-deploy: Enabled (deploys on master branch pushes)
   - Port: 8000 (already configured)

4. **Environment Variables** (already set in render.yaml):
   - `PYTHONUNBUFFERED=1`
   - `MLFLOW_TRACKING_URI=file:./mlruns`

5. **Deploy:**
   - Render will automatically build and deploy using the Dockerfile
   - The service will be available at: `https://telco-churn-api.onrender.com`

### Manual Deployment (if needed):

If you need to deploy manually without render.yaml:

1. Go to Render Dashboard
2. New → Web Service
3. Connect repository
4. Settings:
   - **Name**: telco-churn-api
   - **Environment**: Docker
   - **Dockerfile Path**: ./Dockerfile
   - **Docker Context**: .
   - **Start Command**: (leave empty, uses Dockerfile CMD)
   - **Health Check Path**: /health
   - **Auto-Deploy**: Yes
   - **Branch**: master

### Important Notes:

- **Model Files**: The Dockerfile copies `mlruns/` directory which contains your trained models. Make sure to commit your model files to Git or use a persistent storage solution for production.

- **Port**: Render automatically sets the `PORT` environment variable. The Dockerfile uses `${PORT:-8000}` to fallback to 8000 if PORT is not set.

- **Health Check**: The `/health` endpoint is used by Render to verify the service is running.

- **Build Time**: First deployment may take 5-10 minutes as it installs all dependencies including XGBoost and MLflow.

### Troubleshooting:

1. **Build fails**: Check that all files in `requirements.txt` are available
2. **Model not found**: Ensure `mlruns/` directory is committed to Git
3. **Port issues**: Verify PORT environment variable is set correctly
4. **Health check fails**: Check logs in Render dashboard

### Testing the Deployed API:

Once deployed, test with:
```bash
curl https://telco-churn-api.onrender.com/health
```

Or use the interactive docs:
```
https://telco-churn-api.onrender.com/docs
```

