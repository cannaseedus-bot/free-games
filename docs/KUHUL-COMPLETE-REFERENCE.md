# K'uhul Language Complete Reference
**Version:** 1.0 (Atomic)
**Last Updated:** November 2025
**Status:** Production Ready
**NPM Package:** `asx-language-framework`

---

## 📖 Table of Contents

1. [Core Philosophy](#core-philosophy)
2. [K'uhul Glyphs](#kuhul-glyphs)
3. [Basic Operations](#basic-operations)
4. [Data Flow](#data-flow)
5. [Control Flow](#control-flow)
6. [Complete Examples](#complete-examples)
7. [Advanced Patterns](#advanced-patterns)
8. [Integration with XJSON](#integration-with-xjson)
9. [Performance](#performance)
10. [Best Practices](#best-practices)

---

## 🎯 Core Philosophy

### The Golden Rule

```
JSON DEFINES.
ENGINE RENDERS.
ZERO FRAMEWORKS.
```

**What This Means:**
- **JSON** = All UI, routing, state, APIs defined as JSON
- **Engine** = ASX runtime (K'uhul VM) renders everything
- **Zero Frameworks** = No React, Vue, Angular, Webpack, Vite

### Why K'uhul?

| Traditional | ASX/K'uhul |
|------------|------------|
| React: 150KB | XJSON: 2KB |
| Vue: 120KB | K'uhul: 3KB |
| Webpack config: 500+ lines | ASX config: 0 lines |
| Build time: 30-60s | Build time: 0s |
| Dependencies: 100+ | Dependencies: 0 |
| npm install: 5+ min | Setup: 0s |

---

## 🔤 K'uhul Glyphs

### Basic Operations

#### `[Pop]` - Function Start

Declares a function with a name. Must end with `[Xul]`.

```kuhul
[Pop fetch_users]
  # Function body here
[Xul]
```

**Properties:**
- Declares function with name
- Must end with `[Xul]`
- Can be nested

#### `[Xul]` - Function End

Closes any `[Pop]` block.

```kuhul
[Pop my_func]
  [Sek something]
[Xul]
```

**Properties:**
- Closes any `[Pop]` block
- Required to complete function

#### `[Wo]` - Write Literal

Assign literal value to the stack.

```kuhul
[Wo "hello"]→[Ch'en greeting]
[Wo 42]→[Ch'en count]
[Wo {"name": "Alice"}]→[Ch'en data]
```

**Accepts:**
- Strings: `"hello"`
- Numbers: `42`, `3.14`
- Objects: `{"key": "value"}`
- Arrays: `[1, 2, 3]`
- Booleans: `true`, `false`

**Must pipe** `→` to `[Ch'en]` for assignment.

#### `[Yax]` - Read Variable

Read variable value from memory.

```kuhul
[Yax user]→[Sek process_user]
[Yax data]→[Sek get "items"]→[Ch'en items]
```

**Properties:**
- Reads variable value
- Can chain with `→`
- Immutable read (non-destructive)
- Pushes value to stack

#### `[Ch'en]` - Assign/Store

Store value in variable.

```kuhul
[Wo 10]→[Ch'en counter]
[Yax data]→[Sek parse]→[Ch'en parsed]
```

**Properties:**
- Store value in variable
- Always target of pipe (`→`)
- Creates or updates variable

#### `[Sek]` - Execute Function

Execute built-in or custom function.

```kuhul
[Sek http_get]              # No args
[Sek http_get "url"]        # One arg
[Sek http_get "url" "POST"] # Multiple args
[Yax data]→[Sek process]    # With piped input
```

**Properties:**
- Execute built-in or custom function
- Can accept arguments
- Can receive piped input from `[Yax]`

---

## 📊 Data Flow

### Pipe Operator `→`

Data flows **left → right**:

```kuhul
[Wo "/api/users"]→[Ch'en url]
[Yax url]→[Sek http_get]→[Ch'en response]
[Yax response]→[Sek json_parse]→[Ch'en users]
[Yax users]→[Sek render_list]
```

**Flow Breakdown:**

1. `[Wo ...]` produces value
2. `→` pipes to `[Ch'en]` (store)
3. `[Yax ...]` reads stored value
4. `→` pipes to `[Sek]` (function)
5. Result `→` `[Ch'en]` (store again)

### Stack Mechanics

K'uhul uses a **stack-based execution model**:

```kuhul
[Wo 5]           # Stack: [5]
[Wo 3]           # Stack: [5, 3]
[Sek add]        # Stack: [8]  (5+3)
[Ch'en result]   # Stack: []   (stored in variable)
```

---

## 🔄 Control Flow

### `[K'ayab']` - Loop Start

Iterator/loop block. Iterates over array.

```kuhul
[K'ayab' users_loop]
  [Yax item]→[Sek render_item]
[Kumk'u users_loop]
```

**Properties:**
- Iterator/loop block
- Iterates over array
- Must end with `[Kumk'u]` + same name

### `[Kumk'u]` - Loop End

Closes loop. Must match `[K'ayab']` name.

```kuhul
[K'ayab' process_items]
  [Sek process]
[Kumk'u process_items]
```

### `[@if]` - Conditional

Conditional execution with `@if` / `@else` / `@else-if` patterns.

```kuhul
[Pop check_auth]
  [Yax user]→[Sek is_valid]
  [@if true]
    [Sek render_dashboard]
  [@else]
    [Sek render_login]
[Xul]
```

**Properties:**
- Conditional execution
- `@if` / `@else` / `@else-if` patterns
- Evaluated at runtime

---

## 📝 Complete Examples

### Example 1: Basic Arithmetic

```kuhul
[Pop calculate]
  [Wo 5]→[Wo 3]→[Sek add]→[Ch'en sum]
  [Wo 10]→[Wo 2]→[Sek mul]→[Ch'en product]
  [Yax sum]→[Yax product]→[Sek add]→[Ch'en total]
[Xul]
```

**Result:** `sum = 8`, `product = 20`, `total = 28`

### Example 2: Fetch and Render Users

```kuhul
[Pop fetch_and_render_users]
  # Fetch data
  [Wo "http://localhost:3000/api/users"]→[Ch'en api_url]
  [Yax api_url]→[Sek http_get]→[Ch'en response]
  [Yax response]→[Sek json_parse]→[Ch'en users]

  # Render list
  [Yax users]→[Sek get "items"]→[Ch'en items]

  # Loop through items
  [K'ayab' render_loop]
    [Yax item]→[Sek render_user_card]→[Ch'en rendered]
    [Yax rendered]→[Sek append_to_dom]
  [Kumk'u render_loop]

  # Final render
  [Sek update_hud]
[Xul]

# Call the function
[Yax window]→[Sek on_boot fetch_and_render_users]
```

**In 15 lines of K'uhul:**
- Fetch from API
- Parse JSON
- Extract items
- Loop through array
- Render each item
- Update DOM
- Hook into lifecycle

**Equivalent React code:** 50+ lines

### Example 3: Form Validation

```kuhul
[Pop validate_form]
  # Get form data
  [Yax form]→[Sek get "email"]→[Ch'en email]
  [Yax form]→[Sek get "password"]→[Ch'en password]

  # Validate email
  [Yax email]→[Sek validate_email]→[Ch'en email_valid]

  # Validate password
  [Yax password]→[Sek validate_password]→[Ch'en password_valid]

  # Check both
  [Yax email_valid]→[Yax password_valid]→[Sek and]→[Ch'en form_valid]

  [@if form_valid]
    [Sek submit_form]
  [@else]
    [Sek show_error "Invalid form data"]
[Xul]
```

### Example 4: Multi-Step Workflow

```kuhul
[Pop multi_step_workflow]
  # Step 1: Validate input
  [Yax form_data]→[Sek validate]→[Ch'en validated]
  [@if validated]
    # Step 2: Fetch existing data
    [Yax id]→[Sek fetch_user]→[Ch'en user]

    # Step 3: Merge
    [Yax user]→[Yax validated]→[Sek merge]→[Ch'en merged]

    # Step 4: Save
    [Yax merged]→[Sek save_user]→[Ch'en result]

    # Step 5: Redirect
    [Sek navigate "#/success"]
  [@else]
    [Sek show_error "Validation failed"]
  [@endif]
[Xul]
```

---

## 🚀 Advanced Patterns

### Pattern 1: Async Operations

```kuhul
[Pop async_operation]
  [Sek set_loading true]
  [Yax api_url]→[Sek http_get]→[Ch'en data]
  [@if data]
    [Yax data]→[Sek save_local]→[Ch'en saved]
    [Sek set_loading false]
    [Sek notify "Data loaded"]
  [@else]
    [Sek set_loading false]
    [Sek notify "Failed to load"]
  [@endif]
[Xul]
```

### Pattern 2: Error Handling

```kuhul
[Pop safe_fetch]
  [Yax url]→[Sek http_get]→[Ch'en response]
  [@if response.error]
    [Sek show_error response.error]
  [@else]
    [Yax response]→[Sek process]
  [@endif]
[Xul]
```

### Pattern 3: State Management

```kuhul
[Pop update_state]
  # Get current state
  [Yax global_state]→[Ch'en current]

  # Update specific field
  [Yax current]→[Sek set "user" user_data]→[Ch'en updated]

  # Save back
  [Yax updated]→[Ch'en global_state]

  # Trigger re-render
  [Sek render]
[Xul]
```

### Pattern 4: Component Communication

```kuhul
[Pop parent_component]
  # Emit event to children
  [Wo {"type": "update", "data": {...}}]→[Ch'en event]
  [Yax event]→[Sek broadcast_to_children]
[Xul]

[Pop child_component]
  # Listen for parent events
  [Yax parent_event]→[Sek on_message]
  [@if message.type == "update"]
    [Yax message.data]→[Sek update_ui]
  [@endif]
[Xul]
```

---

## 🔗 Integration with XJSON

### Embedding K'uhul in XJSON

```json
{
  "xjson": "1.0",
  "page": "dashboard",
  "hud": {
    "type": "dashboard",
    "content": {
      "@html": {
        "@body": {
          "@node": "div",
          "@children": ["Loading..."]
        }
      }
    }
  },
  "kuhul": {
    "onLoad": "[Pop init] [Wo '/api/data']→[Sek fetch]→[Ch'en data] [Yax data]→[Sek render] [Xul]"
  }
}
```

### XJSON Control Flow with K'uhul

```json
{
  "@if": {
    "condition": "[Yax user]→[Sek is_logged_in]",
    "@then": {
      "@html": {
        "@body": {
          "@node": "div",
          "@children": ["Welcome!"]
        }
      }
    },
    "@else": {
      "@html": {
        "@body": {
          "@node": "div",
          "@children": ["Please log in"]
        }
      }
    }
  }
}
```

---

## ⚡ Performance

### Execution Speed

| Operation | Time (ms) |
|-----------|-----------|
| Variable assignment | < 0.01 |
| Function call | < 0.05 |
| HTTP fetch | 10-100 (network) |
| DOM update | 1-5 |
| Loop (1000 items) | 2-5 |

### Memory Usage

| Component | Size |
|-----------|------|
| K'uhul interpreter | 3 KB |
| Stack memory | Dynamic |
| Variable storage | Dynamic |
| Function cache | ~1 KB |

---

## 💡 Best Practices

### 1. Naming Conventions

```kuhul
# Good: Descriptive names
[Pop fetch_user_data]
[Ch'en user_email]
[Ch'en is_authenticated]

# Bad: Unclear names
[Pop f1]
[Ch'en x]
[Ch'en flag]
```

### 2. Error Handling

```kuhul
# Always check for errors
[Yax response]→[Sek has_error]→[Ch'en is_error]
[@if is_error]
  [Sek handle_error]
[@else]
  [Sek process_data]
[@endif]
```

### 3. Code Organization

```kuhul
# Organize by feature/domain
[Pop user_login]
[Pop user_logout]
[Pop user_register]

[Pop product_list]
[Pop product_detail]
[Pop product_search]
```

### 4. Performance Optimization

```kuhul
# Cache frequently accessed data
[Yax expensive_data]→[Sek is_cached]→[Ch'en cached]
[@if cached]
  [Yax cache]→[Ch'en result]
[@else]
  [Sek compute_expensive]→[Ch'en result]
  [Yax result]→[Ch'en cache]
[@endif]
```

### 5. Testing

```kuhul
# Write testable functions
[Pop add_numbers]
  [Yax a]→[Yax b]→[Sek add]→[Ch'en result]
  [Yax result]
[Xul]

# Test
[Pop test_add_numbers]
  [Wo 5]→[Ch'en a]
  [Wo 3]→[Ch'en b]
  [Sek add_numbers]
  [Yax result]→[Sek assert_equals 8]
[Xul]
```

---

## 📚 Built-in Functions

### HTTP Operations

- `http_get` - GET request
- `http_post` - POST request
- `http_put` - PUT request
- `http_delete` - DELETE request

### Data Operations

- `json_parse` - Parse JSON string
- `json_encode` - Encode to JSON
- `get` - Get object property
- `set` - Set object property
- `merge` - Merge objects

### DOM Operations

- `render` - Render to DOM
- `append_to_dom` - Append element
- `update_hud` - Update HUD
- `create_element` - Create DOM element

### Validation

- `validate_email` - Validate email format
- `validate_password` - Validate password
- `validate_form` - Validate form data
- `is_valid` - Generic validation

### Utility

- `navigate` - Navigate to route
- `show_error` - Show error message
- `notify` - Show notification
- `set_loading` - Set loading state

---

## 🎓 Learning Path

1. **Start Here:**
   - Read Core Philosophy
   - Learn basic glyphs (`[Pop]`, `[Wo]`, `[Ch'en]`, `[Yax]`, `[Sek]`, `[Xul]`)
   - Practice simple programs

2. **Build Foundation:**
   - Data flow with `→` operator
   - Control flow (`[@if]`, `[K'ayab']`)
   - Error handling patterns

3. **Advanced:**
   - Integration with XJSON
   - Multi-step workflows
   - State management

4. **Master:**
   - Performance optimization
   - Complex applications
   - Custom built-in functions

---

## 📖 Quick Reference

### Operators

| Operator | Function | Example |
|----------|----------|---------|
| `[Pop]` | Start function | `[Pop fetch]` |
| `[Xul]` | End function | `[Xul]` |
| `[Wo]` | Write literal | `[Wo 42]` |
| `[Yax]` | Read variable | `[Yax count]` |
| `[Ch'en]` | Assign | `[Ch'en x]` |
| `[Sek]` | Execute | `[Sek fn]` |
| `→` | Pipe | `[Yax x]→[Sek fn]` |
| `[@if]` | Conditional | `[@if condition]` |
| `[K'ayab']` | Loop start | `[K'ayab' loop]` |
| `[Kumk'u]` | Loop end | `[Kumk'u loop]` |

---

**Version:** 1.0 Atomic
**Updated:** November 2025
**Status:** Production Ready
**License:** ASX Framework
**NPM:** `asx-language-framework`

---

🚀 **Start Building Now!**

You have everything you need to create powerful applications with K'uhul. No frameworks, no build tools, just pure declarative programming.

**Welcome to the future of web development!** ⟁
