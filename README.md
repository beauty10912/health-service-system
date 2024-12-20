# health-service-system
class Patient:
    def __init__(self, patient_id, name, age, gender, medical_history=None):
        self.patient_id = patient_id
        self.name = name
        self.age = age
        self.gender = gender
        self.medical_history = medical_history if medical_history else []
        self.notifications = []  # Store notifications here

    def update_medical_history(self, record):
        self.medical_history.append(record)
        self.add_notification(f"Your medical history has been updated with: {record}")

    def view_details(self):
        return f"Patient ID: {self.patient_id}\nName: {self.name}\nAge: {self.age}\nGender: {self.gender}\nMedical History: {', '.join(self.medical_history)}"

    def add_notification(self, message):
        self.notifications.append(message)

    def view_notifications(self):
        if not self.notifications:
            print("No new notifications.")
        else:
            print(f"Notifications for {self.name}:")
            for notification in self.notifications:
                print(f"- {notification}")


class Appointment:
    def __init__(self, patient, doctor, date, time):
        self.patient = patient
        self.doctor = doctor
        self.date = date
        self.time = time

    def appointment_details(self):
        return f"Appointment for {self.patient.name} with Dr. {self.doctor} on {self.date} at {self.time}"

    def reschedule(self, new_date, new_time):
        self.date = new_date
        self.time = new_time
        self.patient.add_notification(f"Your appointment with Dr. {self.doctor} has been rescheduled to {new_date} at {new_time}.")

    def cancel(self):
        self.patient.add_notification(f"Your appointment with Dr. {self.doctor} scheduled for {self.date} at {self.time} has been canceled.")


class HealthServiceSystem:
    def __init__(self):
        self.patients = {}
        self.appointments = {}

    def add_patient(self):
        patient_id = int(input("Enter Patient ID: "))
        name = input("Enter Patient Name: ")
        age = int(input("Enter Patient Age: "))
        gender = input("Enter Patient Gender: ")
        medical_history = input("Enter Patient Medical History (comma separated): ").split(",") if input("Do you want to add medical history? (y/n): ").lower() == 'y' else []

        patient = Patient(patient_id, name, age, gender, medical_history)
        self.patients[patient_id] = patient
        print(f"Patient {name} added successfully!")
        patient.add_notification(f"Welcome {name}! Your patient account has been created.")

    def schedule_appointment(self):
        patient_id = int(input("Enter Patient ID for Appointment: "))
        if patient_id not in self.patients:
            print("Patient not found.")
            return

        doctor = input("Enter Doctor's Name: ")
        date = input("Enter Appointment Date (YYYY-MM-DD): ")
        time = input("Enter Appointment Time (HH:MM AM/PM): ")

        patient = self.patients[patient_id]
        appointment = Appointment(patient, doctor, date, time)
        self.appointments[appointment.appointment_details()] = appointment
        patient.add_notification(f"Your appointment with Dr. {doctor} has been scheduled for {date} at {time}.")
        print(f"Appointment scheduled for {patient.name} with Dr. {doctor} on {date} at {time}")

    def cancel_appointment(self):
        appointment_details = input("Enter Appointment details to cancel (e.g., 'Appointment for John Doe with Dr. Adams on 2024-12-21 at 10:00 AM'): ")
        if appointment_details in self.appointments:
            appointment = self.appointments[appointment_details]
            appointment.cancel()
            del self.appointments[appointment_details]
            print(f"Appointment {appointment_details} has been canceled.")
        else:
            print("Appointment not found.")

    def reschedule_appointment(self):
        appointment_details = input("Enter Appointment details to reschedule (e.g., 'Appointment for John Doe with Dr. Adams on 2024-12-21 at 10:00 AM'): ")
        if appointment_details in self.appointments:
            appointment = self.appointments[appointment_details]
            new_date = input("Enter new Appointment Date (YYYY-MM-DD): ")
            new_time = input("Enter new Appointment Time (HH:MM AM/PM): ")
            appointment.reschedule(new_date, new_time)
            print(f"Appointment {appointment_details} has been rescheduled to {new_date} at {new_time}.")
        else:
            print("Appointment not found.")

    def view_patient_details(self):
        patient_id = int(input("Enter Patient ID to view details: "))
        if patient_id in self.patients:
            patient = self.patients[patient_id]
            print(patient.view_details())
        else:
            print(f"Patient with ID {patient_id} not found.")

    def view_patient_notifications(self):
        patient_id = int(input("Enter Patient ID to view notifications: "))
        if patient_id in self.patients:
            patient = self.patients[patient_id]
            patient.view_notifications()
        else:
            print(f"Patient with ID {patient_id} not found.")

    def view_all_appointments(self):
        if not self.appointments:
            print("No appointments scheduled.")
        for appointment in self.appointments.values():
            print(appointment.appointment_details())


# Simulating the health service system
def main():
    system = HealthServiceSystem()

    while True:
        print("\nHealth Service System")
        print("1. Add Patient")
        print("2. Schedule Appointment")
        print("3. Cancel Appointment")
        print("4. Reschedule Appointment")
        print("5. View Patient Details")
        print("6. View Patient Notifications")
        print("7. View All Appointments")
        print("8. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            system.add_patient()
        elif choice == '2':
            system.schedule_appointment()
        elif choice == '3':
            system.cancel_appointment()
        elif choice == '4':
            system.reschedule_appointment()
        elif choice == '5':
            system.view_patient_details()
        elif choice == '6':
            system.view_patient_notifications()
        elif choice == '7':
            system.view_all_appointments()
        elif choice == '8':
            print("Exiting system...")
            break
        else:
            print("Invalid choice, please try again.")


if __name__ == "__main__":
    main()
    
