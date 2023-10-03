# CPP_Creative_Project
2nd year creative project

/*
*Author: Clement Sello Tsetsa 42010225
*Program: Energy Calculator
*Description: The program calculate the amount of energy used by electrical appliances
* and provides the total energy consumed to the user in the output.
*/

#include <iostream>
#include <fstream>
#include <string>
#include <vector>
#include <memory>
#include <algorithm>

// Define the Appliance struct
struct Appliance {
    std::string name;
    double powerRating;
    double duration;
};

// Function to add an appliance to the list
void AddAppliance(std::vector<Appliance>& appliances) {
    Appliance newAppliance;
    std::cout << "Enter the name of the appliance: ";
    std::cin >> newAppliance.name;
    std::cout << "Enter the power rating (in kilowatts or watts): ";
    std::cin >> newAppliance.powerRating;
    std::cout << "Enter the duration of use (in hours or minutes): ";
    std::cin >> newAppliance.duration;

    // Convert watts to kilowatts if necessary
    if (newAppliance.powerRating > 1000) {
        newAppliance.powerRating /= 1000.0;  // Convert watts to kilowatts
    }

    // Convert minutes to hours if necessary
    if (newAppliance.duration < 1) {
        newAppliance.duration /= 60.0;  // Convert minutes to hours
    }

    appliances.push_back(newAppliance);
}

// Function to display the list of appliances and total energy consumption
void DisplayAppliances(const std::vector<Appliance>& appliances) {
    if (appliances.empty()) {
        std::cout << "The list of appliances is empty." << std::endl;
        return;
    }

    std::cout << "List of Appliances:" << std::endl;
    std::cout << "--------------------------------------------------" << std::endl;
    std::cout << "Name\t\tPower Rating (kW)\tDuration (hours)\tEnergy (kWh)" << std::endl;
    std::cout << "--------------------------------------------------" << std::endl;

    double totalEnergy = 0.0;

    for (const Appliance& app : appliances) {
        double energy = app.powerRating * app.duration;
        totalEnergy += energy;
        std::cout << app.name << "\t\t" << app.powerRating << "\t\t\t" << app.duration << "\t\t" << energy << std::endl;
    }

    std::cout << "--------------------------------------------------" << std::endl;
    std::cout << "Total Energy Consumption (kWh): " << totalEnergy << std::endl;
}

// Function to delete an appliance by name
void DeleteAppliance(std::vector<Appliance>& appliances, const std::string& name) {
    auto it = std::remove_if(appliances.begin(), appliances.end(), [&name](const Appliance& app) {
        return app.name == name;
    });

    if (it != appliances.end()) {
        appliances.erase(it, appliances.end());
        std::cout << "Appliance '" << name << "' deleted successfully." << std::endl;
    } else {
        std::cout << "Appliance not found." << std::endl;
    }
}

int main() {
    std::vector<Appliance> appliances;

    while (true) {
        std::cout << "Energy Calculator" << std::endl;
        std::cout << "1. View Appliances" << std::endl;
        std::cout << "2. Delete Appliance" << std::endl;
        std::cout << "3. Add Appliance" << std::endl;
        std::cout << "4. Exit" << std::endl;
        std::cout << "Enter your choice: ";

        int choice;
        std::cin >> choice;

        switch (choice) {
            case 1:
                DisplayAppliances(appliances);
                break;
            case 2:
                {
                    std::string name;
                    std::cout << "Enter the name of the appliance to delete: ";
                    std::cin >> name;
                    DeleteAppliance(appliances, name);
                }
                break;
            case 3:
                AddAppliance(appliances);
                break;
            case 4:
                // Save the data to a file before exiting
                {
                    std::ofstream file("appliances.txt");
                    if (file.is_open()) {
                        for (const Appliance& app : appliances) {
                            file << app.name << " " << app.powerRating << " " << app.duration << "\n";
                        }
                        file.close();
                        std::cout << "Data saved to 'appliances.txt'." << std::endl;
                    } else {
                        std::cerr << "Error: Unable to open file for writing." << std::endl;
                    }
                }
                return 0;
            default:
                std::cout << "Invalid choice. Please try again." << std::endl;
        }
    }

    return 0;
}
