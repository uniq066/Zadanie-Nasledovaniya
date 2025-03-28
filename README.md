# Zadanie-Nasledovaniya 
#include <iostream>
#include <vector>
#include <string>
#include <limits>

using namespace std;

// Базовый класс Техника
class Equipment {
protected:
    double price;
    string manufacturer;
    
public:
    Equipment() : price(0), manufacturer("") {}
    
    Equipment(double p, string m) {
        setPrice(p);
        manufacturer = m;
    }
    
    virtual void input() {
        cout << "Введите производителя: ";
        cin >> manufacturer;
        
        cout << "Введите цену: ";
        while(!(cin >> price) || price < 0) {
            cout << "Ошибка! Введите положительное число: ";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
        }
    }
    
    virtual void display() const {
        cout << "Производитель: " << manufacturer << endl;
        cout << "Цена: " << price << endl;
    }
    
    void setPrice(double p) {
        if(p >= 0) price = p;
        else price = 0;
    }
    
    bool operator==(const Equipment& other) const {
        return price == other.price && manufacturer == other.manufacturer;
    }
    
    Equipment operator+(const Equipment& other) const {
        return Equipment(price + other.price, manufacturer + "+" + other.manufacturer);
    }
    
    Equipment& operator++() {
        price++;
        return *this;
    }
    
    virtual ~Equipment() {}
};

// Бытовая техника
class HomeAppliance : public Equipment {
    int powerConsumption;
    string category;
    
public:
    HomeAppliance() : Equipment(), powerConsumption(0), category("") {}
    
    HomeAppliance(double p, string m, int pc, string c) 
        : Equipment(p, m), powerConsumption(pc), category(c) {}
        
    void input() override {
        Equipment::input();
        cout << "Введите потребляемую мощность (Вт): ";
        while(!(cin >> powerConsumption) || powerConsumption < 0) {
            cout << "Ошибка! Введите положительное число: ";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
        }
        cout << "Введите категорию: ";
        cin >> category;
    }
    
    void display() const override {
        Equipment::display();
        cout << "Потребляемая мощность: " << powerConsumption << " Вт" << endl;
        cout << "Категория: " << category << endl;
    }
};

// Садовая техника
class GardenEquipment : public Equipment {
    double weight;
    string type;
    
public:
    GardenEquipment() : Equipment(), weight(0), type("") {}
    
    GardenEquipment(double p, string m, double w, string t) 
        : Equipment(p, m), weight(w), type(t) {}
        
    void input() override {
        Equipment::input();
        cout << "Введите вес (кг): ";
        while(!(cin >> weight) || weight < 0) {
            cout << "Ошибка! Введите положительное число: ";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
        }
        cout << "Введите тип: ";
        cin >> type;
    }
    
    void display() const override {
        Equipment::display();
        cout << "Вес: " << weight << " кг" << endl;
        cout << "Тип: " << type << endl;
    }
};

// Автомобильная техника
class AutoEquipment : public Equipment {
    int horsePower;
    string model;
    
public:
    AutoEquipment() : Equipment(), horsePower(0), model("") {}
    
    AutoEquipment(double p, string m, int hp, string mod) 
        : Equipment(p, m), horsePower(hp), model(mod) {}
        
    void input() override {
        Equipment::input();
        cout << "Введите мощность (л.с.): ";
        while(!(cin >> horsePower) || horsePower < 0) {
            cout << "Ошибка! Введите положительное число: ";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
        }
        cout << "Введите модель: ";
        cin >> model;
    }
    
    void display() const override {
        Equipment::display();
        cout << "Мощность: " << horsePower << " л.с." << endl;
        cout << "Модель: " << model << endl;
    }
};

int main() {
    vector<Equipment*> items;
    int choice;
    
    while(true) {
        cout << "\nМеню:\n";
        cout << "1. Добавить новый элемент\n";
        cout << "2. Удалить элемент по индексу\n";
        cout << "3. Вывести все элементы\n";
        cout << "4. Сравнить два элемента\n";
        cout << "5. Выход\n";
        cout << "Выберите действие: ";
        
        while(!(cin >> choice) || choice < 1 || choice > 5) {
            cout << "Ошибка! Выберите число от 1 до 5: ";
            cin.clear();
            cin.ignore(numeric_limits<streamsize>::max(), '\n');
        }
        
        if(choice == 1) {
            cout << "\nВыберите тип техники:\n";
            cout << "1. Бытовая техника\n";
            cout << "2. Садовая техника\n";
            cout << "3. Автомобильная техника\n";
            
            int type;
            while(!(cin >> type) || type < 1 || type > 3) {
                cout << "Ошибка! Выберите число от 1 до 3: ";
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
            }
            
            Equipment* newItem = nullptr;
            switch(type) {
                case 1: newItem = new HomeAppliance(); break;
                case 2: newItem = new GardenEquipment(); break;
                case 3: newItem = new AutoEquipment(); break;
            }
            
            newItem->input();
            items.push_back(newItem);
            cout << "Элемент добавлен!\n";
        }
        else if(choice == 2) {
            if(items.empty()) {
                cout << "Список пуст!\n";
                continue;
            }
            
            cout << "Введите индекс для удаления (0-" << items.size()-1 << "): ";
            int index;
            while(!(cin >> index) || index < 0 || index >= items.size()) {
                cout << "Ошибка! Введите корректный индекс: ";
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
            }
            
            delete items[index];
            items.erase(items.begin() + index);
            cout << "Элемент удален!\n";
        }
        else if(choice == 3) {
            if(items.empty()) {
                cout << "Список пуст!\n";
                continue;
            }
            
            for(size_t i = 0; i < items.size(); i++) {
                cout << "\nЭлемент #" << i << ":\n";
                items[i]->display();
            }
        }
        else if(choice == 4) {
            if(items.size() < 2) {
                cout << "Недостаточно элементов для сравнения!\n";
                continue;
            }
            
            cout << "Введите первый индекс (0-" << items.size()-1 << "): ";
            int index1;
            while(!(cin >> index1) || index1 < 0 || index1 >= items.size()) {
                cout << "Ошибка! Введите корректный индекс: ";
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
            }
            
            cout << "Введите второй индекс (0-" << items.size()-1 << "): ";
            int index2;
            while(!(cin >> index2) || index2 < 0 || index2 >= items.size()) {
                cout << "Ошибка! Введите корректный индекс: ";
                cin.clear();
                cin.ignore(numeric_limits<streamsize>::max(), '\n');
            }
            
            if(*items[index1] == *items[index2])
                cout << "Элементы равны!\n";
            else
                cout << "Элементы не равны!\n";
        }
        else if(choice == 5) {
            break;
        }
    }
    
    // Очистка памяти
    for(auto item : items) {
        delete item;
    }
    
    return 0;
}
