# XJSON Complete Guide
**Version:** 1.0 (Atomic)
**Last Updated:** November 2025
**Status:** Production Ready
**NPM Organization:** `@xjson/*`

---

## 📖 Table of Contents

1. [What is XJSON?](#what-is-xjson)
2. [Core Structure](#core-structure)
3. [Node Types](#node-types)
4. [Attributes & Properties](#attributes--properties)
5. [Control Flow (XCFE)](#control-flow-xcfe)
6. [Components](#components)
7. [Styling](#styling)
8. [State Management](#state-management)
9. [Routing](#routing)
10. [Complete Examples](#complete-examples)
11. [Integration with K'uhul](#integration-with-kuhul)
12. [NPM Packages](#npm-packages)

---

## 🎯 What is XJSON?

**XJSON** (eXtended JSON) is a declarative UI definition language that extends standard JSON with:

✅ **DOM nodes** - Define HTML structure in JSON
✅ **Control flow** - `@if`, `@then`, `@else`, `@while`, `@for`
✅ **Components** - Reusable UI blocks
✅ **State binding** - Reactive data flow
✅ **Routing** - Hash-based navigation
✅ **Styling** - Inline or SCX-compressed styles

### Why XJSON?

| Traditional JSX/Templates | XJSON |
|---------------------------|-------|
| Requires transpiler | Pure JSON |
| Build step needed | Zero build |
| Framework-specific | Universal |
| Runtime overhead | Native execution |
| 150KB+ bundle | 2KB payload |

---

## 🏗️ Core Structure

### Basic Node

```json
{
  "@node": "div",
  "@attrs": {
    "id": "container",
    "class": "main"
  },
  "@children": [
    "Text content",
    {
      "@node": "button",
      "@children": ["Click me"]
    }
  ]
}
```

### Complete Page

```json
{
  "xjson": "1.0",
  "page": "home",
  "title": "Home Page",
  "hud": {
    "type": "dashboard",
    "content": {
      "@html": {
        "@body": {
          "@node": "div",
          "@attrs": {"class": "container"},
          "@children": [
            {
              "@node": "h1",
              "@children": ["Welcome"]
            }
          ]
        }
      }
    }
  }
}
```

---

## 📦 Node Types

### Container Nodes

#### `div` - Generic Container

```json
{
  "@node": "div",
  "@attrs": {"class": "container"},
  "@children": [...]
}
```

#### `section` - Content Section

```json
{
  "@node": "section",
  "@attrs": {"id": "main-content"},
  "@children": [...]
}
```

### Text Nodes

#### `h1`-`h6` - Headings

```json
{
  "@node": "h1",
  "@children": ["Main Title"]
}
```

#### `p` - Paragraph

```json
{
  "@node": "p",
  "@children": ["Lorem ipsum dolor sit amet..."]
}
```

#### `span` - Inline Text

```json
{
  "@node": "span",
  "@attrs": {"class": "highlight"},
  "@children": ["Important"]
}
```

### Interactive Nodes

#### `button` - Button

```json
{
  "@node": "button",
  "@attrs": {
    "onclick": "handleClick",
    "class": "btn-primary"
  },
  "@children": ["Click Me"]
}
```

#### `input` - Form Input

```json
{
  "@node": "input",
  "@attrs": {
    "type": "text",
    "placeholder": "Enter name",
    "name": "username",
    "required": true
  }
}
```

#### `a` - Link

```json
{
  "@node": "a",
  "@attrs": {
    "href": "#/dashboard",
    "class": "nav-link"
  },
  "@children": ["Go to Dashboard"]
}
```

### List Nodes

#### `ul` / `li` - Unordered List

```json
{
  "@node": "ul",
  "@attrs": {"class": "items"},
  "@children": [
    {"@node": "li", "@children": ["Item 1"]},
    {"@node": "li", "@children": ["Item 2"]},
    {"@node": "li", "@children": ["Item 3"]}
  ]
}
```

### Form Nodes

#### `form` - Form Container

```json
{
  "@node": "form",
  "@attrs": {
    "id": "login-form",
    "onsubmit": "handleSubmit"
  },
  "@children": [
    {
      "@node": "input",
      "@attrs": {"type": "email", "name": "email"}
    },
    {
      "@node": "input",
      "@attrs": {"type": "password", "name": "password"}
    },
    {
      "@node": "button",
      "@attrs": {"type": "submit"},
      "@children": ["Login"]
    }
  ]
}
```

---

## 🎨 Attributes & Properties

### Standard HTML Attributes

All standard HTML attributes work:

```json
{
  "@node": "input",
  "@attrs": {
    "type": "email",
    "placeholder": "Enter email",
    "required": true,
    "id": "email-field",
    "class": "form-input",
    "name": "email",
    "autocomplete": "email"
  }
}
```

### Common Attributes

| Attribute | Purpose | Example |
|-----------|---------|---------|
| `id` | Unique identifier | `"id": "main-container"` |
| `class` | CSS classes | `"class": "btn btn-primary"` |
| `style` | Inline styles | `"style": {"color": "#fff"}` |
| `data-*` | Custom data | `"data-user-id": "123"` |
| `aria-*` | Accessibility | `"aria-label": "Close"` |

### Event Handlers

```json
{
  "@node": "button",
  "@attrs": {
    "onclick": "handleClick",
    "onmouseover": "handleHover",
    "onmouseout": "handleOut"
  }
}
```

### Children Types

Children can be:

1. **Strings** - Plain text
2. **Numbers** - Numeric values
3. **Booleans** - `true`/`false`
4. **Objects** - Nested XJSON nodes
5. **Arrays** - Multiple children

```json
{
  "@node": "div",
  "@children": [
    "Plain text",
    123,
    true,
    {"@node": "span", "@children": ["Nested"]},
    [
      {"@node": "p", "@children": ["Para 1"]},
      {"@node": "p", "@children": ["Para 2"]}
    ]
  ]
}
```

---

## 🔄 Control Flow (XCFE)

**XCFE** = XJSON Control Flow Engine

### `@if` / `@then` / `@else`

```json
{
  "@if": {
    "condition": "user.isLoggedIn",
    "@then": {
      "@html": {
        "@body": {
          "@node": "div",
          "@children": ["Welcome back!"]
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

### Comparison Operators

```json
{
  "@if": {
    "condition": {
      "op": ">",
      "left": "${user.age}",
      "right": 18
    },
    "@then": {...}
  }
}
```

**Supported operators:**
- `==` - Equal
- `!=` - Not equal
- `>` - Greater than
- `<` - Less than
- `>=` - Greater than or equal
- `<=` - Less than or equal
- `&&` - Logical AND
- `||` - Logical OR

### `@for` Loop

```json
{
  "@for": {
    "var": "item",
    "in": "items",
    "@do": {
      "@node": "div",
      "@attrs": {"class": "item"},
      "@children": ["${item.name}"]
    }
  }
}
```

### `@while` Loop

```json
{
  "@while": {
    "condition": {
      "op": "<",
      "left": "${counter}",
      "right": 10
    },
    "@do": {
      "@node": "div",
      "@children": ["Count: ${counter}"]
    }
  }
}
```

---

## 🧩 Components

### Component Definition

```json
{
  "@component": {
    "name": "MetricCard",
    "props": [
      {"name": "title", "type": "string"},
      {"name": "value", "type": "string"},
      {"name": "icon", "type": "string"}
    ],
    "template": {
      "@html": {
        "@node": "div",
        "@attrs": {"class": "metric-card"},
        "@children": [
          {
            "@node": "span",
            "@attrs": {"class": "icon"},
            "@children": ["{{icon}}"]
          },
          {
            "@node": "h3",
            "@children": ["{{title}}"]
          },
          {
            "@node": "p",
            "@attrs": {"class": "value"},
            "@children": ["{{value}}"]
          }
        ]
      }
    }
  }
}
```

### Component Instance

```json
{
  "@component-instance": {
    "component": "MetricCard",
    "props": {
      "title": "Active Users",
      "value": "1,234",
      "icon": "👥"
    }
  }
}
```

### Nested Components

```json
{
  "@node": "div",
  "@attrs": {"class": "dashboard"},
  "@children": [
    {
      "@component-instance": {
        "component": "Header",
        "props": {"title": "Dashboard"}
      }
    },
    {
      "@component-instance": {
        "component": "MetricCard",
        "props": {"title": "Users", "value": "1,234"}
      }
    },
    {
      "@component-instance": {
        "component": "Footer",
        "props": {}
      }
    }
  ]
}
```

---

## 🎨 Styling

### Inline Styles

```json
{
  "@node": "div",
  "@attrs": {
    "style": {
      "backgroundColor": "#050814",
      "color": "#16f2aa",
      "padding": "20px",
      "borderRadius": "10px",
      "boxShadow": "0 4px 12px rgba(0,0,0,0.3)"
    }
  }
}
```

### CSS Classes

```json
{
  "@node": "div",
  "@attrs": {
    "class": "container flex items-center justify-between"
  }
}
```

### SCX Compressed Styles

```json
{
  "@node": "div",
  "@attrs": {
    "⟁s": "⟁bg1 ⟁text1 ⟁p20 ⟁br10"
  }
}
```

Maps to:
```json
{
  "⟁bg1": {"backgroundColor": "#050814"},
  "⟁text1": {"color": "#16f2aa"},
  "⟁p20": {"padding": "20px"},
  "⟁br10": {"borderRadius": "10px"}
}
```

---

## 📊 State Management

### State Definition

```json
{
  "@state": {
    "initial": {
      "user": null,
      "authenticated": false,
      "items": [],
      "loading": false
    },
    "actions": [
      {
        "name": "login",
        "payload": {"email": "string", "password": "string"},
        "handler": "[Yax payload]→[Sek authenticate]→[Ch'en result]"
      },
      {
        "name": "loadItems",
        "handler": "[Yax items]→[Sek fetch '/api/items']→[Ch'en items]"
      }
    ],
    "subscriptions": [
      {
        "path": "items",
        "onChange": "renderItems"
      },
      {
        "path": "authenticated",
        "onChange": "updateUI"
      }
    ]
  }
}
```

### State Binding

```json
{
  "@node": "div",
  "@children": [
    {
      "@node": "h1",
      "@children": ["Welcome, ${user.name}!"]
    },
    {
      "@node": "p",
      "@children": ["You have ${items.length} items"]
    }
  ]
}
```

### Variable Substitution

Use `${varName}` syntax:

```json
{
  "@node": "div",
  "@attrs": {
    "id": "${containerId}",
    "class": "container ${theme}"
  },
  "@children": ["Hello, ${user.name}!"]
}
```

---

## 🗺️ Routing

### Route Definition

```json
{
  "routes": {
    "/": "home",
    "/dashboard": "dashboard",
    "/settings": "settings",
    "/users/:id": "userDetail"
  },
  "pages": {
    "home": {...},
    "dashboard": {...},
    "settings": {...},
    "userDetail": {...}
  }
}
```

### Navigation

```json
{
  "@node": "a",
  "@attrs": {
    "href": "#/dashboard"
  },
  "@children": ["Go to Dashboard"]
}
```

Or programmatically:

```json
{
  "@node": "button",
  "@attrs": {
    "onclick": "navigate('#/settings')"
  },
  "@children": ["Settings"]
}
```

### Route Parameters

```json
{
  "routes": {
    "/users/:userId": "userDetail",
    "/posts/:postId/comments/:commentId": "commentDetail"
  }
}
```

Access in K'uhul:

```kuhul
[Pop userDetail]
  [Yax route.params.userId]→[Ch'en userId]
  [Yax userId]→[Sek fetch_user]→[Ch'en user]
[Xul]
```

---

## 📝 Complete Examples

### Example 1: Simple Page

```json
{
  "xjson": "1.0",
  "page": "home",
  "title": "Home Page",
  "hud": {
    "type": "dashboard",
    "content": {
      "@html": {
        "@body": {
          "@node": "div",
          "@attrs": {"class": "container"},
          "@children": [
            {
              "@node": "h1",
              "@children": ["Welcome to ASX TAPE ARCADE"]
            },
            {
              "@node": "p",
              "@children": ["The future of web development."]
            },
            {
              "@node": "button",
              "@attrs": {
                "onclick": "navigate('#/dashboard')",
                "class": "btn-primary"
              },
              "@children": ["Get Started"]
            }
          ]
        }
      }
    }
  }
}
```

### Example 2: Form with Validation

```json
{
  "xjson": "1.0",
  "page": "register",
  "title": "Register",
  "hud": {
    "@html": {
      "@body": {
        "@node": "form",
        "@attrs": {"id": "register-form"},
        "@children": [
          {
            "@node": "h2",
            "@children": ["Create Account"]
          },
          {
            "@node": "input",
            "@attrs": {
              "type": "email",
              "name": "email",
              "placeholder": "Email",
              "required": true
            }
          },
          {
            "@node": "input",
            "@attrs": {
              "type": "password",
              "name": "password",
              "placeholder": "Password",
              "required": true
            }
          },
          {
            "@node": "button",
            "@attrs": {"type": "submit"},
            "@children": ["Register"]
          }
        ]
      }
    }
  },
  "asx": {
    "inline": "document.getElementById('register-form').onsubmit=(e)=>{e.preventDefault();const data=new FormData(e.target);fetch('/api/register',{method:'POST',body:JSON.stringify(Object.fromEntries(data))}).then(r=>r.json()).then(d=>{if(d.success){window.location='#/login'}else{alert(d.error)}})}"
  }
}
```

### Example 3: Dashboard with Components

```json
{
  "xjson": "1.0",
  "page": "dashboard",
  "title": "Dashboard",
  "components": [
    {
      "@component": {
        "name": "MetricCard",
        "props": [
          {"name": "title", "type": "string"},
          {"name": "value", "type": "string"},
          {"name": "icon", "type": "string"}
        ],
        "template": {
          "@node": "div",
          "@attrs": {"class": "metric-card"},
          "@children": [
            {"@node": "span", "@children": ["{{icon}}"]},
            {"@node": "h3", "@children": ["{{title}}"]},
            {"@node": "p", "@children": ["{{value}}"]}
          ]
        }
      }
    }
  ],
  "hud": {
    "@html": {
      "@body": {
        "@node": "div",
        "@attrs": {"class": "dashboard"},
        "@children": [
          {
            "@node": "h1",
            "@children": ["Dashboard"]
          },
          {
            "@node": "div",
            "@attrs": {"class": "metrics"},
            "@children": [
              {
                "@component-instance": {
                  "component": "MetricCard",
                  "props": {"title": "Users", "value": "1,234", "icon": "👥"}
                }
              },
              {
                "@component-instance": {
                  "component": "MetricCard",
                  "props": {"title": "Revenue", "value": "$12.5K", "icon": "💰"}
                }
              },
              {
                "@component-instance": {
                  "component": "MetricCard",
                  "props": {"title": "Orders", "value": "567", "icon": "📦"}
                }
              }
            ]
          }
        ]
      }
    }
  }
}
```

### Example 4: Conditional Rendering

```json
{
  "xjson": "1.0",
  "page": "profile",
  "@state": {
    "initial": {
      "user": null,
      "loading": true
    }
  },
  "hud": {
    "@html": {
      "@body": {
        "@if": {
          "condition": "${loading}",
          "@then": {
            "@node": "div",
            "@children": ["Loading..."]
          },
          "@else": {
            "@if": {
              "condition": "${user}",
              "@then": {
                "@node": "div",
                "@children": [
                  {
                    "@node": "h1",
                    "@children": ["Welcome, ${user.name}!"]
                  },
                  {
                    "@node": "p",
                    "@children": ["Email: ${user.email}"]
                  }
                ]
              },
              "@else": {
                "@node": "div",
                "@children": ["Please log in"]
              }
            }
          }
        }
      }
    }
  }
}
```

---

## 🔗 Integration with K'uhul

### K'uhul Handlers

```json
{
  "xjson": "1.0",
  "page": "data-page",
  "kuhul": {
    "onLoad": "[Pop init] [Wo '/api/data']→[Sek fetch]→[Ch'en data] [Yax data]→[Sek render] [Xul]",
    "handlers": {
      "handleClick": "[Pop onClick] [Sek alert 'Clicked!'] [Xul]",
      "handleSubmit": "[Pop onSubmit] [Yax form]→[Sek validate]→[Sek submit] [Xul]"
    }
  }
}
```

### Lifecycle Hooks

```json
{
  "lifecycle": {
    "onMount": "[Pop mounted] [Sek init_app] [Xul]",
    "onUpdate": "[Pop updated] [Sek refresh_data] [Xul]",
    "onUnmount": "[Pop cleanup] [Sek destroy] [Xul]"
  }
}
```

---

## 📦 NPM Packages

### Available Packages

Install from NPM:

```bash
npm install @xjson/core
npm install @xjson/runtime
npm install @xjson/parser
npm install @xjson/validator
```

### Usage

```javascript
import { parse, render } from '@xjson/core';

const xjson = {
  "xjson": "1.0",
  "page": "demo",
  "hud": { /* ... */ }
};

const dom = render(xjson);
document.body.appendChild(dom);
```

---

## 🎓 Best Practices

### 1. Keep Structure Flat

```json
// Good
{
  "@node": "div",
  "@children": [
    {"@component-instance": {"component": "Header"}},
    {"@component-instance": {"component": "Content"}},
    {"@component-instance": {"component": "Footer"}}
  ]
}

// Bad (too nested)
{
  "@node": "div",
  "@children": [{
    "@node": "div",
    "@children": [{
      "@node": "div",
      "@children": [...]
    }]
  }]
}
```

### 2. Use Components for Reusability

```json
// Define once
{"@component": {"name": "Button", ...}}

// Use many times
{"@component-instance": {"component": "Button", "props": {...}}}
{"@component-instance": {"component": "Button", "props": {...}}}
{"@component-instance": {"component": "Button", "props": {...}}}
```

### 3. Separate Concerns

```json
{
  "structure": { /* XJSON */ },
  "state": { /* State management */ },
  "logic": { /* K'uhul handlers */ },
  "styles": { /* SCX styles */ }
}
```

---

**Version:** 1.0 Atomic
**Updated:** November 2025
**Status:** Production Ready
**NPM:** `@xjson/*`

---

🚀 **Start Building with XJSON!**

Pure JSON. Zero frameworks. Infinite possibilities.
