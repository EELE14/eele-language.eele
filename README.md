# EELE Language - Complete Documentation

## Table of Contents

1. [Introduction](#introduction)
2. [Installation and Setup](#installation-and-setup)
3. [Basic Syntax](#basic-syntax)
4. [Data Types](#data-types)
5. [Variables and Constants](#variables-and-constants)
6. [Operators](#operators)
7. [Control Structures](#control-structures)
8. [Functions](#functions)
9. [Event System](#event-system)
10. [Discord Commands](#discord-commands)
11. [Built-in Functions](#built-in-functions)
12. [Scheduling](#scheduling)
13. [Error Handling](#error-handling)
14. [Advanced Features](#advanced-features)
15. [Compilation and Execution](#compilation-and-execution)
16. [Best Practices](#best-practices)
17. [Example: Full Bot](#example-full-bot)

---

## Introduction

**EELE** is an easy-to-learn, universal programming language built specifically for creating Discord bots. It blends the simplicity of natural language with the power of a full programming language.

### Key Features

* **Case-insensitive** – keywords are not case-sensitive
* **Simple syntax** – clear, descriptive keywords
* **Event-based** – reacts to Discord events
* **Type-safe** – automatic type inference
* **Extensible** – add new keywords and modules

---

## Installation and Setup

### Requirements

```bash
pip install lark discord.py python-dotenv
```

### Project Structure

```
eele-project/
├── eele_compiler.py      # Main compiler
├── examples/             # Example programs
│   ├── hello_world.eele
│   ├── interactive.eele
│   └── moderation.eele
├── .env                  # Environment variables
└── README.md
```

### Environment Setup

Create a `.env` file:

```env
DISCORD_TOKEN=your_discord_bot_token_here
```

---

## Basic Syntax

### Bot Declaration

Every EELE program begins with a bot declaration:

```eele
BOT "MyBotName"
```

### Comments

```eele
// Single-line comment

/*
   Multi-line
   comment
*/
```

### Blocks

Blocks are defined with curly braces:

```eele
IF condition {
  // Code here
}
```

---

## Data Types

### Primitive Types

| Type        | Syntax          | Example            |
| ----------- | --------------- | ------------------ |
| **STRING**  | `"text"`        | `"Hello World"`    |
| **NUMBER**  | `42`, `3.14`    | `42`, `3.14159`    |
| **BOOLEAN** | `TRUE`, `FALSE` | `TRUE`             |
| **NULL**    | `NULL`          | `NULL`             |
| **EMOJI**   | Emoji character | 😀, ⭐, \:thumbsup: |

### Collections

```eele
// Lists
LET numbers = [1, 2, 3, 4, 5]
LET texts = ["a", "b", "c"]

// Maps/Objects (optional in v1)
LET person = {
  name: "Max",
  age: 25,
  active: TRUE
}
```

---

## Variables and Constants

### Variable Declaration

```eele
LET variable_name = value       // Mutable
CONST constant_name = value     // Immutable
```

### Examples

```eele
LET name = "EELE"
LET counter = 0
CONST MAX_TRIES = 3

// Assignments
SET counter = counter + 1
SET name = "NewName"

// ERROR: Constants cannot be changed
// SET MAX_TRIES = 5  // RuntimeError!
```

### Scope

```eele
LET global_var = "global"

IF TRUE {
  LET local_var = "local"        // Only visible inside this block
  SET global_var = "changed"      // Modify global variable
}

// local_var is not available here
```

---

## Operators

### Arithmetic Operators

```eele
LET a = 10
LET b = 3

LET sum = a + b           // 13
LET diff = a - b          // 7
LET prod = a * b          // 30
LET quot = a / b          // 3.333...
LET rem = a % b           // 1
```

### Comparison Operators

```eele
LET x = 5
LET y = 10

IF x == y { SAY "Equal" }
IF x != y { SAY "Not equal" }
IF x < y  { SAY "x is smaller" }
IF x <= y { SAY "x is smaller or equal" }
IF x > y  { SAY "x is greater" }
IF x >= y { SAY "x is greater or equal" }
```

### Logical Operators

```eele
LET a = TRUE
LET b = FALSE

IF a AND b { SAY "Both true" }
IF a OR b  { SAY "At least one is true" }
IF NOT a   { SAY "a is false" }

// Combinations
IF (x > 5) AND (y < 20) {
  SAY "x is greater than 5 and y is less than 20"
}
```

### String Concatenation

```eele
LET first = "Max"
LET last  = "Mustermann"
LET full  = first + " " + last  // "Max Mustermann"

LET n = 42
LET text = "The answer is " + n // "The answer is 42"
```

---

## Control Structures

### IF-ELSE Statements

```eele
IF condition {
  // Code when true
} ELSEIF other_condition {
  // Code when other_condition is true
} ELSE {
  // Code when all others are false
}
```

**Example:**

```eele
LET age = 18

IF age < 13 {
  SAY "Child"
} ELSEIF age < 18 {
  SAY "Teenager"
} ELSEIF age < 65 {
  SAY "Adult"
} ELSE {
  SAY "Senior"
}
```

### WHILE Loops

```eele
WHILE condition {
  // Repeat code
}
```

**Example:**

```eele
LET countdown = 5
WHILE countdown > 0 {
  SAY "Countdown: " + countdown
  WAIT 1
  SET countdown = countdown - 1
}
SAY "Start! 🚀"
```

### LOOP (Counting Loop)

```eele
LOOP times {
  // Repeat n times
}
```

**Examples:**

```eele
// Simple repetition
LOOP 3 {
  SAY "Hello!"
}

// With variable
LET repeats = 5
LOOP repeats {
  REACT ⭐
}
```

### Loop Control

```eele
LET i = 0
WHILE i < 10 {
  SET i = i + 1
  
  IF i == 3 {
    CONTINUE   // Skip the rest of this iteration
  }
  
  IF i == 8 {
    BREAK      // Exit the loop entirely
  }
  
  SAY "Number: " + i
}
```

---

## Functions

### Function Definition

```eele
FUNC function_name(param1, param2) {
  // Function body
  RETURN value    // Optional
}
```

### Examples

```eele
// Simple function without parameters
FUNC greet() {
  SAY "Hello from the function!"
}

// Function with parameters
FUNC add(a, b) {
  RETURN a + b
}

// Function with logic
FUNC classify_age(age) {
  IF age < 18 {
    RETURN "minor"
  } ELSE {
    RETURN "adult"
  }
}

// Calls
greet()
LET sum = add(5, 3)             // 8
LET status = classify_age(20)    // "adult"
```

---

## Event System

The event system is the heart of EELE. Events react to Discord activity.

### Core Events

#### ON READY

Executed when the bot successfully connects to Discord.

```eele
ON READY {
  SAY "Bot is ready! 👋"
  SET_STATUS "online" WITH "EELE Language"
}
```

#### ON MESSAGE

Reacts to exact messages.

```eele
ON MESSAGE "!ping" {
  SAY "Pong! 🏓"
}

ON MESSAGE "!hello" {
  LET user = AUTHOR.NAME
  SAY "Hello " + user + "! 😊"
}
```

#### ON MATCH (Regex)

Reacts to messages that match a regular expression.

```eele
// Simple regex
ON MATCH /!echo (.+)/ {
  LET message = MATCH.GROUP(1)
  SAY "You said: " + message
}

// Numbers
ON MATCH /!roll (\d+)/ {
  LET sides = MATCH.GROUP(1)
  LET result = RANDOM_INT(1, sides)
  SAY "🎲 " + result + " (d" + sides + ")"
}
```

### Reaction Events

```eele
ON REACTION_ADD {
  IF REACTION == "👍" {
    SAY "Someone likes this! 👍"
  }
}

ON REACTION_REMOVE {
  SAY "Reaction removed: " + REACTION
}
```

### Member Events

```eele
ON MEMBER_JOIN {
  LET channel = FETCH CHANNEL BY_NAME "welcome"
  IF channel {
    SEND TO channel "Welcome " + USER.NAME + "! 🎉"
  }
}

ON MEMBER_LEAVE {
  LOG "Member left: " + USER.NAME
}
```

### Error Events

```eele
ON ERROR err {
  LOG "Error occurred: " + err.MESSAGE
  SAY "Sorry, something went wrong! 😅"
}
```

---

## Discord Commands

### Sending Messages

```eele
// Send to current channel
SAY "Hello World!"

// Reply to a message
REPLY "This is a reply"

// Send a direct message
DM USER target_user "Private message"

// Send to a specific channel
LET ch = FETCH CHANNEL BY_NAME "bot-spam"
SEND TO ch "Message to #bot-spam"
```

### Reactions

```eele
// Single reaction
REACT 😀
REACT ⭐
REACT :thumbsup:

// Multiple reactions
LOOP 3 {
  REACT ❤️
}
```

### Users and Channels

```eele
// Find a user
FETCH USER BY_ID "123456789" AS user
IF user {
  SAY "Found: " + user.NAME
}

// Find a channel
FETCH CHANNEL BY_NAME "general" AS channel
IF channel {
  SEND TO channel "Hello #general!"
}
```

### Status and Presence

```eele
// Set status
SET_STATUS "online" WITH "EELE Language"
SET_STATUS "idle" WITH "Break..."
SET_STATUS "dnd" WITH "Do not disturb"

// Without activity text
SET_STATUS "online"
```

### Timing

```eele
// Wait
WAIT 5           // wait 5 seconds
WAIT 0.5         // wait 0.5 seconds

// Combined
SAY "Message 1"
WAIT 2
SAY "Message 2 (after 2 seconds)"
```

---

## Built-in Functions

### String and List Length

```eele
// Length
LET text = "Hello"
LET length = LEN(text)       // 5

LET list = [1, 2, 3, 4]
LET count = LEN(list)        // 4
```

### List Manipulation

```eele
LET my_list = [1, 2, 3]

// Add
PUSH my_list 4           // [1, 2, 3, 4]
APPEND my_list 5         // [1, 2, 3, 4, 5]

// Remove
POP my_list              // Removes last element
REMOVE my_list 0         // Removes element at index 0
```

### Time and Date

```eele
// Current time
LET now = NOW()
PRINT now                 // Unix timestamp

// Formatting
LET tstr = FORMAT_DATETIME(now, "%H:%M:%S")
SAY "It is " + tstr

LET dstr = FORMAT_DATETIME(now, "%d.%m.%Y")
SAY "Today is " + dstr
```

### Random Numbers

```eele
// Random float between 0 and 1
LET r = RANDOM()

// Random integers
LET dice = RANDOM_INT(1, 6)      // 1..6
LET draw = RANDOM_INT(1, 100)    // 1..100
```

### Environment Variables

```eele
LET token = ENV("DISCORD_TOKEN")
LET debug = ENV("DEBUG") == "true"

IF debug {
  LOG "Debug mode enabled"
}
```

### JSON Processing

```eele
// Parse JSON
LET json_string = '{"name": "Max", "age": 25}'
LET obj = JSON_PARSE(json_string)
SAY "Name: " + obj.name

// Create JSON
LET data = {name: "Anna", age: 30}
LET json_out = JSON_STRINGIFY(data)
PRINT json_out
```

### Debugging and Logging

```eele
// Console output
PRINT "Debug info"
PRINT var1, var2

// Structured logs
LOG "User " + AUTHOR.NAME + " executed a command"

// Assertions (tests)
ASSERT age > 0, "Age must be positive"
ASSERT LEN(name) > 0, "Name must not be empty"
```

---

## Scheduling

### Recurring Tasks

```eele
// Every 30 seconds
SCHEDULE EVERY "30s" {
  LOG "30 seconds passed"
}

// Every 5 minutes
SCHEDULE EVERY "5m" {
  LET ch = FETCH CHANNEL BY_NAME "status"
  SEND TO ch "5-minute update 📊"
}

// Every hour
SCHEDULE EVERY "1h" {
  SET_STATUS "online" WITH "Hourly update"
}
```

### Daily Events

```eele
// Every day at 09:00
SCHEDULE AT "09:00" {
  LET ch = FETCH CHANNEL BY_NAME "general"
  SEND TO ch "Good morning! ☀️"
}

// Every day at 22:00
SCHEDULE AT "22:00" {
  SAY "Good night! 🌙"
  SET_STATUS "idle" WITH "Bedtime"
}
```

### Time Format Spec

| Format    | Description | Example   |
| --------- | ----------- | --------- |
| `"Ns"`    | N seconds   | `"30s"`   |
| `"Nm"`    | N minutes   | `"5m"`    |
| `"Nh"`    | N hours     | `"2h"`    |
| `"HH:MM"` | Daily time  | `"09:30"` |

---

## Error Handling

### Basic Error Handler

```eele
ON ERROR err {
  LOG "Error: " + err.MESSAGE
  SAY "Sorry, an error occurred!"
}
```

### Try-Catch Pattern (via Functions)

```eele
FUNC safe_divide(a, b) {
  IF b == 0 {
    LOG "Prevented division by zero"
    RETURN NULL
  }
  RETURN a / b
}

LET result = safe_divide(10, 0)
IF result != NULL {
  SAY "Result: " + result
} ELSE {
  SAY "Division was not possible"
}
```

### Validation Example

```eele
ON MESSAGE "!age" {
  LET age_input = INPUT_AFTER_COMMAND()
  
  IF NOT IS_NUMBER(age_input) {
    SAY "Please enter a valid number!"
    RETURN
  }
  
  LET age = TO_NUMBER(age_input)
  
  IF age < 0 OR age > 150 {
    SAY "Age must be between 0 and 150!"
    RETURN
  }
  
  SAY "Your age: " + age + " years"
}
```

---

## Advanced Features

### Context Variables

EELE provides context variables automatically:

```eele
ON MESSAGE "!info" {
  SAY "Author: " + AUTHOR.NAME
  SAY "Channel: " + CHANNEL.NAME
  SAY "Server: " + GUILD.NAME
  SAY "Message ID: " + MESSAGE.ID
}
```

### Available Context Objects

| Variable   | Available in    | Description            |
| ---------- | --------------- | ---------------------- |
| `AUTHOR`   | MESSAGE events  | Author of the message  |
| `CHANNEL`  | MESSAGE events  | Channel of the message |
| `GUILD`    | Most events     | Discord server         |
| `MESSAGE`  | MESSAGE events  | Message object         |
| `USER`     | MEMBER events   | Affected user          |
| `REACTION` | REACTION events | The reaction           |

### Object Properties

```eele
// User properties
AUTHOR.NAME          // Display name
AUTHOR.ID            // User ID
AUTHOR.MENTION       // @mention

// Channel properties
CHANNEL.NAME         // Channel name
CHANNEL.ID           // Channel ID
CHANNEL.MENTION      // #channel mention

// Guild properties
GUILD.NAME           // Server name
GUILD.MEMBER_COUNT   // Member count
GUILD.ID             // Server ID
```

### Permissions and Roles

```eele
ON MESSAGE "!mod" {
  IF AUTHOR.HAS_PERMISSION("manage_messages") {
    SAY "You are a moderator! 👮"
  } ELSE {
    SAY "You do not have moderation rights."
  }
}

// Role management (admin rights required)
ON MESSAGE "!promote" {
  IF AUTHOR.HAS_PERMISSION("manage_roles") {
    LET target = MENTIONED_USERS[0]
    ADD_ROLE USER target ROLE "Moderator"
    SAY target.NAME + " has been promoted to Moderator!"
  }
}
```

### Advanced List Operations

```eele
LET numbers = [1, 2, 3, 4, 5]

// Filtering (with a helper function)
FUNC is_even(n) {
  RETURN n % 2 == 0
}

// Iteration
FOR n IN numbers {
  IF is_even(n) {
    SAY n + " is even"
  }
}
```

---

## Compilation and Execution

### Run Directly

```bash
# Run an EELE program directly
python eele_compiler.py my_bot.eele

# With debug output
python eele_compiler.py my_bot.eele --debug

# Provide token explicitly
python eele_compiler.py my_bot.eele --token "BOT_TOKEN_HERE"
```

### Compile to Python

```bash
# Compile to Python code
python eele_compiler.py my_bot.eele --compile

# Specify output file
python eele_compiler.py my_bot.eele --compile -o discord_bot.py
```

### Run Tests

```bash
# Run built-in tests
python eele_compiler.py --test
```

---

## Best Practices

### 1. Code Organization

```eele
BOT "MyBot"

// Define constants at the top
CONST MAX_MESSAGES = 100
CONST BOT_VERSION = "1.0.0"

// Utility functions
FUNC is_admin(user) {
  RETURN user.HAS_PERMISSION("administrator")
}

// Event handlers afterwards
ON READY {
  LOG "Bot started - version " + BOT_VERSION
}
```

### 2. Error Handling

```eele
// Always validate input
ON MESSAGE "!kick" {
  IF NOT is_admin(AUTHOR) {
    SAY "No permission!"
    RETURN
  }
  
  IF LEN(MENTIONED_USERS) == 0 {
    SAY "Please mention a user!"
    RETURN
  }
  
  // Then perform the action
}
```

### 3. Modular Functions

```eele
// Reusable functions
FUNC send_embed(title, description, color) {
  // Embed logic here
}

FUNC log_action(user, action) {
  LET ts = FORMAT_DATETIME(NOW(), "%H:%M:%S")
  LOG ts + " - " + user.NAME + ": " + action
}
```

### 4. Performance

```eele
// Avoid unnecessary API calls
LET admin_channel = NULL

ON READY {
  SET admin_channel = FETCH CHANNEL BY_NAME "admin-log"
}

FUNC log_to_admin(msg) {
  IF admin_channel {
    SEND TO admin_channel msg
  }
}
```

---

## Example: Full Bot

```eele
BOT "FullExampleBot"

// Configuration
CONST PREFIX = "!"
CONST ADMIN_ROLE = "Moderator"

// Global variables
LET user_points = {}
LET bot_enabled = TRUE

// Helpers
FUNC is_moderator(user) {
  RETURN user.HAS_ROLE(ADMIN_ROLE)
}

FUNC get_points(user_id) {
  IF user_id IN user_points {
    RETURN user_points[user_id]
  }
  RETURN 0
}

// Bot start
ON READY {
  SET_STATUS "online" WITH "EELE Language v1.0"
  LOG "Bot started successfully!"
}

// Basic commands
ON MESSAGE PREFIX + "ping" {
  SAY "Pong! 🏓"
}

ON MESSAGE PREFIX + "help" {
  SAY "Available: !ping, !points, !daily"
}

// Points system
ON MESSAGE PREFIX + "points" {
  LET pts = get_points(AUTHOR.ID)
  SAY AUTHOR.NAME + " has " + pts + " points! ⭐"
}

ON MESSAGE PREFIX + "daily" {
  LET bonus = RANDOM_INT(10, 50)
  SET user_points[AUTHOR.ID] = get_points(AUTHOR.ID) + bonus
  SAY "You received " + bonus + " points! 🎉"
}

// Moderation
ON MESSAGE PREFIX + "clear" {
  IF is_moderator(AUTHOR) {
    SAY "Cleaning channel! 🧹"
    // Clear logic here
  } ELSE {
    SAY "You don't have permission for this command!"
  }
}

// Automation
SCHEDULE EVERY "1h" {
  IF bot_enabled {
    SET_STATUS "online" WITH "Active since " + FORMAT_DATETIME(NOW(), "%H:%M")
  }
}

SCHEDULE AT "00:00" {
  SAY "🌙 A new day begins! Daily rewards have been reset."
}

// Welcome new members
ON MEMBER_JOIN {
  LET welcome = FETCH CHANNEL BY_NAME "welcome"
  IF welcome {
    SEND TO welcome "Welcome " + USER.MENTION + "! 🎉\nType !help for commands."
  }
}

// Error handling
ON ERROR err {
  LOG "Error occurred: " + err.MESSAGE
  SAY "Sorry, something went wrong! 😅"
}
```

---

