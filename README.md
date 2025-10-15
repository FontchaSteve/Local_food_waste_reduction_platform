Local Food Waste Reduction Platform:
The local food waste reduction platform is a distributed client-server application designed to address the dual challenges of commecial food waste and food insecurity in Cameroon. The project’s primary academic objective is to archive  and implement a system demonstrating scalability, fault tolerance.The system facilitates collaboration between authenticated Businesses (Donors) and registered Clients (claimers/community groups) through a transaction-based model, underpinned by python, flask,  docker orchestration. The technical design prioritizes resilience and high performance, essential attributes for a reliable social utility platform operating in a challenging logistical environment.

The local food waste reduction platform serves as a vital, zero-cost logistical and transactional bridge. It transforms waste into a resource by providing a reliable and instantaneous communication channel:

*The Platform’s Mission

-For Businesses: It offers a compliant, efficient, and direct method for managing food surplus, transferring it directly to those who need it, thus fulfilling social responsibility.

For Clients: It provides reliable, real-time access to available food resources, enhancing the capacity of community groups to serve their beneficiaries.

Core collaboration mechanism and user roles
The platform is designed around a secure, two-sided transaction model that enforces authentication and authorization, crucial for both security and logistical planning.

 *User Roles and Authorization
 
The system defines two distinct and mandatory user types, each requiring a separate interface and set of permissions:
The Business (Donor):
Permission: Authorized to access the route to create new listings.
Data Required: Registration includes business name, location (for pickup), email, and phone number.
The Client (Claimer):
Permission: Authorized to view the dashboard and initiate the Claim transaction.
Data Required: Registration includes contact name, email, and phone number (for receiving logistical confirmation).

*The listing and claim transaction

Listing Creation: A Business uploads details of the surplus food, including item name, quantity, a description, the available until datetime, and uploads a reference image. This listing is immediately recorded in the PostgreSQL database with a status of 'Available'.

The Claim: A Client views the listing and initiates the claim. This is a critical atomic transaction executed by the Web Service:

The Listing status is instantly updated from 'Available' to 'Claimed'.
A new record is created in the claim table, linking the client id to the listing id and timestamping the transaction.

*Fault Tolerance and Resilience
A reliable social service platform must be fault-tolerant, meaning system operations continue and data is protected even when components fail.

*Redundancy in frontend services
By running multiple Web service containers, the system achieves dervice redundancy. If one Web container crashes, the implied load balancer instantly routes new requests to the surviving Web containers. From the user's perspective, the application remains fully available, demonstrating continuous uptime.


*Technology Versioning (requirements.txt):
- Frameworks: Flask, SQLAlchemy (ORM)
= Utilities: psycopg2-binary (PostgreSQL driver), passlib (security for hashing passwords), flask-mail (integration for email sending).

* Database Schema (app/models.py)
The data structure is built using SQLAlchemy's declarative base, enforcing strong relationships between the three core tables:
- User:Manages authentication, authorization, and contact details.
username, password_hash, user_type (Enum: BUSINESS, CLIENT).
-Listing :Stores the surplus food donation details.
business_id (FK to User), image_filename, status (Enum: AVAILABLE, CLAIMED).
-Claim: Records the immutable transaction between the Client and the Listing.
The core dependencies are locked to specific versions to ensure environment reproducibility across development and deployment containers:

* horizontal scaling (web service):
- Load Balancing : By configuring the docker-compose.yml to run multiple identical web services (e.g., replicas: 3), we simulate a production environment where a load balancer distributes traffic across these instances. This is Horizontal Scalings handling increased load by adding more machines (containers),. not by making one machine more powerful. This ensures the service can handle numerous simultaneous users browsing and logging in during peak hours.

*data integrity and high availability (PostgreSQL)
The persistence layer (PostgreSQL) is the most critical component. While docker-compose only deploys a single instance for simplicity, the design conceptually supports advanced fault tolerance.
onceptual Replication: In a production environment, this PostgreSQL instance would be configured for Asynchronous Replication (Primary-Replica setup). If the Primary Database instance fails, a Replica can be instantly promoted to Primary, taking over the load. This ensures Data availability  and prevents the entire application from grinding to a halt due to a single point of database failure
,
The Local food waste eeduction platform successfully realizes a complex distributed architecture to solve a direct social challenge in Cameroon. his technical foundation allows the platform to be scaled significantly to accommodate growing user numbers and increasing transaction volumes, securing its role as a sustainable logistical solution for food waste management and poverty alleviation.
