# Experiment 3 — Application Integration Test

**Date:** 2026.6.22–2026.6.28

This experiment verified the complete application workflow:

1. User authentication in the Flutter application.
2. Lesion-image selection and clinical metadata entry.
3. Prediction request sent to the FastAPI backend.
4. Inference using the trained multi-modal PyTorch model.
5. Screening-risk result displayed in the application.
6. Prediction history saved to and retrieved from SQLite.

The application presents predictions as screening support rather than a final medical diagnosis.
