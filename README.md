Explanation of the Tables:
1.Students Table: 
Stores information about students, including their names, contact details, and the hall they are assigned to (hall_id foreign key).

2.Halls Table: 
Contains details about the university halls, including name, address, capacity, and how many rooms are available.

3.RoomAllocations Table:
 Tracks the allocation of rooms to students in university halls. This table associates a student with a specific room in a hall and stores details about the duration of the stay.

4.Bookings Table:
 If there are hall events (such as conferences, parties, or academic events), this table stores the event bookings made by students. It stores the event name, date, and time, along with the booking status (confirmed or cancelled).

5Payments Table:
 Manages payments related to bookings made for hall events or room accommodations. It tracks the amount, payment status, and method used (e.g., credit card, cash).

6.Events Table: 
If your system supports managing events in the halls (e.g., student meetups, department events), this table stores event details like name, description, date, time, duration, and which hall is hosting the event.


Implementation

You can directly copy and paste all the commands from the text given here into the SQL console to create and insert values into your table



-- Creating the Students Table
CREATE TABLE Students (
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone_number VARCHAR(15),
    department VARCHAR(100),
    enrollment_year INT,
    hall_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (hall_id) REFERENCES Halls(hall_id)
);

-- Creating the Halls Table
CREATE TABLE Halls (
    hall_id INT PRIMARY KEY AUTO_INCREMENT,
    hall_name VARCHAR(100) NOT NULL,
    hall_address VARCHAR(255) NOT NULL,
    hall_capacity INT NOT NULL,  -- Number of rooms or students the hall can accommodate
    available_rooms INT NOT NULL,  -- Number of available rooms
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Creating the Room Allocation Table
CREATE TABLE RoomAllocations (
    allocation_id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT NOT NULL,
    hall_id INT NOT NULL,
    room_number VARCHAR(10) NOT NULL,
    allocation_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    duration INT,  -- Duration in months, for how long the student stays
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (hall_id) REFERENCES Halls(hall_id)
);

-- Creating the Bookings Table (for hall events)
CREATE TABLE Bookings (
    booking_id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT NOT NULL,
    event_name VARCHAR(100) NOT NULL,
    event_date DATE NOT NULL,
    event_time TIME NOT NULL,
    booking_status ENUM('pending', 'confirmed', 'cancelled') DEFAULT 'pending',
    booking_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (student_id) REFERENCES Students(student_id)
);

-- Creating the Payments Table
CREATE TABLE Payments (
    payment_id INT PRIMARY KEY AUTO_INCREMENT,
    booking_id INT NOT NULL,
    payment_amount DECIMAL(10, 2) NOT NULL,
    payment_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    payment_method ENUM('credit_card', 'debit_card', 'cash', 'online') NOT NULL,
    payment_status ENUM('pending', 'completed', 'failed') DEFAULT 'pending',
    FOREIGN KEY (booking_id) REFERENCES Bookings(booking_id)
);

-- Creating the Events Table (for managing hall events)
CREATE TABLE Events (
    event_id INT PRIMARY KEY AUTO_INCREMENT,
    event_name VARCHAR(100) NOT NULL,
    event_description TEXT,
    event_date DATE NOT NULL,
    event_time TIME NOT NULL,
    event_duration INT NOT NULL,  -- Duration in hours
    hall_id INT NOT NULL,  -- Which hall is hosting the event
    FOREIGN KEY (hall_id) REFERENCES Halls(hall_id),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
# hall-management
