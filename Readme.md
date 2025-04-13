# Distributed File System - Socket Programming

This project implements a simplified Distributed File System (DFS) using socket programming in C. It consists of a main server (S1), three secondary file servers (S2, S3, S4), and a command-line client (`w25clients`) that allows users to interact with the DFS.

> **Abstraction Note:** The client is only aware of the main server (S1) and does not directly interact with or know about the existence of S2, S3, or S4. All routing and file management is handled transparently by S1.
---

## Components

### `S1.c` – Main Server
- Acts as the central point for client interaction.
- Handles file uploads, downloads, deletions, and TAR file requests.
- Forwards files to appropriate secondary servers based on file type.

### `S2.c` – PDF File Server
- Stores and manages `.pdf` files.
- Responds to file operations forwarded by S1.

### `S3.c` – Text File Server
- Stores and manages `.txt` files.

### `S4.c` – ZIP File Server
- Stores and manages `.zip` files.

### `w25clients.c` – Command-line Client
- User interface to interact with the DFS.
- Sends commands to S1 to perform file operations.

---

## Features

- Upload `.c`, `.pdf`, `.txt`, or `.zip` files
- Download or remove files from distributed servers
- List filenames in a directory (`dispfnames`)
- Download a TAR archive of all files of a given type (`downltar`)
- Handles invalid commands and unsupported file types gracefully

---

## Supported Commands

```bash
uploadf <filename> <destination_path>    # Upload a file
downlf <filepath>                        # Download a file
removef <filepath>                       # Delete a file
downltar <filetype: .c/.txt/.pdf>       # Download TAR archive
dispfnames <directory_path>             # List files in directory
```

> **Note:** All file paths or destination paths provided by the client must start with `~S1/`. Paths must follow the virtual directory structure where `~S1/` is the base.

---

## Getting Started

### Compile
```bash
gcc S1.c -o S1
gcc S2.c -o S2
gcc S3.c -o S3
gcc S4.c -o S4
gcc w25clients.c -o w25clients
```

### Run Servers and Client (in separate terminals)
```bash
./S2   # On port 9002 (PDF)
./S3   # On port 9003 (TXT)
./S4   # On port 9004 (ZIP)
./S1   # Main server on port 9001
./w25clients
```

---

## 📁 Storage Directory Structure 

```text
~/S1         # Local .c file storage
~/S2         # PDF files
~/S3         # TXT files
~/S4         # ZIP files
```

---

##  Notes

- Ensure all servers are running before starting the client.
- File paths must follow the expected formats (e.g., no slashes in filename for `uploadf`).
- All file operations from the client should begin with `~S1/` to ensure proper routing.
- Error messages are sent back to the client with a prefix `'E'` and should be handled accordingly.

---

## Authors

- Saima Khatoon
