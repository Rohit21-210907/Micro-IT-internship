# Micro-IT-internship

import java.sql.*;
import java.util.Scanner;

public class StudentManagementSystem {
    static final String DB_URL = "jdbc:sqlite:database.db";
    static Connection conn;
    static Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        try {
            conn = DriverManager.getConnection(DB_URL);
            createTable();

            while (true) {
                System.out.println("\n📘 Student Management System");
                System.out.println("1. Add Student");
                System.out.println("2. View All Students");
                System.out.println("3. Search Student");
                System.out.println("4. Update Student");
                System.out.println("5. Delete Student");
                System.out.println("6. Exit");
                System.out.print("Enter your choice (1-6): ");
                String choice = scanner.nextLine();

                switch (choice) {
                    case "1":
                        addStudent();
                        break;
                    case "2":
                        viewStudents();
                        break;
                    case "3":
                        searchStudent();
                        break;
                    case "4":
                        updateStudent();
                        break;
                    case "5":
                        deleteStudent();
                        break;
                    case "6":
                        System.out.println("👋 Exiting...");
                        conn.close();
                        return;
                    default:
                        System.out.println("❌ Invalid choice. Please try again.");
                }
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    static void createTable() throws SQLException {
        String sql = """
            CREATE TABLE IF NOT EXISTS students (
                id INTEGER PRIMARY KEY AUTOINCREMENT,
                name TEXT NOT NULL,
                age INTEGER,
                grade TEXT
            );
        """;
        Statement stmt = conn.createStatement();
        stmt.execute(sql);
    }

    static void addStudent() throws SQLException {
        System.out.print("Enter name: ");
        String name = scanner.nextLine();
        System.out.print("Enter age: ");
        int age = Integer.parseInt(scanner.nextLine());
        System.out.print("Enter grade: ");
        String grade = scanner.nextLine();

        String sql = "INSERT INTO students (name, age, grade) VALUES (?, ?, ?)";
        PreparedStatement pstmt = conn.prepareStatement(sql);
        pstmt.setString(1, name);
        pstmt.setInt(2, age);
        pstmt.setString(3, grade);
        pstmt.executeUpdate();
        System.out.println("✅ Student added successfully!");
    }

    static void viewStudents() throws SQLException {
        String sql = "SELECT * FROM students";
        Statement stmt = conn.createStatement();
        ResultSet rs = stmt.executeQuery(sql);
        System.out.println("\n--- All Students ---");
        while (rs.next()) {
            System.out.printf("%d | %s | %d | %s\n", rs.getInt("id"), rs.getString("name"), rs.getInt("age"), rs.getString("grade"));
        }
    }

    static void searchStudent() throws SQLException {
        System.out.print("Enter name to search: ");
        String name = scanner.nextLine();
        String sql = "SELECT * FROM students WHERE name LIKE ?";
        PreparedStatement pstmt = conn.prepareStatement(sql);
        pstmt.setString(1, "%" + name + "%");
        ResultSet rs = pstmt.executeQuery();
        while (rs.next()) {
            System.out.printf("%d | %s | %d | %s\n", rs.getInt("id"), rs.getString("name"), rs.getInt("age"), rs.getString("grade"));
        }
    }

    static void updateStudent() throws SQLException {
        System.out.print("Enter student ID to update: ");
        int id = Integer.parseInt(scanner.nextLine());
        System.out.print("Enter new name: ");
        String name = scanner.nextLine();
        System.out.print("Enter new age: ");
        int age = Integer.parseInt(scanner.nextLine());
        System.out.print("Enter new grade: ");
        String grade = scanner.nextLine();

        String sql = "UPDATE students SET name = ?, age = ?, grade = ? WHERE id = ?";
        PreparedStatement pstmt = conn.prepareStatement(sql);
        pstmt.setString(1, name);
        pstmt.setInt(2, age);
        pstmt.setString(3, grade);
        pstmt.setInt(4, id);
        int rows = pstmt.executeUpdate();
        if (rows > 0) {
            System.out.println("✅ Student updated successfully!");
        } else {
            System.out.println("❌ Student ID not found.");
        }
    }

    static void deleteStudent() throws SQLException {
        System.out.print("Enter student ID to delete: ");
        int id = Integer.parseInt(scanner.nextLine());

        String sql = "DELETE FROM students WHERE id = ?";
        PreparedStatement pstmt = conn.prepareStatement(sql);
        pstmt.setInt(1, id);
        int rows = pstmt.executeUpdate();
        if (rows > 0) {
            System.out.println("🗑️ Student deleted successfully!");
        } else {
            System.out.println("❌ Student ID not found.");
        }
    }
}
