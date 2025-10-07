# Technical Project for Feedback Bot

## I. Hierarchical Structure of Folders and Modules

telegram_bot/
├── src/
│  ├── main.rs      # Entry point of the application
│  ├── bot/        # Main modules of the bot
│  │  ├── mod.rs     # Declaration of the bot module
│  │  ├── api_client.rs # Interaction with Telegram Bot API
│  │  ├── message_router.rs # Routing of incoming messages
│  │  ├── command_handler.rs # Command handling
│  │  ├── intent_recognizer.rs # User intent recognition
│  │  ├── dialog_manager.rs # Dialog management
│  │  ├── ai_agent.rs   # Interaction with AI agent (Llama-3)
│  │  ├── models.rs   # Definitions of data structures (User, Message, etc.)
│  │  ├── commands/   # Command modules
│  │  │  ├── mod.rs   # Declaration of the commands module
│  │  │  ├── start.rs  # Command /start
│  │  │  ├── help.rs  # Command /help
│  │  │  └── ...    # Other commands
│  ├── db/        # Module for database operations
│  │  ├── mod.rs     # Declaration of the db module
│  │  ├── connection.rs # Database connection management
│  │  ├── models.rs   # Data models for the database
│  │  ├── queries.rs   # SQL queries or ORM operations
│  ├── events/      # Module for event handling
│  │  ├── mod.rs     # Declaration of the events module
│  │  ├── event_bus.rs  # Implementation of the event bus
│  │  ├── handlers/   # Event handlers
│  │  │  ├── mod.rs   # Declaration of the handlers module
│  │  │  ├── dialog.rs # Event handler for dialog events
│  │  │  └── ...    # Other handlers
│  ├── config/      # Module for configuration management
│  │  ├── mod.rs     # Declaration of the config module
│  │  ├── settings.rs  # Loading and storing settings
│  ├── nlp/        # Module for natural language processing
│  │  ├── mod.rs     # Declaration of the nlp module
│  │  ├── intent.rs   # Module for intent recognition
│  │  ├── entity.rs   # Module for entity extraction
│  ├── utils/       # Utility functions
│  │  ├── mod.rs     # Declaration of the utils module
│  │  ├── errors.rs   # Definition of custom errors
│  ├── main.rs
│  └── utils/
├── Cargo.toml     # Rust manifest file
└── README.md      # Project description

## II. Technical Specification of Modules

### 1. src/main.rs

- **Responsibility:** Entry point of the application.
- **Functionality:**
  - Initialize the logger.
  - Load configuration.
  - Initialize the database connection.
  - Create an instance of TelegramBotApiClient.
  - Start the update handler from Telegram Bot API.
- **Dependencies:** config, tracing, db, bot

### 2. src/bot/mod.rs

- **Responsibility:** Declaration of the bot module.
- **Functionality:** Exports all sub-modules (api_client, message_router, command_handler, etc.).

### 3. src/bot/api_client.rs

- **Responsibility:** Interaction with Telegram Bot API.
- **Functionality:**
  - Initialize the Telegram Bot API client (using teloxide or tbot).
  - Receive updates from Telegram Bot API (using long polling or webhooks).
  - Send messages to Telegram.
  - Handle errors related to Telegram Bot API.
- **Dependencies:** teloxide or tbot, config

### 4. src/bot/message_router.rs

- **Responsibility:** Routing of incoming messages.
- **Functionality:**
  - Receive messages from TelegramBotApiClient.
  - Determine the message type (command, text, etc.).
  - Route the message to the appropriate handler (Command Handler, Intent Recognizer).
- **Dependencies:** command_handler, intent_recognizer

### 5. src/bot/command_handler.rs

- **Responsibility:** Handling commands (messages starting with /).
- **Functionality:**
  - Receive messages from MessageRouter.
  - Extract the command name and arguments.
  - Call the appropriate command handler (e.g., start.rs, help.rs).
- **Dependencies:** commands, db, ai_agent

### 6. src/bot/commands/mod.rs

- **Responsibility:** Declaration of the commands module.
- **Functionality:** Exports all sub-modules of commands (start.rs, help.rs, etc.).

### 7. src/bot/commands/start.rs

