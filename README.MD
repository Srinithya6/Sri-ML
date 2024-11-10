# Bus Journey Booking Application

This Python application interacts with a MySQL database to retrieve customer journey information based on a stored procedure. The application uses a command-line interface, allowing users to connect to the database, fetch journey details for specific customers, and display the results in a tabular format.

## Host Language

The primary programming language used is **Python**. MySQL is used as the database system.

## Prerequisites

Ensure you have the following installed:
- **Python 3.x**
- **MySQL Database Server** (version 8.0 or later is recommended)
- **Python Packages**:
  - `pymysql` - To interact with the MySQL database
  - `tabulate` - To format output into tables
  - `cryptography` - To support certain MySQL authentication methods

Install the required Python packages using:
```bash
pip install pymysql tabulate cryptography
```


## Database Setup

1. Create the Database and Tables
Use the provided SQL dump to create the bus_schema database and its tables. Run the SQL script in your MySQL environment to set up the tables, insert sample data, and establish necessary constraints.

2. Create the get_customer_journeys stored procedure in the bus_schema database to retrieve customer journey information by running the following SQL commands in your MySQL environment:

```bash
DELIMITER //
CREATE PROCEDURE get_customer_journeys(IN username_p VARCHAR(64))
BEGIN
    SELECT c.username, c.first_name, c.last_name, b.payment_method, b.seats_booked, ri.fare_price, ri.scheduled_date_time, 
        ri.r_instance, r.origin_city_name, r.origin_state, r.destination_city_name, r.destination_state, bt.type AS bus_type, 
        bt.model AS bus_model, bt.license_plate, ri.bus_id
    FROM bus_journey_booking AS b
    JOIN route_instance AS ri ON b.route_instance = ri.r_instance
    JOIN route AS r ON ri.route_id = r.route_id
    JOIN bus_type AS bt ON ri.bus_id = bt.bus_id
    JOIN customer AS c ON b.username = c.username
    WHERE c.username = username_p
    ORDER BY ri.scheduled_date_time, r.origin_city_name;
END //
DELIMITER ;
```
This procedure retrieves journey information for a specific customer, ordered by the scheduled date and time, followed by the origin city.

## Running the Application
1. Start the MySQL Database: Ensure your MySQL server is running on localhost with a user who has access to the bus_schema database.

2. Run the Python Script: In in VSCode or PyCharm, execute the Python script from the command line in the directory where the script is located:

```bash
python hw8_busschema.py
```

## Enter Database Credentials:

When prompted, enter your MySQL username and password.
If the credentials are correct, you’ll see a confirmation message that you've successfully connected to the database.
If the credentials are incorrect, an error message will prompt you to re-enter valid credentials.

If the credentials are correct, the application will ask to select an option

Menu Options:

- 1: Generate a list of journeys for a specific user
- 2: Disconnect and close the application
- Select an option (1 or 2): 

- Option 1: Fetching Customer Journeys
- Choose option 1 from the main menu.
- Enter a valid username (e.g., cust3).
- The application will call the get_customer_journeys procedure and display journey details in a tabular format.

- Option 2: Disconnect and close the application.
The application will be disconnected .

- If invalid menu input is given, then the application will ask to select the valid input i.e., 1 or 2
- If invalid customer username is given, then the application will ask to select the valid customer usernames from list


## Cryptography Module Error
If you see an error about missing cryptography for authentication, ensure that the cryptography package is installed. Install it using:
```bash
pip install cryptography
```

## MySQL Errors
If the stored procedure fails, check:

- the bus_schema database and tables are correctly set up.
- the get_customer_journeys procedure exists and is defined as per the instructions.
