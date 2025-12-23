# Churn Prediction API - Usage Examples

## Base URL
```
http://127.0.0.1:8000
```

## Endpoints

### 1. Health Check
```bash
curl http://127.0.0.1:8000/health
```

### 2. Root Endpoint
```bash
curl http://127.0.0.1:8000/
```

### 3. Predict Churn (POST)

#### Complete curl Command (Linux/Mac/Git Bash):
```bash
curl -X POST "http://127.0.0.1:8000/predict" \
  -H "Content-Type: application/json" \
  -d '{
    "gender": "Female",
    "SeniorCitizen": 0,
    "Partner": "Yes",
    "Dependents": "No",
    "tenure": 5,
    "PhoneService": "Yes",
    "MultipleLines": "No",
    "InternetService": "DSL",
    "OnlineSecurity": "No",
    "OnlineBackup": "Yes",
    "DeviceProtection": "No",
    "TechSupport": "No",
    "StreamingTV": "No",
    "StreamingMovies": "No",
    "Contract": "Month-to-month",
    "PaperlessBilling": "Yes",
    "PaymentMethod": "Electronic check",
    "MonthlyCharges": 70.0,
    "TotalCharges": "350"
  }'
```

#### PowerShell Command:
```powershell
$body = @{
    gender = "Female"
    SeniorCitizen = 0
    Partner = "Yes"
    Dependents = "No"
    tenure = 5
    PhoneService = "Yes"
    MultipleLines = "No"
    InternetService = "DSL"
    OnlineSecurity = "No"
    OnlineBackup = "Yes"
    DeviceProtection = "No"
    TechSupport = "No"
    StreamingTV = "No"
    StreamingMovies = "No"
    Contract = "Month-to-month"
    PaperlessBilling = "Yes"
    PaymentMethod = "Electronic check"
    MonthlyCharges = 70.0
    TotalCharges = "350"
} | ConvertTo-Json

Invoke-RestMethod -Uri "http://127.0.0.1:8000/predict" -Method Post -Body $body -ContentType "application/json"
```

#### Python Example:
```python
import requests

url = "http://127.0.0.1:8000/predict"
data = {
    "gender": "Female",
    "SeniorCitizen": 0,
    "Partner": "Yes",
    "Dependents": "No",
    "tenure": 5,
    "PhoneService": "Yes",
    "MultipleLines": "No",
    "InternetService": "DSL",
    "OnlineSecurity": "No",
    "OnlineBackup": "Yes",
    "DeviceProtection": "No",
    "TechSupport": "No",
    "StreamingTV": "No",
    "StreamingMovies": "No",
    "Contract": "Month-to-month",
    "PaperlessBilling": "Yes",
    "PaymentMethod": "Electronic check",
    "MonthlyCharges": 70.0,
    "TotalCharges": "350"
}

response = requests.post(url, json=data)
print(response.json())
```

## Field Descriptions

| Field | Type | Required | Description | Valid Values |
|-------|------|----------|-------------|--------------|
| gender | string | Yes | Customer gender | "Male", "Female" |
| SeniorCitizen | integer | Yes | Is senior citizen | 0 (No), 1 (Yes) |
| Partner | string | Yes | Has partner | "Yes", "No" |
| Dependents | string | Yes | Has dependents | "Yes", "No" |
| tenure | integer | Yes | Months with company | 0-72 |
| PhoneService | string | Yes | Has phone service | "Yes", "No" |
| MultipleLines | string | Yes | Multiple phone lines | "Yes", "No", "No phone service" |
| InternetService | string | Yes | Internet service type | "DSL", "Fiber optic", "No" |
| OnlineSecurity | string | Yes | Online security | "Yes", "No", "No internet service" |
| OnlineBackup | string | Yes | Online backup | "Yes", "No", "No internet service" |
| DeviceProtection | string | Yes | Device protection | "Yes", "No", "No internet service" |
| TechSupport | string | Yes | Tech support | "Yes", "No", "No internet service" |
| StreamingTV | string | Yes | Streaming TV | "Yes", "No", "No internet service" |
| StreamingMovies | string | Yes | Streaming movies | "Yes", "No", "No internet service" |
| Contract | string | Yes | Contract type | "Month-to-month", "One year", "Two year" |
| PaperlessBilling | string | Yes | Paperless billing | "Yes", "No" |
| PaymentMethod | string | Yes | Payment method | "Electronic check", "Mailed check", "Bank transfer (automatic)", "Credit card (automatic)" |
| MonthlyCharges | float | Yes | Monthly charges | Any positive number |
| TotalCharges | string | Yes | Total charges | Any string/number (can be empty) |

## Response Format

```json
{
  "churn_probability": 0.4523,
  "churn_prediction": 0
}
```

- `churn_probability`: Float between 0 and 1 (probability of churning)
- `churn_prediction`: Integer (0 = No churn, 1 = Churn)

## Interactive API Documentation

Visit these URLs in your browser:
- **Swagger UI**: http://127.0.0.1:8000/docs
- **ReDoc**: http://127.0.0.1:8000/redoc

## Quick Test Examples

### High Churn Risk Customer:
```json
{
  "gender": "Male",
  "SeniorCitizen": 0,
  "Partner": "No",
  "Dependents": "No",
  "tenure": 1,
  "PhoneService": "Yes",
  "MultipleLines": "No",
  "InternetService": "Fiber optic",
  "OnlineSecurity": "No",
  "OnlineBackup": "No",
  "DeviceProtection": "No",
  "TechSupport": "No",
  "StreamingTV": "Yes",
  "StreamingMovies": "Yes",
  "Contract": "Month-to-month",
  "PaperlessBilling": "Yes",
  "PaymentMethod": "Electronic check",
  "MonthlyCharges": 99.0,
  "TotalCharges": "99"
}
```

### Low Churn Risk Customer:
```json
{
  "gender": "Female",
  "SeniorCitizen": 1,
  "Partner": "Yes",
  "Dependents": "No",
  "tenure": 60,
  "PhoneService": "Yes",
  "MultipleLines": "Yes",
  "InternetService": "DSL",
  "OnlineSecurity": "Yes",
  "OnlineBackup": "Yes",
  "DeviceProtection": "Yes",
  "TechSupport": "Yes",
  "StreamingTV": "No",
  "StreamingMovies": "No",
  "Contract": "Two year",
  "PaperlessBilling": "No",
  "PaymentMethod": "Bank transfer (automatic)",
  "MonthlyCharges": 50.0,
  "TotalCharges": "3000"
}
```

