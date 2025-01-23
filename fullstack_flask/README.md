# Flask-based Chatbot

This repository includes a simple Python Flask app that streams responses from OpenAI
to an HTML/JS frontend using [NDJSON](http://ndjson.org/) over a [ReadableStream](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream).

## Start the app
1. Using conda to create a new environment
    ```shell
    conda create -n myenv python=3.11
    ```

2. Install required packages
    ```shell
    pip install -r requirements-dev.txt
    ```

3. Create a .env file, and put your OPENAI_API_KEY and PINECONE_API_KEY in this.

4. Change the port in gunicorn.conf.py according to your machine's setup.

5. For the first time to start the app, you need to create the database
    Using the following code in python to initialize the DB:
    ```python
    from src.app import app, db
    with app.app_context():
        db.create_all()
    ```

6. Start the flask app
    ```shell
    gunicorn src.app:app
    ```

    If you want to start it in the background
    ```shell
    nohup gunicorn app:app &
    ```

7. Navigate to 'http://localhost:50505' to access this Web app if it's local environment. Change the port to the one you specified if needed.


## explained by chatGPT

The code creates a Flask-based chatbot application that streams responses from OpenAI to a web frontend. Here’s a step-by-step explanation of how the app is constructed:

1. **App Initialization (

app.py

)**:
   - A Flask app is created and configured.
   - The database URI is set based on whether the app is running in production or not.
   - SQLAlchemy is used for database interactions.
   - The app imports views and models.

2. **Database Models (

models.py

)**:
   - Defines two models: 

User

 and 

ChatMessage

.
   - 

User

 model stores user information.
   - 

ChatMessage

 model stores chat history with questions and answers.

3. **Search Functionality (

search.py

)**:
   - Uses DuckDuckGo search to fetch news and text results based on keywords.
   - Provides functions 

search_news

 and 

search_text

 to get search results.

4. **Chat API (`chat_api.py`)**:
   - Defines the 

call_chat

 function to handle chat interactions.
   - Uses OpenAI's API to generate responses based on a prompt template.
   - Streams the response tokens to the client.
   - Saves the chat history to the database.

5. **Views (

views.py

)**:
   - Defines routes for the Flask app.
   - 

index

 route renders the main HTML page.
   - 

chat_handler

 route handles chat requests and streams responses.
   - 

get_user

 route retrieves user data from the database.

6. **Gunicorn Configuration (`gunicorn.conf.py`)**:
   - Configures Gunicorn server settings.
   - Sets the number of workers and threads based on the environment.
   - Specifies the port and other settings for running the app.

7. **Environment Setup (`README.md`)**:
   - Instructions for setting up the environment, installing dependencies, and configuring the app.
   - Details on creating a 

.env

 file for storing API keys.
   - Instructions for initializing the database.

This setup allows the app to handle real-time communication between the client and the server, providing a responsive chatbot experience.