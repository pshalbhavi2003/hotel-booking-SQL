# hotel-booking-SQL
SQL project to manage hotel bookings, customers, rooms, payments, and tours, tour reservation ,change logs

create database abc_resort_booking;
use abc_resort_booking;

create table customers(customer_id int primary key,
 name varchar(20), 
 phone bigint unique, 
 email varchar(20) unique,
 created_at timestamp default(current_timestamp()) 
 );
 
 drop table payments;
 
 create table rooms(room_id int primary key,
 room_type varchar(20),
 price_per_night decimal(10,2),
 status varchar(20) check(status in("available","booked"))
 );
 
 create table bookings(booking_id int primary key,
 customer_id int,
 room_id int,
 checkin_date date,
 checkout_date date,
 status varchar(20) check (status in ('confirmed', 'checked_in', 'completed', 'cancelled')),
 foreign key (customer_id) references customers(customer_id),
 foreign key (room_id) references rooms(room_id)
 );
 
 create table payments (payment_id int primary key,
    booking_id int,
    amount  decimal(10,2),
    payment_date timestamp default(current_timestamp()),
    method  varchar(20) check (method in ('cash', 'card', 'Phone pay')),
    foreign key (booking_id) references bookings(booking_id)
);

 create table tours (tour_id int primary key,
tour_name  varchar(50),
price_per_person decimal(10,2),
capacity int
);

create table tour_reservations (reservation_id int primary key,
    customer_id int,
    tour_id int,
    pax_count int,
    reservation_date timestamp default(current_timestamp()),
    foreign key (customer_id) references customers(customer_id),
    foreign key (tour_id) references tours(tour_id)
);

 create table change_log (log_id int primary key,
    booking_id  int,
    old_status  varchar(20),
    new_status  varchar(20),
    changed_at  timestamp default(current_timestamp()),
    foreign key (booking_id) references bookings(booking_id)
);

insert into customers (customer_id, name, phone, email )values
(1, 'Rahul Sharma', '9876543210', 'rahul@gmail.com'),
(2, 'Priya Mehta',  '9988776655', 'priya@gmail.com'),
(3, 'Arjun Verma',  '9123456789', 'arjun@gmail.com'),
(4, 'Neha Gupta',  '9011223344', 'neha@gmail.com'),
(5, 'Karan Patel', '9090909090', 'karan@gmail.com'),
(6, 'Simran Kaur', '9888123456', 'simran@gmail.com');
select*from customers;

insert into rooms (room_id, room_type, price_per_night, status) values
(101, 'Deluxe',   3500.00, 'available'),
(102, 'Standard', 2000.00, 'booked'),
(103, 'Suite',    6000.00, 'available'),
(104, 'Deluxe',   3700.00, 'available'),
(105, 'Standard', 2200.00, 'booked'),
(106, 'Suite',    6500.00, 'available');
select*from rooms;

insert into bookings (booking_id, customer_id, room_id, checkin_date, 
checkout_date, status) values
(1001, 1, 102, '2025-09-12', '2025-09-15', 'confirmed'),
(1002, 2, 101, '2025-09-18', '2025-09-20', 'checked_in'),
(1003, 3, 103, '2025-09-22', '2025-09-25', 'cancelled'),
(1004, 4, 104, '2025-09-19', '2025-09-21', 'confirmed'),
(1005, 5, 105, '2025-09-23', '2025-09-26', 'completed'),
(1006, 6, 106, '2025-09-24', '2025-09-28', 'confirmed');
select*from bookings;

insert into payments (payment_id, booking_id, 
amount, method) values
(5001, 1001, 6000.00,'phone pay'),
(5002, 1002, 7000.00,'card'),
(5003, 1003, 9000.00,'cash'),
(5004, 1004, 7400.00,'phone pay'),
(5005, 1005, 6600.00,'card'),
(5006, 1006, 13000.00,'phone pay');
select*from payments;

insert into tours (tour_id, tour_name,
 price_per_person, capacity) values
(201, 'Scuba Diving', 500.00, 30),
(202, 'Beach Adventure', 1500.00, 20),
(203, 'Mountain Treking', 2500.00, 15),
(204, 'River Rafting', 1800.00, 25),
(205, 'Desert tour', 3000.00, 10),
(206, 'Wildlife tour', 2000.00, 20);
select*from tours;

insert into tour_reservations (reservation_id, customer_id,
 tour_id, pax_count) values
(3001, 1, 201, 2),
(3002, 2, 202, 4),
(3003, 3, 203, 1),
(3004, 4, 204, 3),
(3005, 5, 205, 2),
(3006, 6, 206, 5);
select *from tour_reservations;

insert into change_log (log_id, booking_id, 
old_status, new_status) values
(4001, 1001, 'pending',   'confirmed'),
(4002, 1002, 'confirmed', 'checked_in'),
(4003, 1003, 'confirmed', 'cancelled'),
(4004, 1004, 'pending',   'confirmed'),
(4005, 1005, 'checked_in','completed'),
(4006, 1006, 'pending',   'confirmed');
select*from change_log;


