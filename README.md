# futuristic-shop-management-system
Shop Management System built using Python, CustomTkinter, and SQLite.

## About the Project

The application provides a menu-based interface where users can select food items from different categories. The selected items are added to the bill automatically, and the system calculates the total amount along with GST. Bills can be saved as PDF files and printed directly from the application.

This project helped me understand GUI development, event handling, file operations, and PDF generation using Python.

## Features

- Categorized food menu
- Add multiple items to an order
- Automatic quantity management
- Live bill preview
- GST calculation
- Generate customer bills
- Save bills as PDF
- Print receipts
- Real-time date and time display
- Clear current order

## Menu Categories

- South Indian
- Dosa Grill
- Uthappam
- Meals
- Chinese Appetizers
- Chinese Wok
- Curry Pot
- Freshly Tandoor Bread

## Technologies Used

- Python
- Tkinter
- ReportLab
- Datetime Module
- OS Module

## Project Structure

```text
restaurant-pos-billing-system/
│
├── Restaurant_pos.py
├── README.md
├── requirements.txt
└── .gitignore
```

## Installation

Clone the repository:

```bash
git clone https://github.com/sachu45/restaurant-pos-billing-system.git
```

Move into the project folder:

```bash
cd restaurant-pos-billing-system
```

Install required package:

```bash
pip install reportlab
```

Run the application:

```bash
python Restaurant_pos.py
```

## How It Works

1. Open the application.
2. Select a food category.
3. Click on menu items to add them to the order.
4. Review the bill in the receipt panel.
5. Click **Generate Bill**.
6. The application calculates GST and total amount.
7. A PDF invoice is created automatically.
8. Print the bill if required.

## Future Improvements

Some features that can be added in future versions:

- Customer database
- Admin login system
- Inventory management
- Sales reports
- SQLite database integration
- QR code payments
- Online order support

## What I Learned

While building this project, I learned:

- GUI development with Tkinter
- Python event handling
- Working with dictionaries and data structures
- Generating PDFs using ReportLab
- File handling and printing operations
- Organizing a desktop application

## Author

**Sarrvesh J R**

GitHub: https://github.com/sachu45

LinkedIn: https://www.linkedin.com/in/sarrvesh-jr

## License

This project is available for learning and educational purposes.
