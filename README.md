# Phind API Reference
 
 ## Overview
 
 Phind is an AI answer engine that provides users with the ability to interact with an AI model to get answers, assistance with queries, and engage in chat conversations. This API reference details the endpoints available for interacting with Phind's services.

 ### This is not finished and subject to change
 
 You might notice that this doc looks a little barebones - I'm working on properly reverse enginering their backend. For now, this is what you get (the old docs were out of date and a lot had changed!).

 
 ## Authentication
 
 ### Get Session
 
 - **Endpoint:** `GET /api/auth/session`
 - **Description:** Retrieves the current session information for the user.
 - **Headers:**
   - `Accept`: `*/*`
   - `Accept-Encoding`: `gzip, deflate, br`
   - `Accept-Language`: Language preferences (e.g., `en-GB,en;q=0.9,en-US;q=0.8`)
 - **Query Parameters:**
   - None.
 - **Response:**
   - **Status Code:** `304 Not Modified` (if session is unchanged) or `200 OK` (if a new session is created).
   - **Content-Type:** `application/json`
   - **Body:** An empty JSON object `{}` or session details.
 
 ## Querying
 
 ### Make a Query
 
 - **Endpoint:** `POST https://https.api.phind.com/infer/`
 - **Description:** Submits a user's question to the AI model and receives an answer.
 - **Headers:**
   - `Content-Type`: `application/json;charset=UTF-8`
   - `Origin`: `https://www.phind.com`
 - **Payload:**
   - `question`: The user's query.
   - `options`: Various options for customizing the query (e.g., `date`, `language`, `detailed`, etc.).
   - `context`: Optional context for the question.
   - `challenge`: A numeric value included for anti-fraud or verification purposes.
 - **Response:**
   - **Status Code:** `200 OK`
   - **Content-Type:** `text/event-stream; charset=utf-8`
   - **Body:** A stream of events containing the AI's responses and follow-up prompts.
 
 ### Cache Query Result
 
 - **Endpoint:** `POST /api/db/cache`
 - **Description:** Stores the results of a query in cache.
 - **Headers:**
   - `Content-Type`: `application/json;charset=UTF-8`
   - `Origin`: `https://www.phind.com`
 - **Payload:**
   - `title`: The title of the query.
   - `value`: An array containing the query and its result.
   - `challenge`: A numeric value for verification.
 - **Response:**
   - **Status Code:** `200 OK`
   - **Content-Type:** `application/json; charset=utf-8`
   - **Body:** A JSON object containing a `request_id`.
 
 ## Chat
 
 ### Preflight Request
 
 - **Endpoint:** `OPTIONS https://https.api.phind.com/agent/`
 - **Description:** A preflight request for CORS that precedes the actual request to the chat endpoint.
 - **Headers:**
   - `Access-Control-Request-Headers`: `content-type`
   - `Access-Control-Request-Method`: `POST`
   - `Origin`: `https://www.phind.com`
 - **Response:**
   - **Status Code:** `200 OK`
   - **Content-Type:** `text/plain; charset=utf-8`
   - **Headers:** Appropriate CORS headers.
 
 ### Send Chat Message
 
 - **Endpoint:** `POST https://https.api.phind.com/agent/`
 - **Description:** Sends a message to the AI chat agent and receives a response.
 - **Headers:**
   - `Content-Type`: `application/json;charset=UTF-8`
   - `Origin`: `https://www.phind.com`
 - **Payload:**
   - `user_input`: The user's chat message.
   - `message_history`: An array of previous messages in the conversation.
   - `requested_model`: The AI model being used for the chat.
   - `anon_user_id`: An anonymous identifier for the user.
   - `challenge`: A numeric value for verification.
 - **Response:**
   - **Status Code:** `200 OK`
   - **Content-Type:** `text/event-stream; charset=utf-8`
   - **Body:** A stream of events containing the chat agent's responses.
 
 ### Store Chat Message
 
 - **Endpoint:** `POST /api/db/chat`
 - **Description:** Stores a chat message in the database.
 - **Headers:**
   - `Content-Type`: `application/json;charset=UTF-8`
   - `Origin`: `https://www.phind.com`
 - **Payload:**
   - `title`: The title or subject of the chat.
   - `messages`: An array of message objects with `role`, `content`, and `metadata`.
   - `challenge`: A numeric value for verification.
 - **Response:**
   - **Status Code:** `200 OK`
   - **Content-Type:** `application/json; charset=utf-8`
   - **Body:** A JSON object with details about the stored chat, including an `id`.
 
 ## Notes
 
 - All endpoints are served over HTTPS and require proper headers to be set for CORS and content type.
 - The `challenge` field in requests is used for security purposes and may be part of anti-fraud measures.
 - Responses, especially those involving AI interactions, are returned as streams of events and may require special handling to process.
 - The exact request content and responses will differ per user and context.
 - This API Reference is based on observed requests and responses and is unofficial.
