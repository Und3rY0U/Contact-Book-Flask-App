# Contact Book Flask App

A web-based contact management system built with Python and Flask. Users can store and manage their personal contacts, including details such as full name, phone number, email address, and home address.

> **Base project by [RF-Fahad-Islam](https://github.com/RF-Fahad-Islam/Contact-Book-Flask-App)**

---

## Added Feature — Contact REST API

This forked version introduces a brand new REST API layer (`contact_api.py`) that enables full **CRUD functionality** — Create, Read, Update, and Delete — through JSON-based HTTP requests.

### Purpose
The base application can only be used through a web browser. By adding a REST API, the contact data becomes accessible to any HTTP client — such as mobile applications, third-party services, or automated scripts — without going through the browser interface at all.

---

## Available API Endpoints

| HTTP Method | Route | Action | Response Code |
|-------------|-------|--------|---------------|
| GET | `/api/contacts` | Retrieve all contacts | 200 |
| GET | `/api/contacts/<id>` | Retrieve one contact by ID | 200 |
| POST | `/api/contacts` | Add a new contact | 201 |
| PUT | `/api/contacts/<id>` | Modify an existing contact | 200 |
| DELETE | `/api/contacts/<id>` | Remove a contact | 200 |

All requests and responses are formatted in **JSON**.

---

## Sample API Usage

### Adding a New Contact
**POST** `/api/contacts`

Request body:
```json
{
  "userid": "abc123",
  "name": "Juan dela Cruz",
  "tel": "09171234567",
  "email": "juan@example.com",
  "address": "Manila, PH",
  "color": "blue"
}
```

Successful response (201):
```json
{
  "status": 201,
  "message": "Contact created successfully.",
  "data": {
    "sno": 1,
    "userid": "abc123",
    "name": "Juan dela Cruz",
    "tel": "09171234567",
    "email": "juan@example.com",
    "address": "Manila, PH",
    "color": "blue",
    "date": null
  }
}
```

### Retrieving All Contacts
**GET** `/api/contacts`

Successful response (200):
```json
{
  "status": 200,
  "message": "Contacts retrieved successfully.",
  "data": [...]
}
```

### Updating a Contact
**PUT** `/api/contacts/1`

Request body (only include fields you want to change):
```json
{
  "name": "Juan Updated",
  "address": "Quezon City, PH"
}
```

### Removing a Contact
**DELETE** `/api/contacts/1`

Successful response (200):
```json
{
  "status": 200,
  "message": "Contact with id 1 deleted successfully."
}
```

### Error Codes

| Code | Reason |
|------|--------|
| 400 | Invalid request — missing required fields, duplicate name or phone number |
| 404 | No contact found with the given ID |

---

## Installation & Setup

### Step 1 — Install dependencies

```bash
pip install flask flask-sqlalchemy flask-mail pytest pytest-cov
```

### Step 2 — Configure the database

Open `config.json` and update it with your settings:

```json
{
  "params": {
    "local_server": true,
    "local_server_uri": "sqlite:///contacts.db",
    "pd_uri": "",
    "GMAIL_USER": "",
    "GMAIL_PASSWORD": ""
  }
}
```

> Using `sqlite:///contacts.db` is recommended for local development — no database server required.

### Step 3 — Connect the API to the app

In `app.py`, add the following lines after the `Book1` model definition:

```python
from contact_api import contact_api, init_api
app.register_blueprint(contact_api)
init_api(db, Book1)
```

### Step 4 — Start the application

```bash
python app.py
```

Then open your browser and go to `http://127.0.0.1:5000`.

---

## Running the Tests

The test suite covers all five API endpoints with both passing and failing scenarios.

### Run with coverage report

```bash
pytest test_contact_api.py -v --cov=contact_api --cov-report=term-missing
```

Expected result: **20 tests passed — 100% code coverage**

---

## Project Structure

```
Contact-Book-Flask-App/
├── app.py               # Original Flask application (web UI)
├── contact_api.py       # NEW — REST API Blueprint (full CRUD)
├── test_contact_api.py  # NEW — pytest test suite (100% coverage)
├── config.json          # Configuration file (database, mail settings)
├── templates/           # HTML template files (original)
├── static/              # CSS and JS assets (original)
└── README.md            # Project documentation
```

---

## Original App Features

- User registration and login system
- Add new contacts with name, phone, email, and address
- Edit existing contact details
- Delete a single contact or all contacts at once
- View contacts in a table layout
- Export contacts as an HTML table

---

## Technologies Used

- **Python 3.9+**
- **Flask** — lightweight web framework
- **Flask-SQLAlchemy** — database ORM
- **SQLite** — local database (default)
- **pytest + pytest-cov** — testing and coverage reporting
