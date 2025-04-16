Here is the English translation of your project description:

---

## **📝 Project Overview**

This project is a **real-time task management application (Todo List)** built with **Rust** using the **Axum** web framework. It combines:

- **A high-performance backend** (REST API + SSE for live updates)
- **A lightweight frontend** (HTMX for interactivity without complex JavaScript)
- **A PostgreSQL database** for persistent storage

The application allows:

- ✅ **Adding/deleting tasks** with immediate updates for all connected users
- ✅ **Displaying a dynamic list** without page reloads (via SSE)
- ✅ **Serving static assets** (CSS) for a clean UI

---

## **🛠️ Code Structure**

### **1. Backend (Axum)**

- **Main Routes**:
  - `GET /` → Home page (`home()`)
  - `GET /todos` → Retrieve all tasks (`fetch_todos()`)
  - `POST /todos` → Create a task (`create_todo()`)
  - `DELETE /todos/:id` → Delete a task (`delete_todo()`)
  - `GET /stream` → SSE stream for real-time updates (`handle_stream()`)
  - `GET /styles.css` → Static CSS (`styles()`)

- **Data Management**:
  - **SQLx** for PostgreSQL queries
  - **Models** (`Todo`, `TodoNew`, `TodoUpdate`) to structure data

- **Real-Time Functionality**:
  - **`BroadcastStream`** (Tokio) to notify clients of changes
  - **Server-Sent Events (SSE)** to push updates to the frontend

### **2. Frontend (HTMX + Templates)**

- **HTML Templates**:
  - Direct integration with Axum (e.g., `templates::TodoNewTemplate`)
  - Use of **HTMX** for AJAX requests (`hx-post`, `hx-delete`)

- **CSS**: Static file served via `/styles.css`

### **3. Architecture**

- **Shared State** (`AppState`):
  - Contains the database connection pool (`db`) and the broadcast channel (`tx`)

- **Error Handling**:
  - Centralized via `ApiError` (handling SQLx, Axum errors, etc.)

---

## **🚀 How to Run the Project**

### **Prerequisites**

1. **Rust**: Installed via [rustup](https://rustup.rs/)
2. **PostgreSQL**: Local or remote server (with a configured database)
3. **Environment Variables**: Create a `.env` file at the root:

   ```env
   DATABASE_URL=postgres://user:password@localhost/db_name
   ```

### **Steps**

1. **Clone the repository** (if applicable):

   ```bash
   git clone <repo_url>
   cd <project_dir>
   ```

2. **Install dependencies**:

   ```bash
   cargo build
   ```

3. **Apply SQLx migrations**:

   ```bash
   sqlx migrate run
   ```

4. **Start the server**:

   ```bash
   cargo run
   ```

   - The server starts at `http://localhost:3000`

5. **Access the application**:

   - Open `http://localhost:3000` in a browser

---

## **📌 Practical Functionality**

1. **Add a Task**:
   - The HTMX form sends a `POST /todos` request
   - The task is saved in the database and broadcasted to all clients via SSE

2. **Delete a Task**:
   - Click on an `hx-delete` button → `DELETE /todos/:id` request
   - Immediate UI update for all users

3. **Real-Time Streaming**:
   - The `/stream` page listens for SSE events and automatically updates the list

---

## **🔧 Debugging**

- **Logs**: Check server logs (`cargo run`)
- **SQLx Errors**: Enable logging with `RUST_LOG=sqlx=info`
- **SSE**: Test with `curl http://localhost:3000/stream` to see events

---

## **🌍 Deployment**

- **Cloud**: Use **Docker** + **AWS/Google Cloud** (with Rust + PostgreSQL image)
- **Render/Heroku**: Configure Rust buildpack and the database

---

A **minimalist yet powerful** project to explore Rust, Axum, and HTMX together! 🦀

---

For further insights and examples, you might find the following resources helpful:

- [HTMX Todo List with Axum and Tailwind CSS](https://github.com/BenJeau/htmx-todo)
- [Rust Axum Askama HTMX Todo App](https://github.com/emarifer/rust-axum-askama-htmx-todoapp)
- [Build an App with Rust and HTMX](https://medium.com/@eoinmitchell39/learn-rust-and-htmx-with-a-todo-application-055cf5bbf6cd)

Feel free to explore these projects to deepen your understanding and enhance your application. 
