import json
from datetime import datetime

# Store user and patient data
users = {}
patients = {}


def register_user():
    username = input("Enter a username: ")
    if username in users:
        print("Username already exists. Try a different one.")
        return
    password = input("Enter a password: ")
    users[username] = password
    print("Registration successful!")


def login_user():
    username = input("Enter your username: ")
    password = input("Enter your password: ")
    if users.get(username) == password:
        print("Login successful!")
        return True
    print("Invalid credentials!")
    return False


def add_patient():
    patient_id = input("Enter Patient ID: ")
    if patient_id in patients:
        print("Patient already exists!")
        return
    name = input("Enter Patient Name: ")
    age = int(input("Enter Patient Age: "))
    condition = input("Enter Medical Condition: ")
    patients[patient_id] = {
        "name": name,
        "age": age,
        "condition": condition,
        "exercise_plan": [],
        "progress": []
    }
    print(f"Patient {name} added successfully!")


def view_patients():
    if not patients:
        print("No patients available.")
        return
    print("\nPatient List:")
    for pid, details in patients.items():
        print(f"ID: {pid}, Name: {details['name']}, Age: {details['age']}, Condition: {details['condition']}")


def create_exercise_plan():
    patient_id = input("Enter Patient ID: ")
    if patient_id not in patients:
        print("Patient not found!")
        return
    print(f"Creating an exercise plan for {patients[patient_id]['name']}.")
    exercises = []
    while True:
        exercise = input("Enter exercise (or type 'done' to finish): ")
        if exercise.lower() == 'done':
            break
        duration = input(f"Enter duration for {exercise} (e.g., 30 mins): ")
        exercises.append({"exercise": exercise, "duration": duration})
    patients[patient_id]['exercise_plan'] = exercises
    print("Exercise plan created successfully!")


def view_exercise_plan():
    patient_id = input("Enter Patient ID: ")
    if patient_id not in patients:
        print("Patient not found!")
        return
    plan = patients[patient_id].get('exercise_plan', [])
    if not plan:
        print("No exercise plan found for this patient.")
        return
    print(f"\nExercise Plan for {patients[patient_id]['name']}:")
    for exercise in plan:
        print(f"- {exercise['exercise']}: {exercise['duration']}")


def track_progress():
    patient_id = input("Enter Patient ID: ")
    if patient_id not in patients:
        print("Patient not found!")
        return
    progress_note = input("Enter progress note: ")
    date = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    patients[patient_id]['progress'].append({"date": date, "note": progress_note})
    print("Progress recorded successfully!")


def view_progress():
    patient_id = input("Enter Patient ID: ")
    if patient_id not in patients:
        print("Patient not found!")
        return
    progress = patients[patient_id].get('progress', [])
    if not progress:
        print("No progress recorded for this patient.")
        return
    print(f"\nProgress Notes for {patients[patient_id]['name']}:")
    for entry in progress:
        print(f"- {entry['date']}: {entry['note']}")


def main():
    print("Welcome to the Exercise Prescription System!")
    while True:
        print("\nMain Menu:")
        print("1. Register User")
        print("2. Login User")
        print("3. Add Patient")
        print("4. View Patients")
        print("5. Create Exercise Plan")
        print("6. View Exercise Plan")
        print("7. Track Progress")
        print("8. View Progress")
        print("9. Exit")

        choice = input("Enter your choice: ")
        if choice == "1":
            register_user()
        elif choice == "2":
            if login_user():
                print("Proceed to other operations.")
        elif choice == "3":
            add_patient()
        elif choice == "4":
            view_patients()
        elif choice == "5":
            create_exercise_plan()
        elif choice == "6":
            view_exercise_plan()
        elif choice == "7":
            track_progress()
        elif choice == "8":
            view_progress()
        elif choice == "9":
            print("Exiting the system. Goodbye!")
            break
        else:
            print("Invalid choice. Please try again.")


if __name__ == "__main__":
    main()
