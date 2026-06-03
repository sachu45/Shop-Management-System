# Futuristic Shop Management System using CustomTkinter
# Install dependencies: pip install customtkinter tabulate
import sqlite3 as db
import datetime
from tabulate import tabulate 
import customtkinter as ctk 
from tkinter import messagebox

# Initialize DB

def initDB():
    connectObj = db.connect("shopMgmt.db")
    cursor = connectObj.cursor()
    cursor.execute('''CREATE TABLE IF NOT EXISTS users(userType NUMBER, userName STRING PRIMARY KEY, password STRING)''')
    cursor.execute('''CREATE TABLE IF NOT EXISTS products(productId NUMBER PRIMARY KEY, productName STRING, quantity NUMBER, price FLOAT)''')
    cursor.execute('''CREATE TABLE IF NOT EXISTS purchaseHistory(srno INTEGER PRIMARY KEY AUTOINCREMENT, customerName STRING, productIdsPurchased STRING, amountPaid FLOAT, date STRING, customerPhone NUMBER(10))''')
    cursor.execute('''CREATE TABLE IF NOT EXISTS employees(empId INTEGER PRIMARY KEY AUTOINCREMENT, empName STRING, empJob STRING, empJoinDate STRING)''')
    cursor.execute("SELECT * FROM users WHERE userName = 'shopadmin'")
    if len(cursor.fetchall()) == 0:
        cursor.execute("INSERT INTO users VALUES(0, 'shopadmin', 'admin@1234')")
    cursor.execute("SELECT * FROM users WHERE userName = 'cashier'")
    if len(cursor.fetchall()) == 0:
        cursor.execute("INSERT INTO users VALUES(1, 'cashier', 'cash@1234')")
    connectObj.commit()
    connectObj.close()

# Helper functions

def getInt(entry):
    try:
        return int(entry)
    except:
        return None

def getPhone(entry):
    try:
        ph = int(entry)
        if len(str(ph)) == 10:
            return ph
        else:
            return None
    except:
        return None

# Main App Class

