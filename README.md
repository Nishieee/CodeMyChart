# Code My Chart: ICD-10 Billing code

A comprehensive MLOps pipeline for automated ICD-10 billing code prediction from clinical notes, demonstrating technical alignment with healthcare AI companies like CodaMetrix.

## 🌐 Live Application

**Hosted EC2 Instance**: [http://44.209.55.198:8501/](http://44.209.55.198:8501/)

**Elastic IP Address**: `44.209.55.198` (Permanent - Never Changes)

![Screenshot of the Live Application](./screenshots/app_screenshot.png)
![Screenshot of Prediction Results](./screenshots/predictions_screenshot.png)

## 🏗️ Architecture Overview

This project implements a production-ready MLOps pipeline using:
- **Databricks** for data validation and processing
- **AWS Glue** for ETL and batch inference
- **MLflow** for experiment tracking and model management
- **AWS Lambda** for real-time inference
- **Streamlit** for interactive frontend
- **Docker** for containerization
- **GitHub Actions** for CI/CD

## 🔐 HIPAA Compliance Features

- Encrypted data storage (S3 with SSE-KMS)
- IAM roles with minimum privileges
- Audit logging for data access
- Synthetic data generation (no real PHI)
- Secure model deployment

## 📁 Project Structure

```
CodeMyChart/
├── data/                   # Data storage (raw, cleaned, predictions)
├── databricks/            # Databricks notebooks and SQL scripts
├── glue_jobs/             # AWS Glue ETL jobs
├── model/                 # Model training and MLflow tracking
├── docker/                # Containerization
├── app/                   # Streamlit frontend
├── test/                  # Test suite
└── requirements.txt       # Python dependencies
```

## 🚀 Quick Start

1. **Setup Environment**
   ```bash
   pip install -r requirements.txt
   ```

2. **Generate Synthetic Data**
   ```bash
   python data_gen.py
   ```

3. **Preprocess Data (PySpark)**
   ```bash
   spark-submit glue_jobs/preprocessing_local.py
   ```

4. **Split Data (PySpark)**
   ```bash
   spark-submit model/split_data.py
   ```

5. **Train Model**
   ```bash
   python model/train_model.py
   ```

6. **Run with Docker Compose (Recommended)**
   ```bash
   docker-compose up
   ```
   - Streamlit UI: http://localhost:8502
   - MLflow UI: http://localhost:5001

**Note:** For PySpark scripts, always use `spark-submit` instead of `python`.

## 📊 Model Performance

**Model Architecture**: ClinicalBERT + XGBoost Ensemble
- **BERT Model**: emilyalsentzer/Bio_ClinicalBERT
- **Classifier**: XGBoost (100 estimators, early stopping)
- **Feature Engineering**: BERT embeddings (768) + engineered features (7)

### Performance Metrics

| Metric | Value |
|--------|-------|
| **Overall Accuracy** | 97.95% |
| **Hamming Loss** | 0.0205 |
| **F1 Score (Micro)** | 21.20% |
| **Precision (Micro)** | 46.12% |
| **Recall (Micro)** | 13.77% |

**Training Details**: 50,000 synthetic EHR records, 99 unique ICD-10 codes, 70%/15%/15% train/validation/test split

## 🔧 Configuration

### Environment Variables
```bash
export AWS_REGION=us-east-1
export S3_BUCKET=your-hipaa-bucket
export MLFLOW_TRACKING_URI=http://your-ec2:5000
export LAMBDA_FUNCTION_NAME=icd10-predictor
```

## 💰 Cost Estimation

- **EC2 (t3.medium)**: ~$30/month
- **Lambda**: ~$5/month
- **Glue**: ~$10/month
- **S3**: ~$5/month
- **Total**: ~$50/month

## 🐳 Docker Setup

### Quick Start
```bash
# Build the image
./docker/build.sh

# Run the container
docker-compose up
```

**Access**: Streamlit UI at http://localhost:8502, MLflow UI at http://localhost:5001

### Features
- Production-optimized multi-stage build
- HIPAA-compliant security
- Inference-only container (pre-trained models)
- Health checks and monitoring

## 📦 DVC Integration

### Setup
```bash
./dvc_setup.sh
```

### Run Pipeline
```bash
# Execute entire pipeline
dvc repro

# Run specific stages
dvc repro generate_data
dvc repro train_model
```

### Pipeline Stages
1. **generate_data** - Generate synthetic EHR data
2. **preprocess_data** - Clean and preprocess data
3. **split_data** - Split data for training
4. **train_model** - Train ICD-10 prediction model
5. **evaluate_model** - Evaluate model performance
6. **package_model** - Package model for deployment

### Common Commands
```bash
dvc status          # Check pipeline status
dvc metrics show    # View metrics
dvc plots show      # View plots
dvc push            # Push to remote storage
```

## 🧪 Testing

### Run All Tests
```bash
python test/run_all_tests.py
```

### Run Individual Tests
```bash
python test/test_prediction.py
python test/test_integration.py
python test/test_streamlit_features.py
```

### Test Coverage
- Data pipeline validation
- Model loading and prediction
- Feature engineering
- Streamlit integration
- End-to-end pipeline testing

## 📈 Monitoring & Security

- MLflow experiment tracking
- CloudWatch metrics for Lambda
- S3 access logs
- All data encrypted at rest and in transit
- IAM roles with least privilege

## 📝 License

MIT License - For educational and demonstration purposes only.

## 🤝 Contributing

This is a demonstration project. For production use, ensure proper HIPAA compliance validation and security audits.
