#include <iostream>
#include <math.h>
#include <ctime>
#include <cstdlib>
#include <string>
#include <vector>
#include <fstream>

using namespace std;

// Function prototypes
inline bool IsPrime(unsigned long);
int GeneratePrime();
string Encrypt(const string&, int, int);
bool CheckUsernameExists(const string&);
void WriteCredentialsToFile(const string&, const string&, int, int);
bool CheckCredentials(const string&, const string&, int, int);

int main()
{
    int p, q, fi, mod;
    p = GeneratePrime();
    q = GeneratePrime();
    while (q == p) {
        q = GeneratePrime();
    }

    mod = p * q;
    fi = (p - 1) * (q - 1);

    int e = 2, d = 2;
    for (; ; e++) {
        if (IsPrime(e) && e < fi && (fi % e != 0))
            break;
    }
    for (; ; d++) {
        if ((e * d) % fi == 1)
            break;
    }

    cout << "1. Register" << endl;
    cout << "2. Login" << endl;
    int choice;
    cin >> choice;

    if (choice == 1) {
        cout << "Enter a username: ";
        string username;
        cin >> username;

        if (CheckUsernameExists(username)) {
            cout << "Error: Username already exists. Please choose a different username." << endl;
            return 0;
        }

        cout << "Enter a password: ";
        string password;
        cin >> password;

        WriteCredentialsToFile(username, password, e, mod);
    } else if (choice == 2) {
        cout << "Enter your username: ";
        string username;
        cin >> username;

        cout << "Enter your password: ";
        string password;
        cin >> password;

        if (CheckCredentials(username, password, e, mod)) {
            cout << "Login successful." << endl;
        } else {
            cout << "Invalid username or password." << endl;
        }
    }

    return 0;
}

inline bool IsPrime(unsigned long n)
{
    if (n < 2)
        return false;
    for (unsigned long j = 2; j * j <= n; ++j)
        if (n % j == 0)
            return false;
    return true;
}

int GeneratePrime() {
    srand(time(NULL));
    int a;
    unsigned int b = -1;
    for (unsigned long i = 1; i <= 100000; i++)
    {
        a = 2 + rand() % b;
        if (IsPrime(a))
            return a;
    }
    return 2; // Fallback to 2 if no prime found (unlikely)
}

string Encrypt(const string& text, int e, int mod) {
    vector<int> a;
    for (int i = 0; i < text.size(); i++) {
        a.push_back(text[i]);
    }
    for (int i = 0; i < a.size(); i++) {
        for (int j = 0; j < e; j++) {
            a[i] *= e;
        }
        a[i] %= mod;
    }

    string encryptedText;
    for (int i = 0; i < a.size(); i++) {
        encryptedText += to_string(a[i]) + " ";
    }

    return encryptedText;
}

bool CheckUsernameExists(const string& username) {
    ifstream file("credentials.txt");
    if (file.is_open()) {
        string line;
        while (getline(file, line)) {
            size_t pos = line.find(':');
            string storedUsername = line.substr(0, pos);
            if (storedUsername == username) {
                file.close();
                return true;
            }
        }
        file.close();
    }
    return false;
}

void WriteCredentialsToFile(const string& username, const string& password, int e, int mod) {
    ofstream file("credentials.txt", ios::app);
    if (file.is_open()) {
        file << username << ":" << Encrypt(password, e, mod) << endl;
        file.close();
        cout << "Registration successful. Please login with your credentials." << endl;
    } else {
        cout << "Error: Unable to open file for writing." << endl;
    }
}

bool CheckCredentials(const string& username, const string& password, int e, int mod) {
    ifstream file("credentials.txt");
    if (file.is_open()) {
        string line;
        while (getline(file, line)) {
            size_t pos = line.find(':');
            string storedUsername = line.substr(0, pos);
            string storedPassword = line.substr(pos + 1);
            if (storedUsername == username && Encrypt(password, e, mod) == storedPassword) {
                file.close();
                return true;
            }
        }
        file.close();
    }
    return false;
}