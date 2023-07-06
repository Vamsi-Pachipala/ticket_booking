import java.sql.*;
import java.util.Random;
import java.util.Scanner;
public class PassengerRegistration {
    private static final int PASSENGER_ID_LENGTH = 7;
    private static final String DB_URL = "jdbc:mysql://localhost:3306/casestudy";
    private static final String DB_USERNAME = "root";
    private static final String DB_PASSWORD = "Vamsi@243";

    private static final Scanner scanner = new Scanner(System.in);

    public static void main(String[] args) {
        boolean exit = false;

        while (!exit) {
            displayMainMenu();
            int choice = scanner.nextInt();
            scanner.nextLine();

            switch (choice) {
                case 1:
                    registerPassenger();
                    break;
                case 2:
                    login();
                    break;
                case 3:
                    exit = true;
                    System.out.println("Exiting the application. Goodbye!");
                    break;
                default:
                    System.out.println("Invalid choice. Please try again.");
            }
        }
    }
    private static void displayMainMenu() {
        System.out.println("Menu:");
        System.out.println("1. Register");
        System.out.println("2. Login");
        System.out.println("3. Exit");
        System.out.print("Enter your choice: ");
    }
    private static void registerPassenger() {
        System.out.println("Registration");

        System.out.print("Passenger Name: ");
        String name = scanner.nextLine();

        System.out.print("Email: ");
        String email = scanner.nextLine();

        System.out.print("Password: ");
        String password = scanner.nextLine();

        System.out.print("Address: ");
        String address = scanner.nextLine();

        System.out.print("Contact Number: ");
        String contactNumber = scanner.nextLine();

        Random random = new Random();
        String passengerId = Integer.toString(random.nextInt((int) Math.pow(10, PASSENGER_ID_LENGTH)));

        try (Connection connection = DriverManager.getConnection(DB_URL, DB_USERNAME, DB_PASSWORD)) {
            String sql = "INSERT INTO register (passenger_id, name, email, password, address, contact_number) VALUES (?, ?, ?, ?, ?, ?)";
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setString(1, passengerId);
            statement.setString(2, name);
            statement.setString(3, email);
            statement.setString(4, password);
            statement.setString(5, address);
            statement.setString(6, contactNumber);
            statement.executeUpdate();
            System.out.println("Passenger Registration is successful and PassengerId is "+passengerId);
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static void login() {
        System.out.println("Login");

        System.out.print("PassengerId: ");
        String passengerId = scanner.nextLine();

        System.out.print("Password: ");
        String password = scanner.nextLine();


        try (Connection connection = DriverManager.getConnection(DB_URL, DB_USERNAME, DB_PASSWORD)) {
            String sql = "SELECT * FROM register WHERE passenger_id  = ? AND password = ?";
            PreparedStatement statement = connection.prepareStatement(sql);
            statement.setString(1,passengerId );
            statement.setString(2, password);
            ResultSet resultSet = statement.executeQuery();

            if (resultSet.next()) {
                System.out.println("Login successful!");

                boolean exit = false;

                while (!exit) {
                    displayTicketBookingMenu();
                    int choice = scanner.nextInt();
                    scanner.nextLine();

                    switch (choice) {
                        case 1:
                            bookTicket(passengerId,connection);
                            break;
                        case 2:
                            checkDetails(connection);
                            break;
                        case 3:
                            updateDetails(connection);
                            break;
                        case 4:
                            exit = true;
                            System.out.println("Logging out. Goodbye!");
                            break;
                        default:
                            System.out.println("Invalid choice. Please try again.");
                    }
                }
            } else {
                System.out.println("Login failed. Invalid email or password.");
            }

            resultSet.close();
            statement.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    private static void displayTicketBookingMenu() {
        System.out.println("Ticket Booking");
        System.out.println("1. Book Ticket");
        System.out.println("2. Check Details");
        System.out.println("3. Update Details");
        System.out.println("4. Logout");
        System.out.print("Enter your choice: ");
    }
    private static String getInput(Scanner scanner, String message, String defaultValue)
    {
        System.out.print(message);
        String input = scanner.nextLine();
        return input.isEmpty() ? defaultValue : input;
    }
    private static String getInput(Scanner scanner, String message)
    {
        System.out.print(message);
        return scanner.nextLine();
    }
    private static void bookTicket(String passengerId,Connection connection)
    {
        String[] arr={"New","Confirm","Hold"};
        Scanner scanner = new Scanner(System.in);
        Random ran=new Random();
        String pnrNo = Integer.toString((int)(Math.random()*9999999));
        String travelDate = getInput(scanner, "Travel Date (dd/mm/yyyy): ");
        String source = getInput(scanner, "Source: ");
        String destination = getInput(scanner, "Destination: ");
        String status = arr[ran.nextInt(arr.length)];
        String seatPreference = getInput(scanner, "Seat Preference (Middle/Aisle/Window): ");
        String mealPreference = getInput(scanner, "Meal Preference: ");
        try {
            insertBookingDetails(connection, passengerId, pnrNo, travelDate, source, destination, status, seatPreference, mealPreference);
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
        System.out.println("Ticket Booked successfully, Happy Journey...pnrNumber is "+pnrNo);

    }
    private static void checkDetails(Connection connection) {
        System.out.println("Checking passenger details");
        Scanner scanner = new Scanner(System.in);
        String passengerId = getInput(scanner, "Passenger ID: ");
        String sql = "SELECT * FROM Booking WHERE passenger_id = ?";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, passengerId);
            ResultSet resultSet = statement.executeQuery();
            if (resultSet.next()) {
                System.out.println("Booking Details:");
                System.out.println("Passenger ID: " + resultSet.getString("passenger_id"));
                System.out.println("PNR No: " + resultSet.getString("pnr_no"));
                System.out.println("Travel Date: " + resultSet.getString("travel_date"));
                System.out.println("Source: " + resultSet.getString("source"));
                System.out.println("Destination: " + resultSet.getString("destination"));
                System.out.println("Status: " + resultSet.getString("status"));
                System.out.println("Seat Preference: " + resultSet.getString("seat_preference"));
                System.out.println("Meal Preference: " + resultSet.getString("meal_preference"));
            } else {
                System.out.println("Booking not found for Passenger ID: " + passengerId);
            }
            resultSet.close();
            statement.close();
        } catch (SQLException e) {
            throw new RuntimeException(e);
        }
    }
    private static void updateDetails(Connection connection) throws SQLException {
        System.out.println("Updating passenger details");
        Scanner scanner = new Scanner(System.in);

        String passengerId = getInput(scanner, "Passenger ID: ");
        String sql = "SELECT * FROM Booking WHERE passenger_id = ?";
        PreparedStatement selectStatement = connection.prepareStatement(sql);
        selectStatement.setString(1, passengerId);
        ResultSet resultSet = selectStatement.executeQuery();
        if (resultSet.next()) {
            System.out.println("Current Booking Details:");
            try {
                System.out.println("Passenger ID: " + resultSet.getString("passenger_id"));
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
            System.out.println("PNR No: " + resultSet.getString("pnr_no"));
            System.out.println("Travel Date: " + resultSet.getString("travel_date"));
            System.out.println("Source: " + resultSet.getString("source"));
            System.out.println("Destination: " + resultSet.getString("destination"));
            System.out.println("Status: " + resultSet.getString("status"));
            System.out.println("Seat Preference: " + resultSet.getString("seat_preference"));
            System.out.println("Meal Preference: " + resultSet.getString("meal_preference"));
            System.out.println("Press Enter to keep current details");
            String travelDate = getInput(scanner, "New Travel Date (dd/mm/yyyy): ",resultSet.getString("travel_date"));
            String source = getInput(scanner, "New Source: ",resultSet.getString("source"));
            String destination = getInput(scanner, "New Destination: ",resultSet.getString("destination"));
            String seatPreference = getInput(scanner, "New Seat Preference (Middle/Aisle/Window): ",resultSet.getString("seat_preference"));
            String mealPreference = getInput(scanner, "New Meal Preference: ",resultSet.getString("meal_preference"));
            String updateSql = "UPDATE Booking SET travel_date = ?, source = ?, destination = ?,seat_preference = ?, meal_preference = ? WHERE passenger_id = ?";
            PreparedStatement updateStatement = connection.prepareStatement(updateSql);
            updateStatement.setString(1, travelDate);
            updateStatement.setString(2, source);
            updateStatement.setString(3, destination);
            updateStatement.setString(4, seatPreference);
            updateStatement.setString(5, mealPreference);
            updateStatement.setString(6, passengerId);
            updateStatement.executeUpdate();
            updateStatement.close();
            System.out.println("Booking details updated successfully.");
        } else {
            System.out.println("Booking not found for Passenger ID: " + passengerId);
        }
        resultSet.close();
        selectStatement.close();
    }
    private static void insertBookingDetails(Connection connection, String passengerId, String pnrNo, String travelDate,
                                             String source, String destination, String status, String seatPreference, String mealPreference) throws SQLException {
        String sql= "INSERT INTO booking (passenger_id, pnr_no, travel_date, source, destination, status, seat_preference, meal_preference) " +
                "VALUES (?, ?, ?, ?, ?, ?, ?, ?)";
        PreparedStatement statement = connection.prepareStatement(sql);
        statement.setString(1, passengerId);
        statement.setString(2, pnrNo);
        statement.setString(3, travelDate);
        statement.setString(4, source);
        statement.setString(5, destination);
        statement.setString(6, status);
        statement.setString(7, seatPreference);
        statement.setString(8, mealPreference);
        statement.executeUpdate();
        statement.close();
    }
}

