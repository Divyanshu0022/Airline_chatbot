# FlightAI - An Advanced Airline Support Assistant ✈️

A multi-modal customer support agent for an airline, complete with a dedicated UI. It leverages LLM function-calling to fetch flight data, answer queries, and simulate bookings, showcasing a practical application of modern conversational AI.
<img width="1920" height="1047" alt="Screenshot (425)" src="https://github.com/user-attachments/assets/3df3a7f5-fb6e-44e7-962c-36225e2d033d" />


---

## ✨ Features

-   **Conversational Interface:** An intuitive and user-friendly chat UI built with Gradio.
-   **Tool-Enabled LLM:** Utilizes the powerful Google Gemini 1.5 Flash model through `litellm` to understand and respond to user queries.
-   **Dynamic Actions via Function Calling:** The assistant can perform real-world actions beyond just text generation:
    -   **Check Ticket Prices:** Instantly fetches prices for flights to various destinations.
    -   **Check Flight Availability:** Provides up-to-date information on available seats and flight times.
    -   **Book Flights:** Simulates the flight booking process for a specific passenger on a chosen flight.
-   **Context-Aware Conversations:** Maintains the history of the conversation to handle follow-up questions and complex requests seamlessly.

---

## 🛠️ Tech Stack

-   **Language:** Python
-   **LLM Interaction:** `litellm` (for a consistent interface with models like Gemini)
-   **LLM Model:** `Google Gemini 1.5 Flash`
-   **UI Framework:** `Gradio`
-   **Environment Management:** `python-dotenv`

---

## ⚙️ Project Workflow

The project follows a sophisticated workflow to handle user requests, especially those requiring external data or actions.

1.  **User Input:** The user types a message into the Gradio chat interface.
2.  **LLM Request:** The message, along with the conversation history, is sent to the Gemini LLM.
3.  **Decision Making:** The LLM analyzes the request and decides if it can answer directly or if it needs to use one of the provided tools (`get_ticket_price`, `get_flight_availability`, `book_flight`).
4.  **Tool Call:** If a tool is needed, the LLM responds with a `tool_calls` object instead of a direct answer. This object specifies which function to run and what arguments to use (e.g., `{"destination_city": "London"}`).
5.  **Function Execution:** The Python backend parses this request, calls the appropriate local function (e.g., `get_ticket_price("London")`), and executes it.
6.  **Return to LLM:** The result from the Python function (e.g., a ticket price of "$799") is sent back to the LLM.
7.  **Final Response:** The LLM receives the tool's output and uses this new information to formulate a final, human-readable response to the user (e.g., "The price for a ticket to London is $799.").
8.  **Display:** The final response is displayed in the Gradio UI.

```
graph TD
    A[User sends query via Gradio UI] --> B{LLM processes query};
    B -->|Can answer directly| C[Generates text response];
    B -->|Needs a tool| D[Returns 'tool_calls' object];
    D --> E[handle_tool_call function];
    E --> F{Identifies which tool to run};
    F -->|get_ticket_price| G[Runs get_ticket_price()];
    F -->|get_flight_availability| H[Runs get_flight_availability()];
    F -->|book_flight| I[Runs book_flight()];
    G --> J[Tool result];
    H --> J;
    I --> J;
    J --> K[Send result back to LLM];
    K --> L[LLM generates final response];
    C --> M[Display response in UI];
    L --> M;
```
