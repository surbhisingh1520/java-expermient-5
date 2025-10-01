**Part a: Sum of Integers Using Autoboxing and Unboxing**


import java.util.ArrayList;
import java.util.Scanner;

public class SumUsingAutoboxing {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        ArrayList<Integer> numbers = new ArrayList<>();

        System.out.println("Enter integers (type 'stop' to finish):");

        while (true) {
            String input = sc.next();
            if (input.equalsIgnoreCase("stop")) {
                break;
            }
            try {
                // String parsing to int
                int num = Integer.parseInt(input);
                // Autoboxing: int -> Integer (stored in ArrayList)
                numbers.add(num);
            } catch (NumberFormatException e) {
                System.out.println("Invalid input, enter integers only!");
            }
        }

        int sum = 0;
        // Unboxing: Integer -> int (automatic in enhanced for-loop)
        for (Integer n : numbers) {
            sum += n;
        }

        System.out.println("Sum of entered integers = " + sum);
        sc.close();
    }
}


Part b: Serialization and Deserialization of a Student Object


  import java.io.*;

// Student class implementing Serializable
class Student implements Serializable {
    private static final long serialVersionUID = 1L; // for version control
    int studentID;
    String name;
    double grade;

    public Student(int studentID, String name, double grade) {
        this.studentID = studentID;
        this.name = name;
        this.grade = grade;
    }

    @Override
    public String toString() {
        return "StudentID: " + studentID + ", Name: " + name + ", Grade: " + grade;
    }
}

public class StudentSerialization {
    public static void main(String[] args) {
        String filename = "student.ser";

        // Serialization
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filename))) {
            Student s1 = new Student(101, "Roshni Soni", 9.1);
            oos.writeObject(s1);
            System.out.println("Student object serialized to file: " + filename);
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Deserialization
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filename))) {
            Student s2 = (Student) ois.readObject();
            System.out.println("Deserialized Student Object:");
            System.out.println(s2);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}


Part c: Menu-Based Employee Management System Using File Handling


  import java.io.*;
import java.util.Scanner;

class Employee implements Serializable {
    private static final long serialVersionUID = 1L;
    int id;
    String name;
    String designation;
    double salary;

    public Employee(int id, String name, String designation, double salary) {
        this.id = id;
        this.name = name;
        this.designation = designation;
        this.salary = salary;
    }

    @Override
    public String toString() {
        return "ID: " + id + ", Name: " + name +
               ", Designation: " + designation + ", Salary: " + salary;
    }
}

public class EmployeeManagementSystem {
    private static final String FILE_NAME = "employees.ser";

    public static void addEmployee(Employee emp) {
        try (ObjectOutputStream oos = new ObjectOutputStream(
                new FileOutputStream(FILE_NAME, true)) {
            @Override
            protected void writeStreamHeader() throws IOException {
                reset(); // Prevent header corruption when appending
            }
        }) {
            oos.writeObject(emp);
            System.out.println("Employee added successfully!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void displayEmployees() {
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream(FILE_NAME))) {
            while (true) {
                Employee emp = (Employee) ois.readObject();
                System.out.println(emp);
            }
        } catch (EOFException e) {
            // End of file reached, stop reading
        } catch (IOException | ClassNotFoundException e) {
            System.out.println("No employees found or error reading file.");
        }
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        while (true) {
            System.out.println("\n--- Employee Management Menu ---");
            System.out.println("1. Add Employee");
            System.out.println("2. Display All Employees");
            System.out.println("3. Exit");
            System.out.print("Enter your choice: ");

            int choice = sc.nextInt();

            switch (choice) {
                case 1:
                    System.out.print("Enter ID: ");
                    int id = sc.nextInt();
                    sc.nextLine(); // consume newline
                    System.out.print("Enter Name: ");
                    String name = sc.nextLine();
                    System.out.print("Enter Designation: ");
                    String designation = sc.nextLine();
                    System.out.print("Enter Salary: ");
                    double salary = sc.nextDouble();
                    Employee emp = new Employee(id, name, designation, salary);
                    addEmployee(emp);
                    break;

                case 2:
                    System.out.println("\nEmployee Records:");
                    displayEmployees();
                    break;

                case 3:
                    System.out.println("Exiting application...");
                    sc.close();
                    System.exit(0);

                default:
                    System.out.println("Invalid choice. Try again!");
            }
        }
    }
}
