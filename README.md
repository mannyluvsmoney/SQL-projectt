import tkinter as tk
from tkinter import ttk, messagebox
import sqlite3

conn = sqlite3.connect("hospital.db")
c = conn.cursor()
c.execute("DROP TABLE IF EXISTS Patients")
conn.commit()
conn.close()



def setup_database():
    conn = sqlite3.connect("hospital.db")
    c = conn.cursor()
    c.execute("""
        CREATE TABLE IF NOT EXISTS Patients (
            patient_id INTEGER PRIMARY KEY,
            full_name TEXT,
            date_of_birth TEXT,
            gender TEXT,
            contact_number TEXT,
            address TEXT,
            city TEXT,
            state TEXT,
            zip_code TEXT,
            emergency_name TEXT,
            emergency_phone TEXT
        )
    """)
    conn.commit()
    conn.close()


def submit_data():
    name = entry_name.get()
    dob = entry_dob.get()
    gender = combo_gender.get()
    contact = entry_contact.get()
    address = entry_address.get()
    city = entry_city.get()
    state = entry_state.get()
    zip_code = entry_zip.get()
    emergency_name = entry_emergency_name.get()
    emergency_phone = entry_emergency_phone.get()

    if not all([name, dob, gender, contact, address, city, state, zip_code, emergency_name, emergency_phone]):
        messagebox.showerror("Error", "All fields are required.")
        return

    conn = sqlite3.connect("hospital.db")
    c = conn.cursor()
    c.execute("""
        INSERT INTO Patients 
        (full_name, date_of_birth, gender, contact_number, address, city, state, zip_code, emergency_name, emergency_phone)
        VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
    """, (name, dob, gender, contact, address, city, state, zip_code, emergency_name, emergency_phone))
    conn.commit()
    conn.close()

    messagebox.showinfo("Success", "Patient registered successfully.")

    # Clear fields
    entry_name.delete(0, tk.END)
    entry_dob.delete(0, tk.END)
    combo_gender.set("")
    entry_contact.delete(0, tk.END)
    entry_address.delete(0, tk.END)
    entry_city.delete(0, tk.END)
    entry_state.delete(0, tk.END)
    entry_zip.delete(0, tk.END)
    entry_emergency_name.delete(0, tk.END)
    entry_emergency_phone.delete(0, tk.END)


setup_database()

window = tk.Tk()
window.title("Hospital Patient Registration")

# Labels & Entry Fields
label_name = tk.Label(window, text="Patient Name:")
label_name.grid(row=0, column=0, padx=10, pady=5, sticky="w")
entry_name = tk.Entry(window)
entry_name.grid(row=0, column=1, padx=10, pady=5)

label_dob = tk.Label(window, text="Date of Birth (DD/MM/YYYY):")
label_dob.grid(row=1, column=0, padx=10, pady=5, sticky="w")
entry_dob = tk.Entry(window)
entry_dob.grid(row=1, column=1, padx=10, pady=5)

label_gender = tk.Label(window, text="Gender:")
label_gender.grid(row=2, column=0, padx=10, pady=5, sticky="w")
combo_gender = ttk.Combobox(window, values=["Male", "Female", "Other"], state="readonly")
combo_gender.grid(row=2, column=1, padx=10, pady=5)

label_contact = tk.Label(window, text="Contact Number:")
label_contact.grid(row=3, column=0, padx=10, pady=5, sticky="w")
entry_contact = tk.Entry(window)
entry_contact.grid(row=3, column=1, padx=10, pady=5)

label_address = tk.Label(window, text="Address:")
label_address.grid(row=4, column=0, padx=10, pady=5, sticky="w")
entry_address = tk.Entry(window)
entry_address.grid(row=4, column=1, padx=10, pady=5)

label_city = tk.Label(window, text="City:")
label_city.grid(row=5, column=0, padx=10, pady=5, sticky="w")
entry_city = tk.Entry(window)
entry_city.grid(row=5, column=1, padx=10, pady=5)

label_state = tk.Label(window, text="State:")
label_state.grid(row=6, column=0, padx=10, pady=5, sticky="w")
entry_state = tk.Entry(window)
entry_state.grid(row=6, column=1, padx=10, pady=5)

label_zip = tk.Label(window, text="Zip Code:")
label_zip.grid(row=7, column=0, padx=10, pady=5, sticky="w")
entry_zip = tk.Entry(window)
entry_zip.grid(row=7, column=1, padx=10, pady=5)

label_emergency_name = tk.Label(window, text="Emergency Contact Name:")
label_emergency_name.grid(row=8, column=0, padx=10, pady=5, sticky="w")
entry_emergency_name = tk.Entry(window)
entry_emergency_name.grid(row=8, column=1, padx=10, pady=5)

label_emergency_phone = tk.Label(window, text="Emergency Contact Phone:")
label_emergency_phone.grid(row=9, column=0, padx=10, pady=5, sticky="w")
entry_emergency_phone = tk.Entry(window)
entry_emergency_phone.grid(row=9, column=1, padx=10, pady=5)

# Submit Button
button_submit = tk.Button(window, text="Submit", command=submit_data)
button_submit.grid(row=10, column=0, columnspan=2, pady=15)

window.mainloop()
