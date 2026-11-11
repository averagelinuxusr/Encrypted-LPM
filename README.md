# Encrypted-LPM: Encrypted Local Password Manager

A desktop password manager written in Python. It stores your passwords **encrypted on your own computer**, with no cloud and no online account, and protects them with a single **master password**.

It is a personal project that I built in my free time to learn how real password managers protect data.

## Features

- **Master password** to unlock the app. It is never saved anywhere.
- **Encrypted storage**: every password is encrypted before it is saved in a local SQLite database.
- **Graphical interface** (Tkinter) to add, edit, delete and search saved credentials.
- **Show/hide** button for passwords and **Ctrl+C** to copy a password from the table.
- Works on **Windows and Linux**, and can be packaged as a single `.exe` file.

## How the security works

The design follows the same idea used by tools like KeePass:

1. On first setup, the app creates a random encryption key (**Fernet**, based on AES).
2. This key is itself encrypted with a key derived from your master password, using **PBKDF2-HMAC-SHA256** with **600,000 iterations** and a random **salt**. The result is saved in `secret.key`.
3. When you open the app, it derives the key from the password you type and tries to decrypt `secret.key`. If decryption works, the password was right. There is no stored password to compare with.
4. The decrypted key is then used to encrypt and decrypt the passwords in the database.

```
Master password + salt ──PBKDF2──> derived key ──decrypts──> secret.key ──> Fernet key ──> passwords in the database
```

Without the master password, both `secret.key` and the database are unreadable. This also means that **a forgotten master password cannot be recovered**.

## Getting started

Requirements: Python 3 and the `cryptography` library (Tkinter and SQLite are included with Python).

```bash
pip install cryptography

# 1. First time only: create your master password and the protected key
python genera_chiave.py

# 2. Start the password manager
python password_manager.py
```

Optional: to build a Windows executable with PyInstaller:

```bash
pip install pyinstaller
pyinstaller --onefile --noconsole --name "Password Manager" password_manager.py
```

The interface is in Italian.

## Project files

| File | Content |
|------|---------|
| `genera_chiave.py` | First setup: creates the encryption key and protects it with the master password |
| `password_manager.py` | Login window and main application |

Files created at runtime (`secret.key`, `password.db`) are excluded from the repository through `.gitignore`.

## Skills used

Python · applied cryptography (key derivation, symmetric encryption, salting) · SQLite · GUI development with Tkinter · secure handling of secrets
