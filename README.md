# Hostel-Management
This is our first semester project of c language in which me made a hostel management system.
#include <stdio.h>                          
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_STUDENTS 100
#define MAX_NAME_LENGTH 50
#define ROOMS_PER_HOSTEL 24

typedef struct {
    char name[MAX_NAME_LENGTH];
    char hostel;
    int roomNumber;
} Student;

Student students[MAX_STUDENTS];
int studentCount = 0;

int assignRoom() {
    return (rand() % ROOMS_PER_HOSTEL) + 1;
}

void addStudent() {
    if (studentCount >= MAX_STUDENTS) {
        printf("Maximum student limit reached.\n");
        return;
    }

    char name[MAX_NAME_LENGTH], block;
    printf("Enter student name: ");
    scanf(" %[^\n]", name);
    printf("Enter block (A/B/C): ");
    scanf(" %c", &block);

    if (block != 'A' && block != 'B' && block != 'C') {
        printf("Invalid block. Please try again.\n");
        return;
    }

    students[studentCount].roomNumber = assignRoom();
    strcpy(students[studentCount].name, name);
    students[studentCount].hostel = block;
    studentCount++;

    printf("Student added: %s, Hostel: %c, Room Number: %d\n", name, block, students[studentCount - 1].roomNumber);
}

void getStudentInfo() {
    char name[MAX_NAME_LENGTH];
    printf("Enter student name: ");
    scanf(" %[^\n]", name);

    for (int i = 0; i < studentCount; i++) {
        if (strcmp(students[i].name, name) == 0) {
            printf("Name: %s, Hostel: %c, Room Number: %d\n", students[i].name, students[i].hostel, students[i].roomNumber);
            return;
        }
    }

    printf("Student not found.\n");
}

void viewAllStudents() {
    if (studentCount == 0) {
        printf("No students registered.\n");
        return;
    }

    for (int i = 0; i < studentCount; i++) {
        printf("Name: %s, Hostel: %c, Room Number: %d\n", students[i].name, students[i].hostel, students[i].roomNumber);
    }
}

void initializeRandomStudents() {
    char *names[] = {"Alice", "Bob", "Charlie", "Diana", "Ethan", "Fiona"};
    char hostels[] = {'A', 'B', 'C'};
    for (int i = 0; i < 6; i++) {
        strcpy(students[studentCount].name, names[i]);
        students[studentCount].hostel = hostels[rand() % 3];
        students[studentCount].roomNumber = assignRoom();
        studentCount++;
    }
}

int main() {
    srand(time(NULL));
    initializeRandomStudents();

    char role;
    while (1) {
        printf("\nAre you an admin or a student? (A/S): ");
        scanf(" %c", &role);

        if (role == 'A' || role == 'a') {
            int choice;
            while (1) {
                printf("\nAdmin Menu:\n1. Add Student\n2. Get Student Info\n3. View All Students\n4. Logout\nChoose an option: ");
                scanf("%d", &choice);

                switch (choice) {
                    case 1:
                        addStudent();
                        break;
                    case 2:
                        getStudentInfo();
                        break;
                    case 3:
                        viewAllStudents();
                        break;
                    case 4:
                        printf("Logging out as admin.\n");
                        goto END_MENU;
                    default:
                        printf("Invalid option. Please try again.\n");
                }
            }
        } else if (role == 'S' || role == 's') {
            int choice;
            while (1) {
                printf("\nStudent Menu:\n1. Get Student Info\n2. Logout\nChoose an option: ");
                scanf("%d", &choice);

                switch (choice) {
                    case 1:
                        getStudentInfo();
                        break;
                    case 2:
                        printf("Logging out as student.\n");
                        goto END_MENU;
                    default:
                        printf("Invalid option. Please try again.\n");
                }
            }
        } else {
            printf("Invalid role. Please enter A for admin or S for student.\n");
        }

    END_MENU:;
    }

    return 0;
}
