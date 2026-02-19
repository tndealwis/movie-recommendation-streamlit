# Movie Recommendation System

This project is a complete movie recommendation system featuring a web interface built with Streamlit and a backend API powered by FastAPI. The recommendation engine uses a collaborative filtering approach based on cosine similarity to suggest movies based on user rating patterns.

## Features

- **Collaborative Filtering:** Recommends movies by finding similarities in how users have rated them.
- **Interactive UI:** A Streamlit-based web application for easy user interaction.
- **REST API:** A FastAPI backend that serves recommendations and handles data operations.
- **Performance Tracking:** Tracks which recommendations are shown and which are clicked to calculate a click-through rate (CTR).
- **Dynamic Data:** Ability to add new movies and ratings directly through the UI.
- **Dual Database Support:** Configured to work with both SQLite (for easy setup) and PostgreSQL (for a more production-like environment).

## Architecture

The application is divided into three main components:

1.  **Frontend (Streamlit):** The `streamlit-app.py` file creates the user interface. Users can select a movie from a dropdown, get recommendations, and add new movies via a sidebar form. It communicates with the backend via HTTP requests.

2.  **Backend (FastAPI):** The `myapp/main.py` file defines a RESTful API with endpoints for:
    - Serving movie recommendations.
    - Tracking user clicks on recommendations.
    - Retrieving click-through statistics.
    - Adding new movies to the database.

3.  **Recommendation Engine & Database:**
    - The core recommendation logic is in `myapp/recommender.py`, which uses `pandas` and `scikit-learn` to build a similarity matrix.
    - SQLAlchemy is used as the ORM to interact with a database (defaulting to SQLite) that stores movie data, ratings, and click statistics.

## Technology Stack

- **Backend:** FastAPI, Uvicorn
- **Frontend:** Streamlit
- **ML/Data Science:** Pandas, NumPy, Scikit-learn
- **Database:** SQLAlchemy, SQLite, Psycopg2-binary (for PostgreSQL)

## Dataset

This system is designed to work with the **MovieLens 100K Dataset**. You can download it from the official GroupLens website.

## Setup and Installation

Follow these steps to get the project running on your local machine.

### 1. Clone the Repository

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
