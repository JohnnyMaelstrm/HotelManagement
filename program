#include <iostream>
#include <vector>
#include <string>
#include <fstream>
#include <cstdlib>
#include <ctime>
#include <algorithm>
using namespace std;

struct Customer {
    string name;
    int booking_number;
    int room_number;
    int nights;
    double total_cost;
};

struct Room {
    bool isBooked;
    int room_number;
    int price_per_night;
    string room_type; 
};

const int single_room_price = 100;  
const int double_room_price = 150;  
const int max_rooms = 300;          


double countingdiscount() {
    int discount = rand() % 3; 
    if (discount == 0) return 0.0;   
    if (discount == 1) return 0.1;   
    return 0.2;  
}


bool checkingAvalibility(const vector<Room>& rooms, int room_number) {
    return !rooms[room_number - 1].isBooked;
}


void bookRoom(vector<Room>& rooms, vector<Customer>& customers) {
    string room_type;
    int nights;
    string name;

    cout << "You can choose a room type of your liking. We have either one person room or two person rooms available. "
            "The one person room costs 100 euros and the two person room costs 150 euros. Select the one that you like: (1hh or 2hh): ";
    cin >> room_type;

    if (room_type != "1hh" && room_type != "2hh") {
        cout << "Invalid room type!" << endl;
        return;
    }

    vector<int> available_rooms;
    for (int i = 0; i < rooms.size(); ++i) {
        if (rooms[i].room_type == room_type && !rooms[i].isBooked) {
            available_rooms.push_back(i + 1);  
        }
    }

    if (available_rooms.empty()) {
        cout << "No available rooms of type " << room_type << " at the moment. Please try again later." << endl;
        return;
    }

    cout << "Available " << room_type << " rooms: ";
    for (int room : available_rooms) {
        cout << room << " ";
    }
    cout << endl;

    int room_number;
    cout << "Please select the room you like: ";
    cin >> room_number;

    if (find(available_rooms.begin(), available_rooms.end(), room_number) == available_rooms.end()) {
        cout << "This room is already booked! Please select another room." << endl;
        return;
    }

    cout << "Please enter the name for reservation: ";
    cin.ignore();  
    getline(cin, name);

    cout << "Please enter the total nights for reservation: ";
    cin >> nights;

    int price = (room_type == "1hh") ? single_room_price : double_room_price;
    double total_cost = price * nights;

    double discount = countingdiscount();
    total_cost -= total_cost * discount;

    int booking_number = 10000 + rand() % 90000;

    for (int i = 0; i < rooms.size(); ++i) {
        if (rooms[i].room_number == room_number) {
            rooms[i].isBooked = true;
            break;
        }
    }

    Customer newCustomer = {name, booking_number, room_number, nights, total_cost};
    customers.push_back(newCustomer);

    ofstream outFile("Reservations.txt", ios::app); 
    if (outFile.is_open()) {
        outFile << "Booking Number: " << booking_number << "\n";
        outFile << "Customer Name: " << name << "\n";
        outFile << "Room Number: " << room_number << "\n";
        outFile << "Nights: " << nights << "\n";
        outFile << "Total Cost: " << total_cost << " euros\n";
        outFile << "------------------------\n";
        outFile.close();
    } else {
        cout << "Error opening file!" << endl;
    }

    cout << "Room has been booked. Your booking number is " << booking_number << ". Please don't lose your booking number." << endl;
    cout << "Total cost of your stay: " << total_cost << " euros" << endl;
}

void searchforreservation(const vector<Customer>& customers) {
    string search_name;

    cout << "Enter the name to search for reservation: ";
    cin.ignore();  
    getline(cin, search_name);

    bool customer_found = false;
    
    for (const auto& customer : customers) {
        if (customer.name == search_name) {
            cout << "Reservation found!" << endl;
            cout << "Name: " << customer.name << ", Room: " << customer.room_number
                 << ", Nights: " << customer.nights << ", Total cost: " << customer.total_cost << " euros" << endl;
            customer_found = true;
        }
    }

    if (!customer_found) {  
        cout << "No reservation found for " << search_name << endl;
    }
}

void mainMenu() {
    cout << "\n\t\t\t\t ----- Welcome to the hotel! -----";
    cout << "\n\t\t\t\t ----- Thank you for choosing us -----";
    cout << "\n\t\t\t\t ----- Enjoy your stay and don't hesitate to ask for help -----";
    cout << "\n\t\t\t\t ----- Please select from the following: -----";
    cout << "\n\t\t\t\t ----- 1. Book a room -----";
    cout << "\n\t\t\t\t ----- 2. Search for your reservation -----";
    cout << "\n\t\t\t\t ----- 3. Exit from the main menu -----";
    cout << "\n\t\t\t\t ----- Enter your choice: ----- ";
}

int main() {
    srand(time(0)); 

    int total_rooms = 40 + rand() % 261;  
    vector<Room> rooms(total_rooms);
    vector<Customer> customers;

    int half_rooms = total_rooms / 2;
    for (int i = 0; i < rooms.size(); ++i) {
        rooms[i].room_number = i + 1;
        rooms[i].isBooked = false;

        if (i < half_rooms) {
            rooms[i].room_type = "1hh"; 
            rooms[i].price_per_night = single_room_price;
        } else {
            rooms[i].room_type = "2hh";  
            rooms[i].price_per_night = double_room_price;
        }
    }

    int customer_choice;
    while (true) {
        mainMenu();  
        cin >> customer_choice;

        if (customer_choice == 1) {
            bookRoom(rooms, customers);  
        } else if (customer_choice == 2) {
            searchforreservation(customers);  
        } else if (customer_choice == 3) {
            cout << "Thank you for using this program."<< endl;
            cout << " Have a nice day!" << endl;
        
            break; 
        } else {
            cout << "Try again." << endl;
        }
    }

    return 0;
}
