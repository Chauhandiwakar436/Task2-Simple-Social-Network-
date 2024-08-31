# Task2-Simple-Social-Network-
Simple Social Network
#include <iostream>
#include <vector>
#include <map>
#include <string>
#include <fstream>
#include <iomanip>

using namespace std;

class User {
public:
    string username;
    int userID;
    vector<int> friends;
    vector<string> posts;

    User(int id, string name) : userID(id), username(name) {}

  
    void addFriend(int friendID) {
        friends.push_back(friendID);
    }

    
    void addPost(string post) {
        posts.push_back(post);
    }

   
    void displayProfile() const {
        cout << "User ID: " << userID << ", Username: " << username << endl;
        cout << "Friends: ";
        for (const int& friendID : friends) {
            cout << friendID << " ";
        }
        cout << endl;
        cout << "Posts:" << endl;
        for (const string& post : posts) {
            cout << "- " << post << endl;
        }
    }
};

class SocialNetwork {
private:
    map<int, User> users;  
    string filename;

  
    void loadFromFile() {
        ifstream file(filename);
        if (file.is_open()) {
            int id;
            string name;
            while (file >> id >> name) {
                users[id] = User(id, name);
                int friendCount, postCount;
                file >> friendCount;
                for (int i = 0; i < friendCount; i++) {
                    int friendID;
                    file >> friendID;
                    users[id].addFriend(friendID);
                }
                file >> postCount;
                file.ignore();
                for (int i = 0; i < postCount; i++) {
                    string post;
                    getline(file, post);
                    users[id].addPost(post);
                }
            }
            file.close();
        }
    }

 
    void saveToFile() const {
        ofstream file(filename);
        if (file.is_open()) {
            for (const auto& pair : users) {
                const User& user = pair.second;
                file << user.userID << " " << user.username << endl;
                file << user.friends.size() << endl;
                for (const int& friendID : user.friends) {
                    file << friendID << " ";
                }
                file << endl;
                file << user.posts.size() << endl;
                for (const string& post : user.posts) {
                    file << post << endl;
                }
            }
            file.close();
        }
    }

public:
    SocialNetwork(string filename) : filename(filename) {
        loadFromFile();
    }

  
    void addUser(int id, string name) {
        users[id] = User(id, name);
        saveToFile();
    }
 
    void addFriend(int userID, int friendID) {
        if (users.find(userID) != users.end() && users.find(friendID) != users.end()) {
            users[userID].addFriend(friendID);
            users[friendID].addFriend(userID);  // Mutual friendship
            saveToFile();
        } else {
            cout << "User not found!" << endl;
        }
    }

 
    void addPost(int userID, string post) {
        if (users.find(userID) != users.end()) {
            users[userID].addPost(post);
            saveToFile();
        } else {
            cout << "User not found!" << endl;
        }
    }
 
    void displayUserProfile(int userID) const {
        if (users.find(userID) != users.end()) {
            users.at(userID).displayProfile();
        } else {
            cout << "User not found!" << endl;
        }
    }
};
 class SocialNetwork {
private:
    map<int, User> users;  // Map of userID to User
    string filename;

 
    void loadFromFile() {
        ifstream file(filename);
        if (file.is_open()) {
            int id;
            string name;
            while (file >> id >> name) {
                users[id] = User(id, name);
                int friendCount, postCount;
                file >> friendCount;
                for (int i = 0; i < friendCount; i++) {
                    int friendID;
                    file >> friendID;
                    users[id].addFriend(friendID);
                }
                file >> postCount;
                file.ignore();
                for (int i = 0; i < postCount; i++) {
                    string post;
                    getline(file, post);
                    users[id].addPost(post);
                }
            }
            file.close();
        }
    }

  
    void saveToFile() const {
        ofstream file(filename);
        if (file.is_open()) {
            for (const auto& pair : users) {
                const User& user = pair.second;
                file << user.userID << " " << user.username << endl;
                file << user.friends.size() << endl;
                for (const int& friendID : user.friends) {
                    file << friendID << " ";
                }
                file << endl;
                file << user.posts.size() << endl;
                for (const string& post : user.posts) {
                    file << post << endl;
                }
            }
            file.close();
        }
    }

public:
    SocialNetwork(string filename) : filename(filename) {
        loadFromFile();
    }
 
    void addUser(int id, string name) {
        users[id] = User(id, name);
        saveToFile();
    }

 
    void addFriend(int userID, int friendID) {
        if (users.find(userID) != users.end() && users.find(friendID) != users.end()) {
            users[userID].addFriend(friendID);
            users[friendID].addFriend(userID);  // Mutual friendship
            saveToFile();
        } else {
            cout << "User not found!" << endl;
        }
    }

   
    void addPost(int userID, string post) {
        if (users.find(userID) != users.end()) {
            users[userID].addPost(post);
            saveToFile();
        } else {
            cout << "User not found!" << endl;
        }
    }

   
    void displayUserProfile(int userID) const {
        if (users.find(userID) != users.end()) {
            users.at(userID).displayProfile();
        } else {
            cout << "User not found!" << endl;
        }
    }
};
 

    int choice;
    do {
        cout << "\nSimple Social Network" << endl;
        cout << "1. Create User" << endl;
        cout << "2. Add Friend" << endl;
        cout << "3. Post Message" << endl;
        cout << "4. Display Profile" << endl;
        cout << "5. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1: {
            int id;
            string name;
            cout << "Enter User ID: ";
            cin >> id;
            cout << "Enter Username: ";
            cin >> name;
            sn.addUser(id, name);
            break;
        }
        case 2: {
            int userID, friendID;
            cout << "Enter Your User ID: ";
            cin >> userID;
            cout << "Enter Friend's User ID: ";
            cin >> friendID;
            sn.addFriend(userID, friendID);
            break;
        }
        case 3: {
            int userID;
            string post;
            cout << "Enter Your User ID: ";
            cin >> userID;
            cin.ignore();   
            cout << "Enter Your Post: ";
            getline(cin, post);
            sn.addPost(userID, post);
            break;
        }
        case 4: {
            int userID;
            cout << "Enter User ID to View Profile: ";
            cin >> userID;
            sn.displayUserProfile(userID);
            break;
        }
        case 5:
            cout << "Exiting..." << endl;
            break;
        default:
            cout << "Invalid choice! Please try again." << endl;
        }
    } while (choice != 5);

    return 0;
}