class ShopApp(ctk.CTk):
    def __init__(self):
        super().__init__()
        self.title("Futuristic Shop Management System")
        self.geometry("700x500")
        ctk.set_appearance_mode("dark")
        ctk.set_default_color_theme("blue")
        self.resizable(False, False)
        self.show_main_menu()

    def clear(self):
        for widget in self.winfo_children():
            widget.destroy()

    def show_main_menu(self):
        self.clear()
        ctk.CTkLabel(self, text="Shop Management System", font=("Orbitron", 32, "bold"), text_color="#00ffe7").pack(pady=30)
        ctk.CTkLabel(self, text="Select login type:", font=("Orbitron", 18)).pack(pady=10)
        ctk.CTkButton(self, text="Admin Login", command=self.admin_login, width=200, height=40, font=("Orbitron", 16)).pack(pady=10)
        ctk.CTkButton(self, text="Cashier Login", command=self.cashier_login, width=200, height=40, font=("Orbitron", 16)).pack(pady=10)
        ctk.CTkButton(self, text="Exit", command=self.destroy, fg_color="#ff0055", width=200, height=40, font=("Orbitron", 16)).pack(pady=30)

    def admin_login(self):
        self.clear()
        ctk.CTkLabel(self, text="Admin Login", font=("Orbitron", 28, "bold"), text_color="#00ffe7").pack(pady=30)
        user_entry = ctk.CTkEntry(self, placeholder_text="User ID", width=250)
        user_entry.pack(pady=10)
        pass_entry = ctk.CTkEntry(self, placeholder_text="Password", show="*", width=250)
        pass_entry.pack(pady=10)
        def try_login():
            user = user_entry.get().strip()
            pwd = pass_entry.get().strip()
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute("SELECT userName, password FROM users WHERE userType = 0")
            row = cursor.fetchone()
            connectObj.close()
            if row and user == row[0] and pwd == row[1]:
                self.show_admin_menu(user)
            else:
                messagebox.showerror("Login Failed", "Invalid admin credentials!")
        ctk.CTkButton(self, text="Login", command=try_login, width=200, height=40).pack(pady=20)
        ctk.CTkButton(self, text="Back", command=self.show_main_menu, width=200, height=40, fg_color="#444").pack(pady=5)

    def cashier_login(self):
        self.clear()
        ctk.CTkLabel(self, text="Cashier Login", font=("Orbitron", 28, "bold"), text_color="#00ffe7").pack(pady=30)
        user_entry = ctk.CTkEntry(self, placeholder_text="User ID", width=250)
        user_entry.pack(pady=10)
        pass_entry = ctk.CTkEntry(self, placeholder_text="Password", show="*", width=250)
        pass_entry.pack(pady=10)
        def try_login():
            user = user_entry.get().strip()
            pwd = pass_entry.get().strip()
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute("SELECT userName, password FROM users WHERE userType = 1")
            row = cursor.fetchone()
            connectObj.close()
            if row and user == row[0] and pwd == row[1]:
                self.show_cashier_menu(user)
            else:
                messagebox.showerror("Login Failed", "Invalid cashier credentials!")
        ctk.CTkButton(self, text="Login", command=try_login, width=200, height=40).pack(pady=20)
        ctk.CTkButton(self, text="Back", command=self.show_main_menu, width=200, height=40, fg_color="#444").pack(pady=5)

    def show_admin_menu(self, user):
        self.clear()
        ctk.CTkLabel(self, text=f"Welcome, {user}", font=("Orbitron", 22, "bold"), text_color="#00ffe7").pack(pady=10)
        ctk.CTkLabel(self, text="Admin Menu", font=("Orbitron", 26, "bold"), text_color="#00ffe7").pack(pady=10)
        btns = [
            ("View all sales", self.admin_view_sales),
            ("View available products", self.admin_view_products),
            ("Add new product", self.admin_add_product),
            ("Update product price", self.admin_update_price),
            ("View employees", self.admin_view_employees),
            ("Add employee", self.admin_add_employee),
            ("Change password", lambda: self.change_password(0, user)),
            ("Logout", self.show_main_menu)
        ]
        for text, cmd in btns:
            ctk.CTkButton(self, text=text, command=cmd, width=300, height=40, font=("Orbitron", 16)).pack(pady=7)

    def show_cashier_menu(self, user):
        self.clear()
        self.state('zoomed')  # Full screen
        ctk.CTkLabel(self, text=f"Welcome, {user}", font=("Orbitron", 22, "bold"), text_color="#00ffe7").pack(pady=10)
        ctk.CTkLabel(self, text="Cashier Menu", font=("Orbitron", 26, "bold"), text_color="#00ffe7").pack(pady=10)
        btns = [
            ("View available products", self.cashier_view_products),
            ("Generate / Store Bill", self.cashier_generate_bill),
            ("Add Customer", self.cashier_add_customer),
            ("Change password", lambda: self.change_password(1, user)),
            ("Logout", self.show_main_menu)
        ]
        for text, cmd in btns:
            ctk.CTkButton(self, text=text, command=cmd, width=300, height=40, font=("Orbitron", 16)).pack(pady=7)

    def cashier_add_customer(self):
        self.clear()
        self.state('zoomed')
        ctk.CTkLabel(self, text="Add Customer", font=("Orbitron", 28, "bold"), text_color="#00ffe7").pack(pady=20)
        outer_frame = ctk.CTkFrame(self, fg_color="#222831")
        outer_frame.pack(expand=True, fill="both", padx=0, pady=0)
        frame = ctk.CTkFrame(outer_frame, fg_color="#222831")
        frame.place(relx=0.5, rely=0.5, anchor="center")
        label_font = ("Orbitron", 18)
        entry_width = 350
        ctk.CTkLabel(frame, text="Phone Number:", font=label_font, text_color="#00ffe7").grid(row=0, column=0, padx=15, pady=15, sticky="e")
        phone_entry = ctk.CTkEntry(frame, width=entry_width, font=("Consolas", 16))
        phone_entry.grid(row=0, column=1, padx=15, pady=15)
        ctk.CTkLabel(frame, text="Customer Name:", font=label_font, text_color="#00ffe7").grid(row=1, column=0, padx=15, pady=15, sticky="e")
        name_entry = ctk.CTkEntry(frame, width=entry_width, font=("Consolas", 16))
        name_entry.grid(row=1, column=1, padx=15, pady=15)
        ctk.CTkLabel(frame, text="Address:", font=label_font, text_color="#00ffe7").grid(row=2, column=0, padx=15, pady=15, sticky="e")
        address_entry = ctk.CTkEntry(frame, width=entry_width, font=("Consolas", 16))
        address_entry.grid(row=2, column=1, padx=15, pady=15)
        def save_customer():
            phone = phone_entry.get().strip()
            name = name_entry.get().strip()
            address = address_entry.get().strip()
            if not phone or not name:
                messagebox.showerror("Error", "Phone and Name required!")
                return
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute('''CREATE TABLE IF NOT EXISTS customers(phone TEXT PRIMARY KEY, name TEXT, address TEXT)''')
            cursor.execute("INSERT OR REPLACE INTO customers(phone, name, address) VALUES (?, ?, ?)", (phone, name, address))
            connectObj.commit()
            connectObj.close()
            messagebox.showinfo("Success", "Customer added/updated!")
            self.show_cashier_menu('cashier')
        ctk.CTkButton(frame, text="Save Customer", command=save_customer, width=200, height=40, font=("Orbitron", 16), fg_color="green").grid(row=3, column=0, columnspan=2, pady=20)
        ctk.CTkButton(frame, text="Back", command=lambda: self.show_cashier_menu('cashier'), width=200, height=40, font=("Orbitron", 16), fg_color="#444").grid(row=4, column=0, columnspan=2, pady=5)

    # Admin Functions
    def admin_view_sales(self):
        self.clear()
        ctk.CTkLabel(self, text="All Sales", font=("Orbitron", 24, "bold"), text_color="#00ffe7").pack(pady=10)
        connectObj = db.connect("shopMgmt.db")
        cursor = connectObj.cursor()
        cursor.execute("SELECT * FROM purchaseHistory")
        sales = cursor.fetchall()
        connectObj.close()
        text = tabulate(sales, headers=['Sr No.', 'Customer Name', 'Products', 'Amount Paid', 'Date', 'Phone'], tablefmt="fancy_grid") if sales else "No sales found."
        ctk.CTkTextbox(self, width=650, height=300, font=("Consolas", 14)).pack(pady=10)
        self.children[list(self.children)[-1]].insert("0.0", text) # type: ignore
        ctk.CTkButton(self, text="Back", command=lambda: self.show_admin_menu('shopadmin'), width=200, height=40).pack(pady=10)

    def admin_view_products(self):
        self.clear()
        ctk.CTkLabel(self, text="Available Products", font=("Orbitron", 24, "bold"), text_color="#00ffe7").pack(pady=10)
        connectObj = db.connect("shopMgmt.db")
        cursor = connectObj.cursor()
        cursor.execute("SELECT * FROM products")
        products = cursor.fetchall()
        connectObj.close()
        text = tabulate(products, headers=['Product Id', 'Product Name', 'Quantity', 'Price'], tablefmt="fancy_grid") if products else "No products found."
        ctk.CTkTextbox(self, width=650, height=300, font=("Consolas", 14)).pack(pady=10)
        self.children[list(self.children)[-1]].insert("0.0", text) # type: ignore
        ctk.CTkButton(self, text="Back", command=lambda: self.show_admin_menu('shopadmin'), width=200, height=40).pack(pady=10)

    def admin_add_product(self):
        self.clear()
        ctk.CTkLabel(self, text="Add New Product", font=("Orbitron", 24, "bold"), text_color="#00ffe7").pack(pady=10)
        prdId = ctk.CTkEntry(self, placeholder_text="Product Id", width=250)
        prdId.pack(pady=5)
        prdName = ctk.CTkEntry(self, placeholder_text="Product Name", width=250)
        prdName.pack(pady=5)
        quantity = ctk.CTkEntry(self, placeholder_text="Quantity", width=250)
        quantity.pack(pady=5)
        prdPrice = ctk.CTkEntry(self, placeholder_text="Price", width=250)
        prdPrice.pack(pady=5)
        def add():
            pid = getInt(prdId.get())
            q = getInt(quantity.get())
            price = prdPrice.get()
            if not pid or not prdName.get() or not q or not price:
                messagebox.showerror("Error", "All fields required and must be valid!")
                return
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute("SELECT * FROM products WHERE productId = ?", [pid])
            if len(cursor.fetchall()) == 0:
                cursor.execute("INSERT INTO products VALUES(?, ?, ?, ?)", (pid, prdName.get(), q, price))
            else:
                cursor.execute("UPDATE products SET quantity = quantity + ?, price = ? WHERE productId = ?", (q, price, pid))
            connectObj.commit()
            connectObj.close()
            messagebox.showinfo("Success", f"Product {pid} added/updated.")
            self.admin_view_products()
        ctk.CTkButton(self, text="Add Product", command=add, width=200, height=40).pack(pady=10)
        ctk.CTkButton(self, text="Back", command=lambda: self.show_admin_menu('shopadmin'), width=200, height=40).pack(pady=5)

    def admin_update_price(self):
        self.clear()
        ctk.CTkLabel(self, text="Update Product Price", font=("Orbitron", 24, "bold"), text_color="#00ffe7").pack(pady=10)
        prdId = ctk.CTkEntry(self, placeholder_text="Product Id", width=250)
        prdId.pack(pady=5)
        prdPrice = ctk.CTkEntry(self, placeholder_text="New Price", width=250)
        prdPrice.pack(pady=5)
        def update():
            pid = getInt(prdId.get())
            price = prdPrice.get()
            if not pid or not price:
                messagebox.showerror("Error", "All fields required and must be valid!")
                return
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute("SELECT * FROM products WHERE productId = ?", [pid])
            if len(cursor.fetchall()) == 0:
                messagebox.showerror("Error", f"Product {pid} not found.")
            else:
                cursor.execute("UPDATE products SET price = ? WHERE productId = ?", (price, pid))
                connectObj.commit()
                messagebox.showinfo("Success", f"Price updated for product {pid}.")
            connectObj.close()
            self.admin_view_products()
        ctk.CTkButton(self, text="Update Price", command=update, width=200, height=40).pack(pady=10)
        ctk.CTkButton(self, text="Back", command=lambda: self.show_admin_menu('shopadmin'), width=200, height=40).pack(pady=5)

    def admin_view_employees(self):
        self.clear()
        ctk.CTkLabel(self, text="Employees", font=("Orbitron", 24, "bold"), text_color="#00ffe7").pack(pady=10)
        connectObj = db.connect("shopMgmt.db")
        cursor = connectObj.cursor()
        cursor.execute("SELECT * FROM employees")
        employees = cursor.fetchall()
        connectObj.close()
        text = tabulate(employees, headers=['Emp Id', 'Name', 'Job', 'Join Date'], tablefmt="fancy_grid") if employees else "No employees found."
        ctk.CTkTextbox(self, width=650, height=300, font=("Consolas", 14)).pack(pady=10)
        self.children[list(self.children)[-1]].insert("0.0", text) # type: ignore
        ctk.CTkButton(self, text="Back", command=lambda: self.show_admin_menu('shopadmin'), width=200, height=40).pack(pady=10)

    def admin_add_employee(self):
        self.clear()
        ctk.CTkLabel(self, text="Add Employee", font=("Orbitron", 24, "bold"), text_color="#00ffe7").pack(pady=10)
        empName = ctk.CTkEntry(self, placeholder_text="Employee Name", width=250)
        empName.pack(pady=5)
        empJob = ctk.CTkEntry(self, placeholder_text="Designation", width=250)
        empJob.pack(pady=5)
        def add():
            name = empName.get()
            job = empJob.get()
            joinDate = str(datetime.datetime.now().date())
            if not name or not job:
                messagebox.showerror("Error", "All fields required!")
                return
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute("INSERT INTO employees(empName, empJob, empJoinDate) VALUES(?, ?, ?)", (name, job, joinDate))
            connectObj.commit()
            connectObj.close()
            messagebox.showinfo("Success", f"Employee {name} added.")
            self.admin_view_employees()
        ctk.CTkButton(self, text="Add Employee", command=add, width=200, height=40).pack(pady=10)
        ctk.CTkButton(self, text="Back", command=lambda: self.show_admin_menu('shopadmin'), width=200, height=40).pack(pady=5)

    def change_password(self, userType, user):
        self.clear()
        ctk.CTkLabel(self, text="Change Password", font=("Orbitron", 24, "bold"), text_color="#00ffe7").pack(pady=10)
        oldPass = ctk.CTkEntry(self, placeholder_text="Current Password", show="*", width=250)
        oldPass.pack(pady=5)
        newPass = ctk.CTkEntry(self, placeholder_text="New Password", show="*", width=250)
        newPass.pack(pady=5)
        def change():
            op = oldPass.get().strip()
            np = newPass.get().strip()
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute("SELECT password FROM users WHERE userType = ?", (userType,))
            row = cursor.fetchone()
            if not row or op != row[0]:
                messagebox.showerror("Error", "Incorrect current password!")
                connectObj.close()
                return
            if len(np) < 8:
                messagebox.showerror("Error", "Password must be at least 8 characters!")
                connectObj.close()
                return
            cursor.execute("UPDATE users SET password = ? WHERE userType = ?", (np, userType))
            connectObj.commit()
            connectObj.close()
            messagebox.showinfo("Success", "Password changed successfully.")
            if userType == 0:
                self.show_admin_menu(user)
            else:
                self.show_cashier_menu(user)
        ctk.CTkButton(self, text="Change Password", command=change, width=200, height=40).pack(pady=10)
        ctk.CTkButton(self, text="Back", command=(self.show_admin_menu if userType == 0 else self.show_cashier_menu), width=200, height=40).pack(pady=5) # type: ignore

    # Cashier Functions
    def cashier_view_products(self):
        self.clear()
        ctk.CTkLabel(self, text="Available Products", font=("Orbitron", 24, "bold"), text_color="#00ffe7").pack(pady=10)
        connectObj = db.connect("shopMgmt.db")
        cursor = connectObj.cursor()
        cursor.execute("SELECT * FROM products")
        products = cursor.fetchall()
        connectObj.close()
        text = tabulate(products, headers=['Product Id', 'Product Name', 'Quantity', 'Price'], tablefmt="fancy_grid") if products else "No products found."
        ctk.CTkTextbox(self, width=650, height=300, font=("Consolas", 14)).pack(pady=10)
        self.children[list(self.children)[-1]].insert("0.0", text) # type: ignore
        ctk.CTkButton(self, text="Back", command=lambda: self.show_cashier_menu('cashier'), width=200, height=40).pack(pady=10)

    def cashier_generate_bill(self):
        import tkinter as tk
        from tkinter import ttk
        self.clear()
        # Set a max window size for the billing page
        self.geometry("1100x700")
        self.minsize(900, 650)
        self.state('zoomed')  # Always open in fullscreen
        # Top bar with title and total amount
        topbar = ctk.CTkFrame(self, fg_color="#181818")
        topbar.pack(fill="x", padx=0, pady=0)
        ctk.CTkLabel(topbar, text="ORDER DETAILS", font=("Orbitron", 24, "bold"), text_color="#2e3a59").pack(side="left", padx=30, pady=10)
        total_display_var = tk.StringVar(value="Total: ₹0.00")
        total_display_label = ctk.CTkLabel(topbar, textvariable=total_display_var, font=("Orbitron", 28, "bold"), text_color="#00ff55")
        total_display_label.place(relx=1.0, rely=1.0, anchor="se", x=-30, y=-10)
        
        
        # Top form frame
        form_frame = ctk.CTkFrame(self, fg_color="#23272e")
        form_frame.pack(pady=5, padx=10, fill="x")
        label_font = ("Orbitron", 16)
        entry_width = 180
        # Customer fields
        ctk.CTkLabel(form_frame, text="Phone Number:", font=label_font, text_color="#00ffe7").grid(row=0, column=0, sticky="e", padx=6, pady=6)
        phone_entry = ctk.CTkEntry(form_frame, width=entry_width, font=("Consolas", 13))
        phone_entry.grid(row=0, column=1, padx=6, pady=6)
        def fetch_customer_details(event=None):
            phone = phone_entry.get().strip()
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute('''CREATE TABLE IF NOT EXISTS customers(phone TEXT PRIMARY KEY, name TEXT, address TEXT)''')
            cursor.execute("SELECT rowid, name, address FROM customers WHERE phone = ?", (phone,))
            row = cursor.fetchone()
            connectObj.close()
            if row:
                custid_entry.delete(0, 'end')
                custid_entry.insert(0, str(row[0]))
                custname_entry.delete(0, 'end')
                custname_entry.insert(0, row[1])
                address_entry.delete(0, 'end')
                address_entry.insert(0, row[2])
            else:
                custid_entry.delete(0, 'end')
                custname_entry.delete(0, 'end')
                address_entry.delete(0, 'end')
        phone_entry.bind('<Return>', fetch_customer_details)
        search_cust_btn = ctk.CTkButton(form_frame, text="Search Customer", fg_color="blue", width=100, command=fetch_customer_details)
        search_cust_btn.grid(row=0, column=2, padx=6, pady=6)
        ctk.CTkLabel(form_frame, text="Customer ID:", font=label_font, text_color="#00ffe7").grid(row=1, column=0, sticky="e", padx=6, pady=6)
        custid_entry = ctk.CTkEntry(form_frame, width=entry_width, font=("Consolas", 13))
        custid_entry.grid(row=1, column=1, padx=6, pady=6)
        ctk.CTkLabel(form_frame, text="Customer Name:", font=label_font, text_color="#00ffe7").grid(row=1, column=2, padx=6, pady=6)
        custname_entry = ctk.CTkEntry(form_frame, width=entry_width, font=("Consolas", 13))
        custname_entry.grid(row=1, column=3, padx=6, pady=6)
        ctk.CTkLabel(form_frame, text="Address:", font=label_font, text_color="#00ffe7").grid(row=1, column=4, padx=6, pady=6)
        address_entry = ctk.CTkEntry(form_frame, width=entry_width, font=("Consolas", 13))
        address_entry.grid(row=1, column=5, padx=6, pady=6)
        total_display_var = total_display_var if 'total_display_var' in locals() else tk.StringVar(value="Total: ₹0.00")
        total_display_label = ctk.CTkLabel(self, textvariable=total_display_var, font=("Orbitron", 20, "bold"), text_color="#00cc44")
        total_display_label.pack(anchor="e", padx=50, pady=(10))
        # Product fields
        ctk.CTkLabel(form_frame, text="Product ID:", font=label_font, text_color="#00ffe7").grid(row=4, column=0, sticky="e", padx=6, pady=6)
        prodid_entry = ctk.CTkEntry(form_frame, width=entry_width, font=("Consolas", 13))
        prodid_entry.grid(row=4, column=1, padx=6, pady=6)
        def fetch_product_details(event=None):
            pid = prodid_entry.get().strip()
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute("SELECT productName, price, quantity FROM products WHERE productId = ?", (pid,))
            row = cursor.fetchone()
            connectObj.close()
            if row:
                prodname_entry.delete(0, 'end')
                prodname_entry.insert(0, row[0])
                price_entry.delete(0, 'end')
                price_entry.insert(0, str(row[1]))
                qty_entry.delete(0, 'end')
                qty_entry.insert(0, '1')
            else:
                prodname_entry.delete(0, 'end')
                price_entry.delete(0, 'end')
                qty_entry.delete(0, 'end')
        prodid_entry.bind('<Return>', fetch_product_details)
        fetch_prod_btn = ctk.CTkButton(form_frame, text="Fetch Product", fg_color="orange", width=100, command=fetch_product_details)
        fetch_prod_btn.grid(row=4, column=2, padx=6, pady=6)
        ctk.CTkLabel(form_frame, text="Product Name:", font=label_font, text_color="#00ffe7").grid(row=4, column=5, sticky="e", padx=6, pady=6)
        prodname_entry = ctk.CTkEntry(form_frame, width=entry_width, font=("Consolas", 13))
        prodname_entry.grid(row=4, column=6, padx=6, pady=6)
        ctk.CTkLabel(form_frame, text="Price:", font=label_font, text_color="#00ffe7").grid(row=4, column=3, padx=6, pady=6)
        price_entry = ctk.CTkEntry(form_frame, width=entry_width, font=("Consolas", 13))
        price_entry.grid(row=4, column=4, padx=6, pady=6)
        ctk.CTkLabel(form_frame, text="Quantity:", font=label_font, text_color="#00ffe7").grid(row=7, column=0, sticky="e", padx=6, pady=6)
        qty_entry = ctk.CTkEntry(form_frame, width=entry_width, font=("Consolas", 13))
        qty_entry.grid(row=7, column=1, padx=6, pady=6)
        # Always show Add Product button in a visible position
        add_prod_btn = ctk.CTkButton(form_frame, text="Add Product", fg_color="green", width=100, height=36, command=lambda: add_product())
        add_prod_btn.grid(row=7, column=2, padx=6, pady=6, sticky="nsew")
        # Table for order items
        table_frame = ctk.CTkFrame(self, fg_color="#fff")
        table_frame.pack(padx=10, pady=5, fill="x")  # Remove expand=True
        columns = ("Product ID", "Product Name", "Price", "Quantity", "Amount")
        order_table = ttk.Treeview(table_frame, columns=columns, show="headings", height=16)  # Limit height
        for col in columns:
            order_table.heading(col, text=col)
            order_table.column(col, width=120, anchor="center")
            
        # Add vertical scrollbar
        vsb = ttk.Scrollbar(table_frame, orient="vertical", command=order_table.yview)
        order_table.configure(yscrollcommand=vsb.set)
        order_table.grid(row=0, column=0, sticky="nsew")
        vsb.grid(row=0, column=1, sticky="ns")
        table_frame.grid_rowconfigure(0, weight=1)
        table_frame.grid_columnconfigure(0, weight=1)

        # Bottom summary frame
        summary_frame = ctk.CTkFrame(self, fg_color="#e0e0e0", border_width=0, border_color="#bbb")
        summary_frame.pack(pady=12, padx=30, fill="x")
        summary_label_font = ("Arial", 13, "bold")
        summary_label_color = "#111"
        entry_fg_color = "#fff"
        entry_text_color = "#111"
        entry_width = 140
        entry_font = ("Arial", 13)
        ctk.CTkLabel(summary_frame, text="Subtotal:", font=summary_label_font, text_color=summary_label_color).grid(row=0, column=0, sticky="e", padx=12, pady=7)
        subtotal_entry = ctk.CTkEntry(summary_frame, width=entry_width, font=entry_font, fg_color=entry_fg_color, text_color=entry_text_color)
        subtotal_entry.grid(row=0, column=1, padx=12, pady=7)
        ctk.CTkLabel(summary_frame, text="Discount (%):", font=summary_label_font, text_color=summary_label_color).grid(row=0, column=2, sticky="e", padx=12, pady=7)
        discount_entry = ctk.CTkEntry(summary_frame, width=entry_width, font=entry_font, fg_color=entry_fg_color, text_color=entry_text_color)
        discount_entry.grid(row=0, column=3, padx=12, pady=7)
        ctk.CTkLabel(summary_frame, text="GST (%):", font=summary_label_font, text_color=summary_label_color).grid(row=0, column=4, padx=12, pady=7)
        gst_entry = ctk.CTkEntry(summary_frame, width=entry_width, font=entry_font, fg_color=entry_fg_color, text_color=entry_text_color)
        gst_entry.grid(row=0, column=5, padx=12, pady=7)
        ctk.CTkLabel(summary_frame, text="Total:", font=summary_label_font, text_color=summary_label_color).grid(row=3, column=0, sticky="e", padx=12, pady=7)
        total_entry = ctk.CTkEntry(summary_frame, width=entry_width, font=entry_font, fg_color=entry_fg_color, text_color=entry_text_color)
        total_entry.grid(row=3, column=1, padx=12, pady=7)
        ctk.CTkLabel(summary_frame, text="Paid Amount:", font=summary_label_font, text_color=summary_label_color).grid(row=3, column=2,  padx=12, pady=7)
        paid_entry = ctk.CTkEntry(summary_frame, width=entry_width, font=entry_font, fg_color=entry_fg_color, text_color=entry_text_color)
        paid_entry.grid(row=3, column=3, padx=12, pady=7)
        ctk.CTkLabel(summary_frame, text="Return Amount:", font=summary_label_font, text_color=summary_label_color).grid(row=3, column=4, sticky="e", padx=12, pady=7)
        return_entry = ctk.CTkEntry(summary_frame, width=entry_width, font=entry_font, fg_color=entry_fg_color, text_color=entry_text_color)
        return_entry.grid(row=3, column=5, padx=12, pady=7)
        # Add a back button to the right of return amount
        back_btn_summary = ctk.CTkButton(summary_frame, text="Back", fg_color="#444", width=100, command=lambda: self.show_cashier_menu('cashier'))
        back_btn_summary.grid(row=3, column=6, padx=12, pady=7)
        # Order items list
        order_items = []
        def add_product():
            pid = prodid_entry.get().strip()
            pname = prodname_entry.get().strip()
            try:
                price = float(price_entry.get().strip())
                qty = int(qty_entry.get().strip())
            except:
                messagebox.showerror("Error", "Invalid price or quantity!")
                return
            if not pid or not pname or price <= 0 or qty <= 0:
                messagebox.showerror("Error", "Fill all product fields!")
                return
            # Check stock
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            cursor.execute("SELECT quantity FROM products WHERE productId = ?", (pid,))
            row = cursor.fetchone()
            connectObj.close()
            if not row or row[0] < qty:
                messagebox.showerror("Error", f"Not enough stock for {pname}!");
                return
            amount = price * qty
            order_items.append({"pid": pid, "pname": pname, "price": price, "qty": qty, "amount": amount})
            order_table.insert('', 'end', values=(pid, pname, price, qty, amount))
            prodid_entry.delete(0, 'end')
            prodname_entry.delete(0, 'end')
            price_entry.delete(0, 'end')
            qty_entry.delete(0, 'end')
            update_subtotal()
        def clear_order():
            order_items.clear()
            for i in order_table.get_children():
                order_table.delete(i)
            update_subtotal()
        def update_subtotal():
            subtotal = sum(item['amount'] for item in order_items)
            subtotal_entry.delete(0, 'end')
            subtotal_entry.insert(0, f"{subtotal:.2f}")
            total_entry.delete(0, 'end')
            total_entry.insert(0, f"{subtotal:.2f}")
            return_entry.delete(0, 'end')
            # Update the top-right total display
            total_display_var.set(f"Total: ₹{subtotal:.2f}")
        def calculate_total():
            try:
                subtotal = float(subtotal_entry.get())
                discount = float(discount_entry.get() or 0)
                gst = float(gst_entry.get() or 0)
                paid = float(paid_entry.get() or 0)
            except:
                messagebox.showerror("Error", "Invalid summary values!")
                return
            total = subtotal - (subtotal * discount / 100)
            total += total * gst / 100
            total_entry.delete(0, 'end')
            total_entry.insert(0, f"{total:.2f}")
            return_amt = paid - total
            return_entry.delete(0, 'end')
            return_entry.insert(0, f"{return_amt:.2f}")
            # Update the top-right total display
            total_display_var.set(f"Total: ₹{total:.2f}")

        def save_order():
            phone = phone_entry.get().strip()
            custid = custid_entry.get().strip()
            custname = custname_entry.get().strip()
            address = address_entry.get().strip()
            if not phone or not custname or not order_items:
                messagebox.showerror("Error","Fill all customer and order details!" )
                return
            try:
                subtotal = float(subtotal_entry.get())
                discount = float(discount_entry.get() or 0)
                gst = float(gst_entry.get() or 0)
                total = float(total_entry.get())
                paid = float(paid_entry.get() or 0)
                return_amt = float(return_entry.get() or 0)
            except:
                messagebox.showerror("Error", "Invalid summary values!")
                return
            # Save to DB
            connectObj = db.connect("shopMgmt.db")
            cursor = connectObj.cursor()
            # Update product stock
            for item in order_items:
                cursor.execute("UPDATE products SET quantity = quantity - ? WHERE productId = ?", (item['qty'], item['pid']))
            # Save purchaseHistory
            date = str(datetime.datetime.now().date())
            product_str = ', '.join([f"{item['pid']}x{item['qty']}" for item in order_items])
            cursor.execute('''INSERT INTO purchaseHistory(customerName, productIdsPurchased, amountPaid, date, customerPhone) VALUES (?, ?, ?, ?, ?)''', (custname, product_str, total, date, phone))
            connectObj.commit()
            connectObj.close()
            messagebox.showinfo("Success", "Order saved!")
            clear_order()
            
        clear_btn = ctk.CTkButton(summary_frame, text="Clear Order Items", fg_color="#333", width=100, command=clear_order)
        clear_btn.grid(row=6, column=0, padx=6, pady=8)
        save_btn = ctk.CTkButton(summary_frame, text="Save Order", fg_color="green", width=100, command=save_order)
        save_btn.grid(row=6, column=1, padx=6, pady=8)
        calc_btn = ctk.CTkButton(summary_frame, text="Calculate Total", fg_color="orange", width=100, command=calculate_total)
        calc_btn.grid(row=6, column=2, padx=6, pady=8)
        # Bind Enter key to add product
        # --- Top right action buttons ---
        button_frame = ctk.CTkFrame(self, fg_color="#181818")
        button_frame.place(relx=0.99, rely=0.01, anchor="ne")
        def clear_order_page():
            phone_entry.delete(0, 'end')
            custid_entry.delete(0, 'end')
            custname_entry.delete(0, 'end')
            address_entry.delete(0, 'end')
            prodid_entry.delete(0, 'end')
            prodname_entry.delete(0, 'end')
            price_entry.delete(0, 'end')
            qty_entry.delete(0, 'end')
            subtotal_entry.delete(0, 'end')
            discount_entry.delete(0, 'end')
            gst_entry.delete(0, 'end')
            total_entry.delete(0, 'end')
            paid_entry.delete(0, 'end')
            return_entry.delete(0, 'end')
            for i in order_table.get_children():
                order_table.delete(i)
            order_items.clear()
            total_display_var.set("Total: ₹0.00")
        clear_btn_top = ctk.CTkButton(button_frame, text="Clear Order", fg_color="#444", width=120, command=clear_order_page)
        clear_btn_top.pack(side="right", padx=8)
        calc_btn_top = ctk.CTkButton(button_frame, text="Calculate Total", fg_color="orange", width=140, command=calculate_total)
        calc_btn_top.pack(side="right", padx=8)
        def print_bill():
            import tempfile, os
            shop_name = "Futuristic Shop Management System"
            shop_addr = "123 Future Lane, City, Country"
            bill_lines = [f"{shop_name}\n{shop_addr}\n"]
            bill_lines.append(f"Customer: {custname_entry.get()}  Phone: {phone_entry.get()}")
            bill_lines.append(f"{'-'*40}")
            bill_lines.append(f"{'Product':<15}{'Qty':<5}{'Price':<8}{'Amt':<8}")
            # Fetch order items from the table to ensure latest data
            for row_id in order_table.get_children():
                vals = order_table.item(row_id)['values']
                # vals: [Product ID, Product Name, Price, Quantity, Amount]
                pname = str(vals[1])
                qty = str(vals[3])
                price = str(vals[2])
                amount = str(vals[4])
                bill_lines.append(f"{pname:<15}{qty:<5}{price:<8}{amount:<8}")
            bill_lines.append(f"{'-'*40}")
            bill_lines.append(f"Subtotal: {subtotal_entry.get()}")
            bill_lines.append(f"Discount: {discount_entry.get()}%  GST: {gst_entry.get()}%")
            bill_lines.append(f"Total: {total_entry.get()}")
            bill_lines.append(f"Paid: {paid_entry.get()}  Return: {return_entry.get()}")
            bill_content = '\n'.join(bill_lines)
            with tempfile.NamedTemporaryFile(delete=False, suffix='.txt', mode='w', encoding='utf-8') as f:
                f.write(bill_content)
                temp_path = f.name
            try:
                if os.name == 'nt':
                    os.startfile(temp_path, 'print')
                else:
                    
                    os.system(f"lpr '{temp_path}'")
            except Exception as e:
                messagebox.showerror("Print Error", f"Could not print bill: {e}")
        def save_and_print():
            save_order()
            print_bill()
            
        save_btn_top = ctk.CTkButton(button_frame, text="Save & Print Bill", fg_color="#607D8B", width=160, command=save_and_print)
        save_btn_top.pack(side="right", padx=8)
        # Add a back button for better navigation
        back_btn = ctk.CTkButton(self, text="Back", fg_color="#444", width=100, command=lambda: self.show_cashier_menu('cashier'))
        back_btn.pack(pady=10)

    def show_bill_window_advanced(self, name, phone, date, bill_lines, total):
        import tkinter as tk
        import tempfile
        import os
        bill_win = ctk.CTkToplevel(self)
        bill_win.title("Customer Bill")
        bill_win.geometry("400x500")
        ctk.CTkLabel(bill_win, text="Customer Bill", font=("Orbitron", 22, "bold"), text_color="#00ffe7").pack(pady=10)
        bill_text = tk.Text(bill_win, font=("Consolas", 12), bg="#222831", fg="#00ffe7", wrap="word")
        bill_text.pack(padx=10, pady=10, fill="both", expand=True)
        bill_content = f"Shop Management System\nDate: {date}\nCustomer: {name}\nPhone: {phone}\n\nItems:\n"
        bill_content += "\n".join(bill_lines)
        bill_content += f"\n\nTotal: Rs.{total}"
        bill_text.insert("1.0", bill_content)
        bill_text.config(state="disabled")
        def print_bill():
            with tempfile.NamedTemporaryFile(delete=False, suffix='.txt', mode='w', encoding='utf-8') as f:
                f.write(bill_content)
                temp_path = f.name
            try:
                if os.name == 'nt':
                    os.startfile(temp_path, 'print')
                else:
                    os.system(f"lpr '{temp_path}'")
            except Exception as e:
                messagebox.showerror("Print Error", f"Could not print bill: {e}")
        ctk.CTkButton(bill_win, text="Print Bill", command=print_bill, width=200, height=40).pack(pady=10)
        ctk.CTkButton(bill_win, text="Close", command=bill_win.destroy, width=200, height=40).pack(pady=5)

if __name__ == "__main__":
    initDB()
    app = ShopApp()
    app.mainloop()
