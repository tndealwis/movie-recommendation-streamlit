# Movie Recommendation System

This project implements a full-stack movie recommendation system. It features a responsive web interface built with Streamlit and a backend REST API powered by FastAPI. The core recommendation logic utilizes collaborative filtering based on cosine similarity to analyze user rating patterns and suggest relevant movies.

## Key Functionality

*   **Collaborative Filtering:** Generates movie suggestions by analyzing similarities in user ratings.
*   **Interactive Interface:** A user-friendly web application developed with Streamlit for browsing and interaction.
*   **RESTful Backend:** A dedicated FastAPI service that handles recommendation logic and data management.
*   **Engagement Metrics:** Tracks user interactions to calculate the click-through rate (CTR) for recommended titles.
*   **Data Management:** Allows users to input new movies and ratings directly through the application interface.
*   **Database Flexibility:** Supports SQLite for development ease and PostgreSQL for production environments.

## System Architecture

The application comprises three primary components:

1.  **Frontend (Streamlit):** The `streamlit-app.py` script drives the user interface. It allows users to select movies, view recommendations, and submit new data. Communication with the backend occurs via HTTP requests.

2.  **Backend (FastAPI):** Located in `myapp/main.py`, this component defines the API endpoints responsible for:
    *   Generating and serving movie recommendations.
    *   Logging user clicks for analytics.
    *   Retrieving engagement statistics.
    *   Persisting new movie data.

3.  **Core Logic & Database:**
    *   The recommendation engine resides in `myapp/recommender.py`, utilizing `pandas` and `scikit-learn` to construct similarity matrices.
    *   SQLAlchemy serves as the ORM for database interactions, managing movie details, ratings, and click statistics (defaulting to SQLite).

## Technical Stack

*   **Backend:** FastAPI, Uvicorn
*   **Frontend:** Streamlit
*   **Data Science:** Pandas, NumPy, Scikit-learn
*   **Persistence:** SQLAlchemy, SQLite, Psycopg2-binary

## Data Source

The system is designed to process the **MovieLens 100K Dataset**. This dataset must be obtained from the official GroupLens website.

## Installation Guide

Follow the steps below to set up the environment and run the application.

### 1. Clone the Repository

Retrieve the source code from the repository:

```bash
git clone https://github.com/your-username/movie-recommendation-system-streamlit.git
cd movie-recommendation-system-streamlit
```

### 2. Create and Activate a Virtual Environment

It's recommended to use a virtual environment to manage project dependencies.

```bash
# For macOS/Linux
python3 -m venv venv
source venv/bin/activate

# For Windows
python -m venv venv
venv\Scripts\activate
```

### 3. Install Dependencies

Install all the required Python packages using the `requirements.txt` file.

```bash
pip install -r requirements.txt
```

### 4. Download the Dataset

1.  Download the MovieLens 100K dataset: ml-100k.zip.
2.  Create a `data` directory in the project's root folder.
3.  Extract the `u.data` and `u.item` files from the downloaded zip and place them inside the `data` directory.

Your directory structure should look like this:

```
movie-recommendation-system-streamlit/
├── data/
│   ├── u.data
│   └── u.item
├── myapp/
└── ...
```

### 5. Initialize the Database

Run the data loading script. This will create a new SQLite database file (`movie_recommender.db`) in the root directory and populate it with the movie and rating data.

```bash
python sql_load.py
```

You should see "Movies inserted successfully." and "Ratings inserted successfully." printed in your console.

## How to Run the Application

You will need to run the backend and frontend in two separate terminal windows.

### 1. Start the FastAPI Backend

In your first terminal, run the Uvicorn server:

```bash
uvicorn myapp.main:app --reload
```

The API will be running at `http://127.0.0.1:8000`. You can view the interactive API documentation at `http://127.0.0.1:8000/docs`.

### 2. Start the Streamlit Frontend

In a second terminal, run the Streamlit app:

```bash
streamlit run streamlit-app.py
```

The web application will open in your browser, typically at `http://localhost:8501`.

## API Endpoints

The FastAPI backend provides the following endpoints:

- `GET /recommend/?movie={movie_title}`: Returns a list of recommended movies.
- `POST /click/?movie_id={movie_id}`: Records a "click" on a recommended movie to track engagement.
- `GET /click_stats/?movie_id={movie_id}`: Retrieves the click-through rate for a specific movie.
- `POST /add_movie/`: Adds a new movie and an initial rating to the database.

## Project Structure

```
.
├── data/                 # MovieLens dataset files
├── myapp/                # Main application source code
│   ├── database.py       # SQLite database configuration
│   ├── main.py           # FastAPI application and endpoints
│   ├── models.py         # SQLAlchemy ORM models
│   ├── recommender.py    # Core recommendation logic
│   └── schemas.py        # Pydantic schemas
├── postgres_version/     # Alternative PostgreSQL configuration
├── .gitignore
├── README.md             # This file
├── requirements.txt      # Project dependencies
├── sql_load.py           # Script to load data into SQLite
└── streamlit-app.py      # Streamlit frontend application
```
