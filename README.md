# ImageLens

**ImageLens** is an AI-powered image search application that allows users to find images using **text queries** or **visual similarity**.

It combines **CLIP embeddings**, **Qdrant vector search**, and **MongoDB metadata** to provide semantic image search with a modern React interface.

## Features

- 🔍 **Text-to-Image Search** — Search images using natural-language queries.
- 🖼️ **Image-to-Image Search** — Upload an image and find visually similar images.
- 🤖 **CLIP Embeddings** — Converts text and images into semantic vector representations.
- ⚡ **Qdrant Vector Search** — Performs fast similarity search using cosine distance.
- 🗄️ **MongoDB** — Stores image metadata and search information.
- ☁️ **AWS S3 Support** — Store uploaded images locally or in Amazon S3.
- 📤 **Image Upload & Indexing** — Upload images and automatically add them to the search index.
- 🎯 **Search Filters** — Filter results by category, source, similarity, and sorting options.
- 📊 **Analytics Dashboard** — View image statistics, categories, search activity, and latency.
- 🌙 **Responsive UI** — Modern React interface with responsive layouts and dark mode.

## Tech Stack

### Frontend

- React
- Vite
- Tailwind CSS
- React Router
- Recharts
- Lucide React

### Backend

- Python
- FastAPI
- Uvicorn
- Pydantic
- Motor / PyMongo
- Pillow

### AI & Vector Search

- CLIP
- Qdrant Cloud
- Cosine Similarity

### Storage

- MongoDB
- AWS S3
- Local File Storage

## Architecture

```text
                    ┌─────────────────────┐
                    │     React / Vite    │
                    │      Frontend       │
                    └──────────┬──────────┘
                               │ REST API
                               ▼
                    ┌─────────────────────┐
                    │       FastAPI       │
                    │       Backend       │
                    └──────┬──────┬───────┘
                           │      │
              ┌────────────┘      └─────────────┐
              ▼                                 ▼
       ┌─────────────┐                  ┌─────────────┐
       │   MongoDB   │                  │    Qdrant   │
       │   Metadata  │                  │   Vectors   │
       └─────────────┘                  └──────┬──────┘
                                               │
                                               ▼
                                      ┌─────────────────┐
                                      │ CLIP Embeddings │
                                      └─────────────────┘

                         ┌─────────────────┐
                         │     AWS S3      │
                         │  Image Storage  │
                         └─────────────────┘
```

## How It Works

### Text Search

```text
User Query
    ↓
CLIP Text Embedding
    ↓
Qdrant Similarity Search
    ↓
Image IDs
    ↓
MongoDB Metadata
    ↓
Search Results
```

### Image Search

```text
Uploaded Image
    ↓
CLIP Image Embedding
    ↓
Qdrant Similarity Search
    ↓
Image IDs
    ↓
MongoDB Metadata
    ↓
Similar Images
```

## Project Structure

```text
ImageLens/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   └── v1/
│   │   ├── config/
│   │   ├── core/
│   │   ├── db/
│   │   ├── dependencies/
│   │   ├── models/
│   │   ├── repositories/
│   │   ├── services/
│   │   └── main.py
│   ├── scripts/
│   ├── tests/
│   ├── requirements.txt
│   └── .env.example
│
├── client/
│   ├── src/
│   │   ├── api/
│   │   ├── components/
│   │   ├── hooks/
│   │   ├── pages/
│   │   ├── utils/
│   │   ├── App.jsx
│   │   └── main.jsx
│   ├── package.json
│   └── .env.example
│
└── README.md
```

## API

The backend API is organized under:

```text
/api/v1
```

Main endpoints include:

| Endpoint | Method | Purpose |
|---|---|---|
| `/health/` | GET | Check API status |
| `/images/upload` | POST | Upload and index an image |
| `/search/image` | POST | Search using an image |
| `/search/text` | POST | Search using text |
| `/analytics/summary` | GET | Get application analytics |
| `/db/test` | GET | Test MongoDB connection |
| `/s3/test` | GET | Test S3 connection |

Interactive API documentation is available through FastAPI at:

```text
/docs
```

## Environment Variables

### Backend

Create `backend/.env` from `.env.example`.

```env
MONGODB_URL=<mongodb-connection-string>
MONGODB_DB_NAME=imagelens

QDRANT_URL=<qdrant-url>
QDRANT_API_KEY=<qdrant-api-key>
QDRANT_COLLECTION=images

CLIP_SERVICE_URL=<clip-service-url>

STORAGE_TYPE=local
UPLOAD_DIR=uploads

AWS_ACCESS_KEY_ID=<aws-access-key>
AWS_SECRET_ACCESS_KEY=<aws-secret-key>
AWS_REGION=<aws-region>
AWS_S3_BUCKET_NAME=<bucket-name>
```

Use:

```env
STORAGE_TYPE=local
```

for local image storage, or:

```env
STORAGE_TYPE=s3
```

for AWS S3.

### Frontend

Create `client/.env`:

```env
VITE_API_BASE_URL=http://localhost:8000/api/v1
VITE_BACKEND_ORIGIN=http://localhost:8000
```

## Installation

### 1. Clone

```bash
git clone https://github.com/usman0794/ImageLens.git
cd ImageLens
```

### 2. Backend

```bash
cd backend

python -m venv venv
```

Activate the environment.

**Windows**

```bash
venv\Scripts\activate
```

**Linux / macOS**

```bash
source venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Configure `backend/.env`, then start the API:

```bash
uvicorn app.main:app --reload
```

Backend:

```text
http://localhost:8000
```

API documentation:

```text
http://localhost:8000/docs
```

### 3. Frontend

Open another terminal:

```bash
cd client
npm install
npm run dev
```

Frontend:

```text
http://localhost:5173
```

## Reindexing

Rebuild the Qdrant vector index from existing MongoDB image records:

```bash
cd backend
python -m scripts.reindex_qdrant
```

## License

This project is licensed under the MIT License.
