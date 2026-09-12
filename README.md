# Contact-Management-System
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <limits>

using namespace std;

// ==================== CONTACT CLASS ====================
class Contact
{
public:
    string name;
    string phone;
    string email;
    string address;

    Contact() {}

    Contact(string n, string p, string e, string a)
    {
        name = n;
        phone = p;
        email = e;
        address = a;
    }

    void display() const
    {
        cout << "\n-----------------------------\n";
        cout << "Name    : " << name << endl;
        cout << "Phone   : " << phone << endl;
        cout << "Email   : " << email << endl;
        cout << "Address : " << address << endl;
        cout << "-----------------------------\n";
    }
};

// ==================== CONTACT MANAGER ====================
class ContactManager
{
private:
    vector<Contact> contacts;
    string fileName = "contacts.txt";

public:

    // Load contacts from file
    void loadContacts()
    {
        ifstream file(fileName);

        if (!file)
        {
            return;
        }

        contacts.clear();

        string name, phone, email, address;

        while (getline(file, name))
        {
            if (!getline(file, phone))
                break;

            if (!getline(file, email))
                break;

            if (!getline(file, address))
                break;

            contacts.push_back(Contact(name, phone, email, address));
        }

        file.close();
    }

    // Save contacts to file
    void saveContacts()
    {
        ofstream file(fileName);

        if (!file)
        {
            cout << "Error: Could not open file for saving.\n";
            return;
        }

        for (const Contact &c : contacts)
        {
            file << c.name << endl;
            file << c.phone << endl;
            file << c.email << endl;
            file << c.address << endl;
        }

        file.close();
    }

    // Add new contact
    void addContact()
    {
        string name, phone, email, address;

        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        cout << "\nEnter Name: ";
        getline(cin, name);

        cout << "Enter Phone: ";
        getline(cin, phone);

        cout << "Enter Email: ";
        getline(cin, email);

        cout << "Enter Address: ";
        getline(cin, address);

        contacts.push_back(Contact(name, phone, email, address));

        saveContacts();

        cout << "\nContact added successfully!\n";
    }

    // Display all contacts
    void displayContacts()
    {
        if (contacts.empty())
        {
            cout << "\nNo contacts available.\n";
            return;
        }

        cout << "\n====================================\n";
        cout << "          ALL CONTACTS\n";
        cout << "====================================\n";

        for (int i = 0; i < contacts.size(); i++)
        {
            cout << "\nContact " << i + 1 << ":";
            contacts[i].display();
        }
    }

    // Search contact
    void searchContact()
    {
        if (contacts.empty())
        {
            cout << "\nNo contacts available.\n";
            return;
        }

        string search;
        bool found = false;

        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        cout << "\nEnter name or phone to search: ";
        getline(cin, search);

        for (const Contact &c : contacts)
        {
            if (c.name.find(search) != string::npos ||
                c.phone.find(search) != string::npos)
            {
                c.display();
                found = true;
            }
        }

        if (!found)
        {
            cout << "\nContact not found.\n";
        }
    }

    // Edit contact
    void editContact()
    {
        if (contacts.empty())
        {
            cout << "\nNo contacts available.\n";
            return;
        }

        string search;
        bool found = false;

        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        cout << "\nEnter name or phone of contact to edit: ";
        getline(cin, search);

        for (Contact &c : contacts)
        {
            if (c.name == search || c.phone == search)
            {
                cout << "\nContact found.\n";
                c.display();

                cout << "\nEnter new details:\n";

                cout << "New Name: ";
                getline(cin, c.name);

                cout << "New Phone: ";
                getline(cin, c.phone);

                cout << "New Email: ";
                getline(cin, c.email);

                cout << "New Address: ";
                getline(cin, c.address);

                saveContacts();

                cout << "\nContact updated successfully!\n";

                found = true;
                break;
            }
        }

        if (!found)
        {
            cout << "\nContact not found.\n";
        }
    }

    // Delete contact
    void deleteContact()
    {
        if (contacts.empty())
        {
            cout << "\nNo contacts available.\n";
            return;
        }

        string search;
        bool found = false;

        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        cout << "\nEnter name or phone of contact to delete: ";
        getline(cin, search);

        for (auto it = contacts.begin();
             it != contacts.end();
             ++it)
        {
            if (it->name == search || it->phone == search)
            {
                it->display();

                char confirm;

                cout << "\nAre you sure you want to delete? (Y/N): ";
                cin >> confirm;

                if (confirm == 'Y' || confirm == 'y')
                {
                    contacts.erase(it);

                    saveContacts();

                    cout << "\nContact deleted successfully!\n";
                }
                else
                {
                    cout << "\nDeletion cancelled.\n";
                }

                found = true;
                break;
            }
        }

        if (!found)
        {
            cout << "\nContact not found.\n";
        }
    }
};

// ==================== MAIN FUNCTION ====================
int main()
{
    ContactManager manager;

    // Load saved contacts when program starts
    manager.loadContacts();

    int choice;

    do
    {
        cout << "\n\n========================================\n";
        cout << "       CONTACT MANAGEMENT SYSTEM\n";
        cout << "========================================\n";

        cout << "1. Add Contact\n";
        cout << "2. Display All Contacts\n";
        cout << "3. Search Contact\n";
        cout << "4. Edit Contact\n";
        cout << "5. Delete Contact\n";
        cout << "6. Exit\n";

        cout << "----------------------------------------\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
            manager.addContact();
            break;

        case 2:
            manager.displayContacts();
            break;

        case 3:
            manager.searchContact();
            break;

        case 4:
            manager.editContact();
            break;

        case 5:
            manager.deleteContact();
            break;

        case 6:
            cout << "\nContacts saved successfully.\n";
            cout << "Thank you for using Contact Management System!\n";
            break;

        default:
            cout << "\nInvalid choice! Please try again.\n";
        }

    } while (choice != 6);

    return 0;
}