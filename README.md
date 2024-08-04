#CONTACT BOOK
import tkinter as tk
from tkinter import messagebox
import os
import json

CONTACT_FILE = 'contacts.json'

def load_contacts():                              #all contact show here
    if not os.path.exists(CONTACT_FILE):
        return []
    with open(CONTACT_FILE, 'r') as file:
        return json.load(file)

def save_contacts(contacts):                    #used to save contact
    with open(CONTACT_FILE, 'w') as file:
        json.dump(contacts, file, indent=4)

def add_contact():                              #used to add contact
    name = name_entry.get()
    phone = phone_entry.get()

    if name and phone :
        contacts.append({"name": name, "phone": phone})
        save_contacts(contacts)
        name_entry.delete(0, tk.END)
        phone_entry.delete(0, tk.END)
        refresh_listbox()
    else:
        messagebox.showwarning("Input Error", "Please fill all fields.")

def delete_contact():                         #used to delete contact
    try:
        selected_contact_index = contact_listbox.curselection()[0]
        del contacts[selected_contact_index]
        save_contacts(contacts)
        refresh_listbox()
    except IndexError:
        messagebox.showwarning("Selection Error", "Please select a contact.")

def refresh_listbox():                        #used to show activity in contact list
    contact_listbox.delete(0, tk.END)
    for contact in contacts:
        contact_listbox.insert(tk.END, f'{contact["name"]} - {contact["phone"]} ')

def search_contact():                        #used to search contact
    search_term = search_entry.get().lower()
    filtered_contacts = [contact for contact in contacts if search_term in contact['name'].lower() or search_term in contact['phone'] or search_term in contact['email']]
    refresh_listbox(filtered_contacts)

def update_contact():
    try:
        selected_contact_index = contact_listbox.curselection()[0]
        name = name_entry.get()
        phone = phone_entry.get()
        if name and phone:
            contacts[selected_contact_index] = {"name": name, "phone": phone}
            save_contacts(contacts)
            refresh_listbox()
        else:
            messagebox.showwarning("Input Error", "Please fill all fields.")
    except IndexError:
        messagebox.showwarning("Selection Error", "Please select a contact to update.")

# Load contacts from file
contacts = load_contacts()

# GUI Setup
root = tk.Tk()
root.title("Contact Book")

name_label = tk.Label(root, text="Name")
name_label.pack(pady=5)
name_entry = tk.Entry(root, width=50)
name_entry.pack(pady=5)

phone_label = tk.Label(root, text="Phone")
phone_label.pack(pady=5)
phone_entry = tk.Entry(root, width=50)
phone_entry.pack(pady=5)

add_button = tk.Button(root, text="Add Contact", command=add_contact)
add_button.pack(pady=5)

delete_button = tk.Button(root, text="Delete Contact", command=delete_contact)
delete_button.pack(pady=5)

search_label = tk.Label(root, text="Search")
search_label.pack(pady=5)
search_entry = tk.Entry(root, width=50)
search_entry.pack(pady=5)

search_button = tk.Button(root, text="Search Contact", command=search_contact)
search_button.pack(pady=5)

contact_listbox = tk.Listbox(root, width=70, height=15)
contact_listbox.pack(pady=10)

update_button = tk.Button(root, text="Update Contact", command=update_contact)
update_button.pack(pady=5)

refresh_listbox()

root.mainloop()
# codsoft3
