#Micro-IT internship 
import java.util.Scanner;

class Calculator {
    public double add(double a, double b) {
        return a + b;
    }

    public double subtract(double a, double b) {
        return a - b;
    }

    public double multiply(double a, double b) {
        return a * b;
    }

    public double divide(double a, double b) {
        if (b == 0) {
            throw new ArithmeticException("Division by zero is not allowed.");
        }
        return a / b;
    }
}

public class CalculatorApp {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Calculator calculator = new Calculator();

        System.out.println("Welcome to the Simple Calculator!");
        System.out.print("Enter the first number: ");
        double num1 = scanner.nextDouble();

        System.out.print("Enter the second number: ");
        double num2 = scanner.nextDouble();

        System.out.print("Enter the operation (+, -, *, /): ");
        char operation = scanner.next().charAt(0);
        double result;

        switch (operation) {
            case '+':
                result = calculator.add(num1, num2);
                break;
            case '-':
                result = calculator.subtract(num1, num2);
                break;
            case '*':
                result = calculator.multiply(num1, num2);
                break;
            case '/':
                try {
                    result = calculator.divide(num1, num2);
                } catch (ArithmeticException e) {
                    System.out.println(e.getMessage());
                    return; // Exit the program if division by zero
                }
                break;
            default:
                System.out.println("Invalid operation! Please use +, -, *, or /.");
                return; // Exit the program if the operation is invalid
        }

        System.out.println("The result is: " + result);
    }
}
