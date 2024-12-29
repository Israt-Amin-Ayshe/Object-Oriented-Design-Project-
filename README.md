# Object-Oriented-Design-Project-
import java.io.*;
import java.util.Scanner;

public class BusManagementSystem {

    private static final int MAX_TRIES = 3;
    private static final String BUS_FILE = "buses.txt";  // File to store bus information

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String username, password;
        int loginAttempts = 0;
        int choice;

        // Login page
        System.out.println("\t\t\t====================================================");
        System.out.println("\t\t\t Welcome to the Bus Management System ");
        System.out.println("\t\t\t====================================================\n");

        // Login logic with maximum attempts
        while (loginAttempts < MAX_TRIES) {
            System.out.println("Please log in to continue");
            System.out.print("Username: ");
            username = scanner.nextLine();
            password = takePassword();

            if (username.equals("admin") && password.equals("admin123")) {
                System.out.println("\nLogin successful! Loading menu...\n");
                break; // Exit the loop if login is successful
            } else {
                System.out.println("Incorrect username or password. Please try again.\n");
                loginAttempts++;
                if (loginAttempts < MAX_TRIES) {
                    System.out.println("Attempts remaining: " + (MAX_TRIES - loginAttempts) + "\n");
                } else {
                    System.out.println("No attempts left. Exiting program.");
                    return; // Exit the program after 3 unsuccessful attempts
                }
            }
        }

        // Main menu after login
        do {
            System.out.println("1. Add Bus");
            System.out.println("2. View Buses");
            System.out.println("3. Search Bus");
            System.out.println("4. Exit");
            System.out.print("Enter your choice: ");
            choice = scanner.nextInt();
            scanner.nextLine(); // Consume the newline

            switch (choice) {
                case 1:
                    addBus();
                    break;
                case 2:
                    viewBuses();
                    break;
                case 3:
                    searchBus();
                    break;
                case 4:
                    System.out.println("Exiting the program.");
                    System.out.println("\t\t\t\tTHANK YOU");
                    break;
                default:
                    System.out.println("Invalid choice. Please choose again.");
            }
        } while (choice != 4); // Repeat menu until the user exits
    }

    // Helper method to take the password input securely
    private static String takePassword() {
        Console console = System.console();
        if (console == null) {
            System.out.print("Password: ");
            Scanner scanner = new Scanner(System.in);
            return scanner.nextLine();
        } else {
            return new String(console.readPassword("Password: "));
        }
    }

    // Method to add a new bus
    private static void addBus() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter bus number: ");
        String busNumber = scanner.nextLine();
        System.out.print("Enter bus route: ");
        String busRoute = scanner.nextLine();
        System.out.print("Enter bus capacity: ");
        int busCapacity = scanner.nextInt();

        try (BufferedWriter writer = new BufferedWriter(new FileWriter(BUS_FILE, true))) {
            writer.write(busNumber + " " + busRoute + " " + busCapacity);
            writer.newLine();
            System.out.println("Bus added successfully!");
        } catch (IOException e) {
            System.out.println("Error opening file.");
        }
    }

    // Method to view all buses
    private static void viewBuses() {
        try (BufferedReader reader = new BufferedReader(new FileReader(BUS_FILE))) {
            String line;
            System.out.println("Buses in the System:");
            while ((line = reader.readLine()) != null) {
                String[] parts = line.split(" ");
                String busNumber = parts[0];
                String busRoute = parts[1];
                int busCapacity = Integer.parseInt(parts[2]);
                System.out.println("Bus Number: " + busNumber + ", Route: " + busRoute + ", Capacity: " + busCapacity);
            }
        } catch (IOException e) {
            System.out.println("Error opening file.");
        }
    }

    // Method to search for a bus by bus number
    private static void searchBus() {
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter bus number to search: ");
        String searchBusNumber = scanner.nextLine();

        try (BufferedReader reader = new BufferedReader(new FileReader(BUS_FILE))) {
            String line;
            boolean found = false;
            while ((line = reader.readLine()) != null) {
                String[] parts = line.split(" ");
                String busNumber = parts[0];
                String busRoute = parts[1];
                int busCapacity = Integer.parseInt(parts[2]);

                if (busNumber.equals(searchBusNumber)) {
                    System.out.println("Bus Number: " + busNumber + ", Route: " + busRoute + ", Capacity: " + busCapacity);
                    found = true;
                    break; // Exit the loop when the bus is found
                }
            }
            if (!found) {
                System.out.println("Bus not found.");
            }
        } catch (IOException e) {
            System.out.println("Error opening file.");
        }

    }
}
