# Phonebook-2.0
This improves on the previous Phonebook program in that it uses a single array of type PhonebookEntry instead of three individual arrays.

import java.util.Scanner;
import java.io.*;

public class Phonebook {
    public static void main(String [] args) throws Exception {
        try {
            final String FILENAME = "phonebook.txt";
            final int CAPACITY = 100;
            PhonebookEntry[] phonebook = new PhonebookEntry[CAPACITY];
            int size = read(FILENAME, phonebook, CAPACITY);
            Scanner sc = new Scanner(System.in);
            String firstName, lastName, number;
            char choice;
            int lookupCount = 0, reverseLookupCount = 0;
            System.out.print("lookup, reverse-lookup, quit (l/r/q)? ");
            choice = sc.next().charAt(0);
            while (choice != 'q') {
                if (choice == 'l') {
                    System.out.print("last name? ");
                    lastName = sc.next();
                    System.out.print("first name? ");
                    firstName = sc.next();
                    Name other = new Name(lastName, firstName);
                    PhoneNumber num = lookup(phonebook, other, size);
                    if (num != null)
                        System.out.println(other.getFormal() + "'s phone number is " + num.toString() + "\n");
                    else
                        System.out.println("-- Name not found\n");
                    lookupCount++;
                }
                else if (choice == 'r') {
                    System.out.print("phone number (nnn-nnn-nnnn)? ");
                    number = sc.next();
                    PhoneNumber otherNum = new PhoneNumber(number);
                    Name nm = reverseLookup(phonebook, otherNum, size);
                    if (nm != null)
                        System.out.println(number + " belongs to " + nm.getFormal());
                    else
                        System.out.println(" -- Phone number not found\n");
                    reverseLookupCount++;
                }
                System.out.print("lookup, reverse-lookup, quit (l/r/q)? ");
                choice = sc.next().charAt(0);
            }
            System.out.println(lookupCount + " lookups performed\n" + reverseLookupCount + " reverse lookups performed");
        }
        catch (FileNotFoundException e) {
            System.out.println("*** IOException *** " + e.getMessage());
        }
        catch (Exception e) {
            System.out.println("*** Exception *** " + e.getMessage());
        }
    }
    public static PhoneNumber lookup(PhonebookEntry [] phonebook, Name other, int size) {
        for (int i = 0; i < size; i++) {
            if (phonebook[i].getName().equals(other)) {
                return phonebook[i].getPhoneNumber();
            }
        }
        return null;
    }
    public static Name reverseLookup (PhonebookEntry[]phonebook, PhoneNumber otherNum, int size) {
            for (int i = 0; i < size; i++)
                if (phonebook[i].getPhoneNumber().equals(otherNum))
                    return phonebook[i].getName();
            return null;
    }
    static int read (String filename, PhonebookEntry []phonebook,int capacity) throws Exception {
            Scanner scanner = new Scanner(new File(filename));
            int size = 0;
            while (scanner.hasNext()) {
                if (size == capacity) throw new Exception("Phonebook capacity exceeded - increase size of underlying array");
                phonebook[size] = PhonebookEntry.read(scanner);
                size++;
            }
            return size;
    }
}

