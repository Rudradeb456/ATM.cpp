#include <iostream>
#include <string>
#include <fstream>
#include <cstdlib>
#include "bankAccount.cpp"

using namespace std;

class ATM : public bankAccount
{
private:
    string atmCardNumber;
    string atmPin;
    string atmExpiryDate;
    string atmCVV;
    string AccountNumber;

public:
    void MainMenuATM();
    void checkATMCard();
};

void ATM::MainMenuATM()
{
    int choice;
    loadFromFile();

    do
    {
        cout << "\n----------------------------------" << endl;
        cout << "1. Check Balance" << endl;
        cout << "2. Withdraw Money from ATM" << endl;
        cout << "3. Deposit Money to ATM" << endl;
        cout << "4. Mini Statement" << endl;
        cout << "5. Logout" << endl;
        cout << "----------------------------------" << endl;
        cout << "Enter your choice: ";

        cin >> choice;
        cin.ignore();

#ifdef _WIN32
        system("cls");
#else
        system("clear");
#endif

        switch (choice)
        {
        case 1:
            checkBalance();
            break;

        case 2:
            withdrawMoney();
            break;

        case 3:
            depositMoney();
            break;

        case 4:
            allTransactionHistory();
            break;

        case 5:
            cout << "Logging out...!" << endl;
            savetoFile();
            break;

        default:
            cout << "Invalid choice! Please try again." << endl;
        }

    } while (choice != 5);
}

void ATM::checkATMCard()
{
    string cardNumber;
    string pin;

    cout << "Enter ATM Card Number: ";
    getline(cin, cardNumber);

    cout << "Enter ATM Card PIN: ";
    getline(cin, pin);

    ifstream file(cardNumber + ".txt");

    if (!file.is_open())
    {
        cout << "Invalid ATM Card Number!" << endl;
        return;
    }

    getline(file, atmCardNumber);
    getline(file, AccountNumber);
    getline(file, atmExpiryDate);
    getline(file, atmCVV);
    getline(file, atmPin);

    file.close();

    if (atmCardNumber == cardNumber && atmPin == pin)
    {
        cout << "\nLogin Successful!" << endl;

        // accountNumber should be protected in bankAccount
        accountNumber = AccountNumber;

        MainMenuATM();
    }
    else
    {
        cout << "Invalid ATM PIN!" << endl;
    }
}

int main()
{
    ATM atm;
    atm.checkATMCard();
    return 0;
}
