# DAY23 Drill — The Food Truck Web App

## Task

Build a Flask app (`app.py`) with multiple routes and templates.

### Structure

```
drill/
├── app.py
└── templates/
    ├── base.html
    ├── home.html
    ├── menu.html
    └── order.html
```

### Requirements

1. **`/`** — Home page with a welcome message and a link to the menu
2. **`/menu`** — Display a hardcoded list of menu items (use the list below)
3. **`/order/<item>`** — Show an order confirmation page for the given item
4. **`/about`** — About page with the food truck story
5. **Template inheritance** — `base.html` with a common layout (nav, header, footer)

### Menu data (hardcoded in `app.py`)

```python
menu_items = [
    {"name": "Classic Burger", "price": 8.50, "description": "Beef patty with lettuce, tomato, and special sauce"},
    {"name": "Chicken Wrap", "price": 7.00, "description": "Grilled chicken with veggies in a tortilla"},
    {"name": "Veggie Bowl", "price": 6.50, "description": "Quinoa, black beans, avocado, and salsa"},
    {"name": "Fish Tacos", "price": 9.00, "description": "Battered fish with cabbage slaw and lime"},
    {"name": "Fries", "price": 3.50, "description": "Crispy golden fries with sea salt"},
]
```

### Expected pages

- Home: `"Welcome to Python Food Truck! Fresh code, daily."` with links
- Menu: list all items with name, price, description, and an "Order" link
- Order: `"You ordered a {item}! That will be ${price}. Ready in 5 minutes."`
- About: paragraph about the food truck

### Hints

- Use `render_template()` for all pages
- `base.html` contains the `<html>`, `<head>`, `<body>` structure with `{% block content %}{% endblock %}`
- `url_for('route_name')` generates URLs in templates
- For `/order/<item>`, find the item in `menu_items` by name (case-insensitive)
- Handle unknown items with a 404 or custom message

### How to run

```bash
python3 app.py
# Open http://127.0.0.1:5000
```

## TODOs

- [ ] Create `templates/base.html` with common layout
- [ ] Create `templates/home.html`
- [ ] Create `templates/menu.html` with a loop
- [ ] Create `templates/order.html` with item details
- [ ] Implement routes in `app.py`
- [ ] Run and verify all pages
