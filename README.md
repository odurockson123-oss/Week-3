#include <iostream>
#include <fstream>
#include <sstream>
using namespace std;

// Mark attendance
void markAttendance() {
    ifstream studentFile("students.file");
    ofstream attendanceFile("attendance.file", ios::app);

    string course, date;

    cout << "\nEnter Course Code: ";
    cin >> course;

    cout << "Enter Date: ";
    cin >> date;

    string line;

    while (getline(studentFile, line)) {
        stringstream ss(line);
        string index, name, program;
        string status;

        getline(ss, index, '|');
        getline(ss, name, '|');
        getline(ss, program, '|');

        cout << "Student: " << name << " (" << index << ")\n";
        cout << "Enter Status (Present/Absent/Late): ";
        cin >> status;

        attendanceFile << course << "|" << date << "|" << index << "|" << name << "|" << status << endl;
    }

    cout << "Attendance marked successfully.\n";

    studentFile.close();
    attendanceFile.close();
}


// Reports and summary

// Display attendance list for session
void displayAttendanceList() {
    ifstream file("attendance.file");

    string course, date;
    string line;

    cout << "\nEnter Course Code: ";
    cin >> course;

    cout << "Enter Date: ";
    cin >> date;

    cout << "\nAttendance List:\n";

    while (getline(file, line)) {
        stringstream ss(line);

        string fCourse, fDate, index, name, status;

        getline(ss, fCourse, '|');
        getline(ss, fDate, '|');
        getline(ss, index, '|');
        getline(ss, name, '|');
        getline(ss, status, '|');

        if (fCourse == course && fDate == date) {
            cout << index << " - " << name << " : " << status << endl;
        }
    }

    file.close();
}


// Display summary counts
void displaySummary() {
    ifstream file("attendance.file");

    string course, date;
    string line;

    int present = 0, absent = 0, late = 0;

    cout << "\nEnter Course Code: ";
    cin >> course;

    cout << "Enter Date: ";
    cin >> date;

    while (getline(file, line)) {
        stringstream ss(line);

        string fCourse, fDate, index, name, status;

        getline(ss, fCourse, '|');
        getline(ss, fDate, '|');
        getline(ss, index, '|');
        getline(ss, name, '|');
        getline(ss, status, '|');

        if (fCourse == course && fDate == date) {
            if (status == "Present") present++;
            else if (status == "Absent") absent++;
            else if (status == "Late") late++;
        }
    }

    cout << "\nAttendance Summary:\n";
    cout << "Present: " << present << endl;
    cout << "Absent: " << absent << endl;
    cout << "Late: " << late << endl;

    file.close();
}


// Main menu

int main() {
    int choice;

    do {
        cout << "\n===== STUDENT ATTENDANCE SYSTEM =====\n";
        cout << "1. Display Attendance List\n";
        cout << "2. Display Attendance Summary\n";
        cout << "0. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch(choice) {
            case 1: displayAttendanceList(); break;
            case 2: displaySummary(); break;
            case 0: cout << "Exiting...\n"; break;
            default: cout << "Invalid choice.\n";
        }

    } while(choice != 0);

    return 0;
}
