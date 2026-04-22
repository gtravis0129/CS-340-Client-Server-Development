# CS-340-Client-Server-Development
 
About the Project 

This project is a full-stack web application dashboard built for Grazioso Salvare, an international rescue animal training company. The dashboard allows staff to interact with Austin Animal Center outcomes data stored in a MongoDB database, filter dogs by rescue type, and visualize the results through an interactive data table, a geolocation map, and a breed distribution pie chart. The application follows the Model-View-Controller design pattern, where MongoDB serves as the model, the Dash framework provides the view and controller structure, and a Python CRUD module handles all database interactions. 

Motivation 

This project was created to practice implementing full-stack client/server development using MongoDB and Python. Grazioso Salvare needs a tool to quickly identify dogs that are strong candidates for specific types of search-and-rescue training. Different rescue types require different breeds, age ranges, and sex profiles. This dashboard allows staff to apply a filter with a single click and instantly see only the dogs that match the training criteria, along with their geographic location and a breakdown of breeds in the filtered results. 

 

Tools Used 

The following tools and technologies were used to build this project: 

MongoDB 7.0 — Document-oriented NoSQL database used to store and query the Austin Animal Center outcomes data. MongoDB was chosen because it stores data as flexible JSON-like documents that map naturally to Python dictionaries. It also supports powerful query operators such as $in, $gte, and $lte, which are essential for the breed and age-range filtering required by this project. These qualities make MongoDB well suited for interfacing with Python in a client/server application. 

PyMongo — The official Python driver for MongoDB. PyMongo provides the MongoClient class and all CRUD operation methods used in the AnimalShelter module. 

Python 3 — The primary programming language for both the CRUD module and the dashboard code. 

Dash / JupyterDash — The Dash framework by Plotly provides the view and controller structure for the web application dashboard. JupyterDash allows the Dash app to run directly inside Jupyter Lab in Codio. Dash uses callback functions to connect interactive components such as radio buttons and the data table to output widgets such as charts and the map, implementing the controller layer of the MVC pattern. 

Plotly Express — Used to generate the breed distribution pie chart that responds dynamically to the filter selection. 

Dash Leaflet — Used to render the interactive geolocation map with OpenStreetMap tiles. The map displays a marker at the coordinates of the selected animal in the data table. 

Pandas — Used to convert MongoDB query results into DataFrames for manipulation and display in the data table. 

Jupyter Lab (Codio) — The development and execution environment for the dashboard notebook. 

Resources: 

Dash documentation: https://dash.plotly.com 

PyMongo documentation: https://pymongo.readthedocs.io 

MongoDB documentation: https://www.mongodb.com/docs 

Austin Animal Center dataset: https://doi.org/10.26000/025.000001 

 

How to Reproduce This Project 

Step 1 - Import the data into MongoDB: 

cd /usr/local/datasets 

mongoimport --type=csv --headerline --db aac --collection animals --drop ./aac_shelter_outcomes.csv 

Step 2 - Create the aacuser account in MongoDB: 

mongosh 

use admin 

db.createUser({ user: "aacuser", pwd: "Gary.Travis", roles: [{ role: "readWrite", db: "aac" }] }) 

Step 3 - Open the project in Jupyter Lab. Navigate to the code_files directory and open ProjectTwoDashboard.ipynb. 

Step 4 - Run the notebook cell. The Dash app will start and display a URL in the output. Click the URL to open the dashboard in your browser. 

Step 5 - Test the filters by clicking each radio button. Verify that the data table, pie chart, and geolocation map all update to reflect the filtered results. Click individual rows in the data table to move the map marker to that animal's location. 

 

Challenges and Solutions 

The most significant challenge encountered during this project was MongoDB authentication. The Codio environment runs MongoDB without access control enabled, which meant that standard username and password authentication failed even after the aacuser account was created. This was resolved by modifying the CRUD_Python_Module.py constructor to connect using a direct MongoClient connection, while still accepting username and password as constructor parameters to maintain compatibility with the assignment requirements. 

A second challenge was the _id column returned by MongoDB, which contains ObjectId objects that are not JSON-serializable and caused the data table to crash. This was resolved by dropping the _id column from the DataFrame immediately after each query using df.drop(columns=['_id'], inplace=True). 

A third challenge was ensuring the pie chart and geolocation map both updated correctly when a new filter was applied. This was resolved by connecting both chart callbacks to the derived_virtual_data output of the DataTable, so that all three output widgets stay in sync through the data table as the single source of truth. 
