create database hospital_portal;
use hospital_portal;

create table patients(
 patient_id int not null unique auto_increment primary key,
 patient_name varchar(255) not null,
 age int not null,
 admission_date date,
 discharge_date date
);

CREATE TABLE doctors (
  doctor_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  doctor_name VARCHAR(255) NOT NULL,
  specialization VARCHAR(255) NOT NULL
);
CREATE TABLE appointments (
  appointment_id INT NOT NULL AUTO_INCREMENT PRIMARY KEY,
  patient_id INT NOT NULL,
  doctor_id INT NOT NULL,
  appointment_date DATE NOT NULL,
  appointment_time TIME NOT NULL,
  FOREIGN KEY 
  (patient_id) REFERENCES patients (patient_id),
  FOREIGN KEY (doctor_id) REFERENCES doctors (doctor_id)
);

INSERT INTO patients (patient_name, age, admission_date, discharge_date)
VALUES
     ('Mark Daniels', 25, '2023-10-11', '2023-10-17'),
    ('Dylan Johnson', 27, '2023-11-25', '2023-11-30'),
    ('Ben Ferguson', 30, '2023-12-21', '2023-12-26');
DELIMITER //

CREATE PROCEDURE ScheduleAppointment(IN Pi_patient_id INT, IN Di_doctor_id INT, IN Ad_appointment_date DATE, IN At_appointment_time DECIMAL(10, 5))
BEGIN
    INSERT INTO appointments (patient_id, doctor_id, appointment_date, appointment_time)
    VALUES (Pi_patient_id, Di_doctor_id, Ad_appointment_date, At_appointment_time);
END //
DELIMITER ;
DELIMITER //
CREATE PROCEDURE DischargePatient(IN Pi_patient_id INT)
BEGIN
    UPDATE patients SET discharge_date = CURDATE() WHERE patient_id = Pi_patient_id;
END //
DELIMITER ;

INSERT INTO doctors (doctor_name, specialization)
VALUES
   ('Dr. Martin', 'Ophthalmologist'),
    ('Dr. Kevin', 'Cardiologist'),
    ('Dr. Max', 'Anesthesiologist');
    
  CREATE VIEW doctors_appointments_patients_view AS
SELECT
    A.appointment_id,
    A.appointment_date,
    A.appointment_time,
    P.patient_id,
    P.patient_name,
    P.age,
    P.admission_date,
    P.discharge_date,
    D.doctor_id,
    D.doctor_name,
    D.specialization
FROM
    appointments a
JOIN
    patients p ON P.patient_id = P.patient_id
JOIN
    doctors d ON D.doctor_id = D.doctor_id;
