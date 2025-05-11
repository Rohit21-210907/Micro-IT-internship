# Micro-IT-internship
import sqlite3

# Connect to SQLite DB (or create it)
conn = sqlite3.connect('database.db')
cursor = conn.cursor()

# Create a table
cursor.execute('''
    CREATE TABLE IF NOT EXISTS students (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT NOT NULL,
        age INTEGER,
        grade TEXT
    )
''')
conn.commit()

def add_student(name, age, grade):
    cursor.execute("INSERT INTO students (name, age, grade) VALUES (?, ?, ?)", (name, age, grade))
    conn.commit()
    print("✅ Student added successfully!")

def view_students():
    cursor.execute("SELECT * FROM students")
    rows = cursor.fetchall()
    print("\n--- All Students ---")
    for row in rows:
        print(row)

def search_student(name):
    cursor.execute("SELECT * FROM students WHERE name LIKE ?", ('%' + name + '%',))
    rows = cursor.fetchall()
    for row in rows:
        print(row)

def update_student(student_id, name, age, grade):
    cursor.execute("UPDATE students SET name=?, age=?, grade=? WHERE id=?", (name, age, grade, student_id))
    conn.commit()
    print("✅ Student updated successfully!")

def delete_student(student_id):
    cursor.execute("DELETE FROM students WHERE id=?", (student_id,))
    conn.commit()
    print("🗑️ Student deleted successfully!")

def main():
    while True:
        print("\n📘 Student Management System")
        print("1. Add Student")
        print("2. View All Students")
        print("3. Search Student")
        print("4. Update Student")
        print("5. Delete Student")
        print("6. Exit")

        choice = input("Enter your choice (1-6): ")

        if choice == '1':
            name = input("Enter name: ")
            age = int(input("Enter age: "))
            grade = input("Enter grade: ")
            add_student(name, age, grade)
        elif choice == '2':
            view_students()
        elif choice == '3':
            name = input("Enter name to search: ")
            search_student(name)
        elif choice == '4':
            student_id = int(input("Enter student ID to update: "))
            name = input("Enter new name: ")
            age = int(input("Enter new age: "))
            grade = input("Enter new grade: ")
            update_student(student_id, name, age, grade)
        elif choice == '5':
            student_id = int(input("Enter student ID to delete: "))
            delete_student(student_id)
        elif choice == '6':
            print("👋 Exiting...")
            break
        else:
            print("❌ Invalid choice. Please try again.")

    conn.close()

if __name__ == "__main__":
    main()
