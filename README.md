# Task-1-Python
import sqlite3

# Connect to the database
conn = sqlite3.connect('todo_list.db')
# Create a table if it doesn't exist (assuming 'id', 'task', and 'done' columns)
conn.execute('''CREATE TABLE IF NOT EXISTS todo_list (id INTEGER PRIMARY KEY AUTOINCREMENT, task TEXT, done INTEGER)''')

def add_task(task):
    """Adds a new task to the list."""
    conn.execute('INSERT INTO todo_list (task, done) VALUES (?, ?)', (task, 0))
    conn.commit()

def view_tasks():
    """Displays all tasks in the list."""
    cursor = conn.execute('SELECT id, task, done FROM todo_list')
    for row in cursor:
        status = 'Completed' if row[2] else 'Pending'
        print(f"ID: {row[0]} - Task: {row[1]} ({status})")

def mark_task_done(task_id):
    """Marks a task as completed."""
    conn.execute('UPDATE todo_list SET done = 1 WHERE id = ?', (task_id,))
    conn.commit()

def delete_task(task_id):
    """Removes a task from the list."""
    conn.execute('DELETE FROM todo_list WHERE id = ?', (task_id,))
    conn.commit()

def save_and_exit():
    """Saves the current list to a file (optional)"""
    # Implement saving logic (e.g., JSON or CSV)
    conn.close()

while True:
    print("\nTo-Do List App")
    print("1. Add Task")
    print("2. View Tasks")
    print("3. Mark Task Done")
    print("4. Delete Task")
    print("5. Save and Exit")
    choice = input("Enter your choice: ")

    if choice == '1':
        task = input("Enter task description: ")
        add_task(task)
    elif choice == '2':
        view_tasks()
    elif choice == '3':
        task_id = int(input("Enter task ID to mark done: "))
        mark_task_done(task_id)
    elif choice == '4':
        task_id = int(input("Enter task ID to delete: "))
        delete_task(task_id)
    elif choice == '5':
        save_and_exit()
        break
    else:
        print("Invalid choice. Please try again.")
