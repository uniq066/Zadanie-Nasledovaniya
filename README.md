# Zadanie-Nasledovaniya#include <iostream>
#include <vector>
#include <string>
#include <limits> // Для std::numeric_limits

using namespace std;

class Building {
protected:
    double area;
    string address;

public:
    Building(double a, string addr) : area(a), address(addr) {}
    virtual void display() const {
        cout << "Address: " << address << ", Area: " << area << " sq. meters" << endl;
    }
    virtual ~Building() {}
    
    bool operator==(const Building& other) const {
        return area == other.area && address == other.address;
    }
};

class House : public Building {
public:
    House(double a, string addr) : Building(a, addr) {}
    void display() const override {
        cout << "House - ";
        Building::display();
    }
};

class Garage : public Building {
public:
    Garage(double a, string addr) : Building(a, addr) {}
    void display() const override {
        cout << "Garage - ";
        Building::display();
    }
};

class Apartment : public Building {
public:
    Apartment(double a, string addr) : Building(a, addr) {}
    void display() const override {
        cout << "Apartment - ";
        Building::display();
    }
};

// Функция для получения корректного целочисленного ввода
int getValidIntInput() {
    int input;
    while (true) {
        cin >> input;
        if (cin.fail() || input <= 0) {
            cin.clear(); // Очистка флага ошибки
            cin.ignore(numeric_limits<streamsize>::max(), '\n'); // Игнорирование некорректного ввода
            cout << "Please enter a positive integer: ";
        } else {
            cin.ignore(numeric_limits<streamsize>::max(), '\n'); // Игнорирование оставшегося ввода
            return input;
        }
    }
}

// Функция для получения корректного строкового ввода
string getValidStringInput() {
    string input;
    cout << "Enter the address: ";
    getline(cin, input);
    return input;
}

// Функция для получения корректного вещественного ввода
double getValidDoubleInput() {
    double input;
    while (true) {
        cin >> input;
        if (cin.fail() || input <= 0) {
            cin.clear(); // Очистка флага ошибки
            cin.ignore(numeric_limits<streamsize>::max(), '\n'); // Игнорирование некорректного ввода
            cout << "Please enter a positive number for the area: ";
        } else {
            cin.ignore(numeric_limits<streamsize>::max(), '\n'); // Игнорирование оставшегося ввода
            return input;
        }
    }
}

int main() {
    vector<Building*> buildings;
    int choice;

    do {
        cout << "\nMenu:\n";
        cout << "1. Add a building\n";
        cout << "2. Remove a building\n";
        cout << "3. Display all buildings\n";
        cout << "4. Compare two buildings\n";
        cout << "5. Exit\n";
        cout << "Enter your choice: ";
        choice = getValidIntInput();

        switch (choice) {
            case 1: {
                cout << "Choose the type of building:\n";
                cout << "1. House\n";
                cout << "2. Garage\n";
                cout << "3. Apartment\n";
                int type = getValidIntInput();

                cout << "Enter the area: ";
                double area = getValidDoubleInput(); // Используем новую функцию для ввода площади

                string address = getValidStringInput();

                Building* building = nullptr;
                switch (type) {
                    case 1:
                        building = new House(area, address);
                        break;
                    case 2:
                        building = new Garage(area, address);
                        break;
                    case 3:
                        building = new Apartment(area, address);
                        break;
                    default:
                        cout << "Invalid type." << endl;
                        continue;
                }
                buildings.push_back(building);
                cout << "Building added successfully!" << endl; // Подтверждение добавления
                break;
            }
            case 2: {
                cout << "Enter the index of the building to remove: ";
                int index = getValidIntInput() - 1; // Индексы начинаются с 0
                if (index >= 0 && index < buildings.size()) {
                    delete buildings[index];
                    buildings.erase(buildings.begin() + index);
                    cout << "Building removed successfully." << endl; // Подтверждение удаления
                } else {
                    cout <<  cout << "Invalid index." << endl; // Уведомление о некорректном индексе
                }
                break;
            }
            case 3: {
                cout << "All buildings:\n";
                for (size_t i = 0; i < buildings.size(); ++i) {
                    cout << i + 1 << ". ";
                    buildings[i]->display(); // Вывод информации о каждом здании
                }
                break;
            }
            case 4: {
                cout << "Enter the indices of the two buildings to compare (1 to " << buildings.size() << "):\n";
                int index1 = getValidIntInput() - 1; // Индексы начинаются с 0
                int index2 = getValidIntInput() - 1; // Индексы начинаются с 0

                if (index1 >= 0 && index1 < buildings.size() && index2 >= 0 && index2 < buildings.size()) {
                    if (*buildings[index1] == *buildings[index2]) {
                        cout << "The buildings are equal." << endl; // Сообщение о равенстве
                    } else {
                        cout << "The buildings are not equal." << endl; // Сообщение о неравенстве
                    }
                } else {
                    cout << "Invalid indices." << endl; // Уведомление о некорректных индексах
                }
                break;
            }
            case 5: {
                cout << "Exiting the program." << endl; // Сообщение о выходе
                break;
            }
            default: {
                cout << "Invalid choice. Please try again." << endl; // Уведомление о некорректном выборе
                break;
            }
        }
    } while (choice != 5);

    // Освобождение памяти
    for (Building* building : buildings) {
        delete building; // Освобождение памяти для каждого здания
    }

    return 0; // Завершение программы
}
