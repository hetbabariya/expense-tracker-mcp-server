# ExpenseTracker MCP Server

An MCP (Model Context Protocol) server for tracking personal expenses with a SQLite backend.

## Overview

ExpenseTracker is a FastMCP-based server that provides tools for managing expenses through an MCP-compatible client (like Claude Desktop or other AI assistants). It stores expenses in a local SQLite database and provides categorized expense tracking.

## Features

- **Add Expenses**: Record expenses with date, amount, category, subcategory, and notes
- **List Expenses**: Query expenses by date range
- **Summarize Expenses**: Get spending summaries by category within a date range
- **Categories Resource**: Access predefined expense categories and subcategories

## Installation

### Prerequisites

- Python 3.10+
- [uv](https://github.com/astral-sh/uv) (recommended) or pip

### Setup

1. Clone the repository:
```bash
git clone <repository-url>
cd ExpenseTracker
```

2. Create a virtual environment and install dependencies:
```bash
# Using uv
uv venv
uv pip install -e .

# Using pip
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate
pip install -e .
```

## Deployment

This MCP server is deployed on **FastMCP Cloud**:

**URL:** https://efficient-purple-snipe.fastmcp.app/mcp

## Usage

### Running the Server

```bash
python main.py
```

The server runs on `http://0.0.0.0:8000` by default.

### MCP Tools

#### `add_expense`

Add a new expense entry.

**Parameters:**
- `date` (str): Date in YYYY-MM-DD format
- `amount` (float): Expense amount
- `category` (str): Expense category
- `subcategory` (str, optional): Subcategory
- `note` (str, optional): Additional notes

**Example:**
```json
{
  "date": "2024-03-15",
  "amount": 45.50,
  "category": "Food & Dining",
  "subcategory": "dining_out",
  "note": "Dinner with friends"
}
```

#### `list_expenses`

List expenses within a date range.

**Parameters:**
- `start_date` (str): Start date (YYYY-MM-DD)
- `end_date` (str): End date (YYYY-MM-DD)

**Returns:** Array of expense records with `id`, `date`, `amount`, `category`, `subcategory`, and `note`.

#### `summarize`

Get spending summary by category.

**Parameters:**
- `start_date` (str): Start date (YYYY-MM-DD)
- `end_date` (str): End date (YYYY-MM-DD)
- `category` (str, optional): Filter by specific category

**Returns:** Array of summary objects with `category`, `total_amount`, and `count`.

### MCP Resources

#### `expense:///categories`

Returns all available expense categories and subcategories as JSON.

**Categories include:**
- food (groceries, dining_out, etc.)
- transport (fuel, public_transport, etc.)
- housing (rent, maintenance, etc.)
- utilities (electricity, internet, etc.)
- health (medicines, doctor_consultation, etc.)
- education (books, courses, etc.)
- shopping (clothing, electronics, etc.)
- entertainment (movies, streaming, etc.)
- travel (flights, hotels, etc.)
- business (software_tools, hosting, etc.)
- And more...

## Database

Expenses are stored in a SQLite database located at the system's temp directory (`expenses.db`).

**Schema:**
```sql
CREATE TABLE expenses (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    date TEXT NOT NULL,
    amount REAL NOT NULL,
    category TEXT NOT NULL,
    subcategory TEXT DEFAULT '',
    note TEXT DEFAULT ''
);
```

## Project Structure

```
ExpenseTracker/
├── main.py              # Main MCP server implementation
├── categories.json      # Expense categories and subcategories
├── pyproject.toml       # Project configuration
├── uv.lock             # Dependency lock file
├── README.md           # This file
└── .venv/              # Virtual environment
```

## Dependencies

- `fastmcp>=2.14.4` - FastMCP framework for building MCP servers
- `aiosqlite>=0.22.1` - Async SQLite interface
