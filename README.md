# Task1-contactbook-
CONTACT BOOK
import tkinter as tk
from tkinter import messagebox

CONTACTS_FILE = 'contacts.txt'


class ContactBook:
    def _init_(self, root):
        self.root = root
        self.root.title("Contact Book")
        self.root.geometry("400x400")

        self.name_label = tk.Label(root, text="Name")
        self.name_label.pack()

        self.name_entry = tk.Entry(root)
        self.name_entry.pack()

        self.phone_label = tk.Label(root, text="Phone Number")
        self.phone_label.pack()

        self.phone_entry = tk.Entry(root)
        self.phone_entry.pack()

        self.email_label = tk.Label(root, text="Email")
        self.email_label.pack()

        self.email_entry = tk.Entry(root)
        self.email_entry.pack()

        self.add_button = tk.Button(root, text="Add Contact", command=self.add_contact)
        self.add_button.pack()

        self.view_button = tk.Button(root, text="View Contacts", command=self.view_contacts)
        self.view_button.pack()

        self.delete_button = tk.Button(root, text="Delete Contact", command=self.delete_contact)
        self.delete_button.pack()

    def add_contact(self):
        name = self.name_entry.get().strip()
        phone = self.phone_entry.get().strip()
        email = self.email_entry.get().strip()

        if name and phone:
            with open(CONTACTS_FILE, 'a') as file:
                file.write(f'{name},{phone},{email}\n')
            messagebox.showinfo("Success", "Contact added successfully!")
            self.clear_entries()
        else:
            messagebox.showerror("Error", "Name and Phone Number are required!")

    def view_contacts(self):
        try:
            with open(CONTACTS_FILE, 'r') as file:
                contacts = file.readlines()

            if contacts:
                contact_list = '\n'.join([f'{contact.split(",")[0]} - {contact.split(",")[1]} - {contact.split(",")[2].strip()}' for contact in contacts])
                messagebox.showinfo("Contacts", contact_list)
            else:
                messagebox.showinfo("Contacts", "No contacts found.")
        except FileNotFoundError:
            messagebox.showinfo("Contacts", "No contacts found.")

    def delete_contact(self):
        name_to_delete = self.name_entry.get().strip()

        if name_to_delete:
            try:
                with open(CONTACTS_FILE, 'r') as file:
                    contacts = file.readlines()

                with open(CONTACTS_FILE, 'w') as file:
                    for contact in contacts:
                        if contact.split(",")[0] != name_to_delete:
                            file.write(contact)

                messagebox.showinfo("Success", f"Contact '{name_to_delete}' deleted successfully!")
                self.clear_entries()
            except FileNotFoundError:
                messagebox.showerror("Error", "No contacts to delete.")
        else:
            messagebox.showerror("Error", "Please enter the name of the contact to delete.")

    def clear_entries(self):
        self.name_entry.delete(0, tk.END)
        self.phone_entry.delete(0, tk.END)
        self.email_entry.delete(0, tk.END)


if _name_ == "_main_":
    root = tk.Tk()
    app = ContactBook(root)
    root.mainloop()
