Local Food Waste Reduction Platform
A Distributed Client-Server System for Food Waste Reduction. 

This platform connects local businesses with surplus food from restaurants, stores and bakeries to community groups for pickup, built to demonstrate core concepts of Distributed Systems.

Key Features:
Scalability: Achieved through Horizontal Scaling (multiple Flask instances) and Asynchronous Processing via Celery/Redis.

Fault Tolerance: Ensured by Database Replication (PostgreSQL) and Decoupling the Web App from slow tasks.

Collaboration: Collaboration between businesses and client , where the client claim a listed item to the business(restaurant,Bakeries) for physcal exchange.