- **Responsibility:** Handling the /start command.
- **Functionality:**
  - Greet the user.
  - Save user information to the database (if necessary).
- **Dependencies:** db

### 8. src/bot/commands/help.rs

- **Responsibility:** Handling the /help command.
- **Functionality:**
  - Provide a list of available commands and their descriptions.

### 9. src/bot/intent_recognizer.rs

- **Responsibility:** User intent recognition.
- **Functionality:**
  - Receive messages from MessageRouter.
  - Use an NLP model to determine the user's intent.
  - Return the intent and extracted entities.
- **Dependencies:** nlp

### 10. src/bot/dialog_manager.rs

- **Responsibility:** Managing the dialog with the user.
- **Functionality:**
  - Track the current state of the dialog.
  - Determine the next step in the dialog.
  - Store dialog information in the database (if necessary).
- **Dependencies:** db, events

### 11. src/bot/ai_agent.rs

- **Responsibility:** Interaction with the AI agent (Llama-3).
- **Functionality:**
  - Receive requests from CommandHandler or DialogManager.
  - Formulate the request for Llama-3.
  - Receive the response from Llama-3.
  - Return the response to the user.
- **Dependencies:** config, llama3 (or wrapper for interacting with your implementation of Llama-3)

### 12. src/db/mod.rs

- **Responsibility:** Declaration of the db module.
- **Functionality:** Exports sub-modules for database operations (connection, models, queries).

### 13. src/db/connection.rs

- **Responsibility:** Database connection management.
- **Functionality:**
  - Initialize the database connection (PostgreSQL, MongoDB, SQLite).
  - Provide access to the connection for other modules.
  - Handle database connection errors.

### 14. src/db/models.rs

- **Responsibility:** Definitions of data models for the database.
- **Functionality:**
  - Define data structures to represent user, message, dialog information, etc.
  - Use an ORM (e.g., Diesel or SeaORM) to map data structures to database tables.

### 15. src/db/queries.rs

- **Responsibility:** Definitions of SQL queries or ORM operations for database interactions.
- **Functionality:**
  - Execute queries to retrieve, add, modify, and delete data.

### 16. src/events/mod.rs

- **Responsibility:** Declaration of the events module.
- **Functionality:** Exports sub-modules for event handling (event_bus, handlers).

### 17. src/events/event_bus.rs

- **Responsibility:** Implementation of the event bus.
- **Functionality:**
  - Register event handlers.
  - Publish events.
  - Notify registered handlers when events occur.

### 18. src/events/handlers/mod.rs

- **Responsibility:** Declaration of the handlers module.
- **Functionality:** Exports all sub-modules of event handlers (dialog.rs, etc.).

### 19. src/events/handlers/dialog.rs

- **Responsibility:** Handling events related to the dialog.
- **Functionality:**
  - React to changes in the dialog state.
  - Perform actions related to changes in the dialog state (e.g., send a message to the user).

### 20. src/config/mod.rs

- **Responsibility:** Declaration of the config module.
- **Functionality:** Exports sub-modules for configuration management (settings.rs).

### 21. src/config/settings.rs

- **Responsibility:** Loading and storing application settings.
- **Functionality:**
  - Read settings from a configuration file (e.g., TOML, YAML).
  - Provide access to settings for other modules.

### 22. src/nlp/mod.rs

- **Responsibility:** Declaration of the nlp module.
- **Functionality:** Exports sub-modules for natural language processing (intent, entity).

### 23. src/nlp/intent.rs

- **Responsibility:** User intent recognition.
- **Functionality:**
  - Use an NLP model to determine the user's intent.

### 24. src/nlp/entity.rs

- **Responsibility:** Entity extraction from user messages.
- **Functionality:**
  - Use an NLP model to extract entities.

### 25. src/utils/mod.rs

- **Responsibility:** Declaration of the utils module.
- **Functionality:** Exports sub-modules for utility functions (errors.rs).

### 26. src/utils/errors.rs

- **Responsibility:** Definition of custom errors.
- **Functionality:**
  - Define error structures to represent various errors that can occur in the application.

## III. Additional Considerations

- **Testing:** Unit and integration tests for each module.
- **Documentation:** Documentation of each module and its functions.
- **Error Handling:** Implement robust error handling.
- **Logging:** Use a logging system.