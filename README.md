# OSProject
# -----------------------------------------------------------
# Project Title : File Encryption and Decryption Utility (GUI)
# Subject       : Operating System Project
# Developed by  : Labhesh Shrimali,Akhil Shah,Raj patel | PDEU
# -----------------------------------------------------------

import os
from tkinter import Tk, Label, Button, filedialog, messagebox
from cryptography.fernet import Fernet

# ---------- Function to Generate Key ----------
def generate_key():
    key = Fernet.generate_key()
    with open("secret.key", "wb") as key_file:
        key_file.write(key)
    messagebox.showinfo("Key Generated", "Encryption key saved as 'secret.key'")

# ---------- Function to Load Key ----------
def load_key():
    if not os.path.exists("secret.key"):
        messagebox.showwarning("Warning", "No key file found. Please generate key first.")
        return None
    with open("secret.key", "rb") as key_file:
        return key_file.read()

# ---------- Function to Encrypt File ----------
def encrypt_file():
    key = load_key()
    if key is None:
        return
    file_path = filedialog.askopenfilename(title="Select file to encrypt")
    if not file_path:
        return

    fernet = Fernet(key)
    with open(file_path, "rb") as file:
        data = file.read()

    encrypted_data = fernet.encrypt(data)
    with open(file_path, "wb") as file:
        file.write(encrypted_data)

    messagebox.showinfo("Success", "File encrypted successfully!")

# ---------- Function to Decrypt File ----------
def decrypt_file():
    key = load_key()
    if key is None:
        return
    file_path = filedialog.askopenfilename(title="Select file to decrypt")
    if not file_path:
        return

    fernet = Fernet(key)
    with open(file_path, "rb") as file:
        encrypted_data = file.read()

    try:
        decrypted_data = fernet.decrypt(encrypted_data)
    except Exception:
        messagebox.showerror("Error", "Decryption failed! Wrong key or damaged file.")
        return

    with open(file_path, "wb") as file:
        file.write(decrypted_data)

    messagebox.showinfo("Success", "File decrypted successfully!")

# ---------- Main GUI ----------
def main():
    root = Tk()
    root.title("File Encryption & Decryption Utility")
    root.geometry("420x280")
    root.config(bg="#222")

    Label(root, text="File Encryption & Decryption", font=("Segoe UI", 14, "bold"), fg="white", bg="#222").pack(pady=15)
    Button(root, text="Generate Key", command=generate_key, width=25, height=2, bg="#28a745", fg="white").pack(pady=6)
    Button(root, text="Encrypt File", command=encrypt_file, width=25, height=2, bg="#007bff", fg="white").pack(pady=6)
    Button(root, text="Decrypt File", command=decrypt_file, width=25, height=2, bg="#ffc107", fg="black").pack(pady=6)
    Button(root, text="Exit", command=root.destroy, width=25, height=2, bg="#dc3545", fg="white").pack(pady=10)

    Label(root, text="Developed by 23BIT107,23BIT114,23BIT116 | PDEU", font=("Segoe UI", 9), fg="gray", bg="#222").pack(side="bottom", pady=8)

    root.mainloop()

if __name__ == "__main__":
    main()
