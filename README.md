
CMPE273-Fall15-Assignment3

Description: Part II - Trip Planner

The trip planner is a feature that will take a set of locations from the database and will then check against UBER’s price estimates API to suggest the best possible route in terms of costs and duration.

POST        /trips   # Plan a trip

Request:
{
   
    "starting_from_location_id: "999999",
    "location_ids" : [ "10000", "10001", "20004", "30003" ] 
    
}

The response should sort the locations on their uber cost and duration and create a "best_route_location_ids" array.

Response: HTTP 201
{

     "id" : "1122",
     “status” : “planning”,
     "starting_from_location_id: "999999",
     "best_route_location_ids" : [ "30003", "10001", "10000", "20004" ],
     "total_uber_costs" : 125,
     "total_uber_duration" : 640,
     "total_distance" : 25.05 
  
}


GET        /trips/{trip_id} # Check the trip details and status
        
Request:  GET             /trips/1122

Response:
{

     "id" : "1122",
     "status" : "planning",
     "starting_from_location_id: "999999",
     "best_route_location_ids" : [ "30003", "10001", "10000", "20004" ],
     "total_uber_costs" : 125,
     "total_uber_duration" : 640,
     "total_distance" : 25.05 
     
}

PUT        /trips/{trip_id}/request # Start the trip by requesting UBER for the first destination. 
Calling all UBER request API to request a car from starting point to the next destination. Once a destination is reached, the subsequent call the API will request a car for the next destination.

Request:  PUT             /trips/1122/request

Response:
{

     "id" : "1122",
     "status" : "requesting",
     "starting_from_location_id”: "999999",
     "next_destination_location_id”: "30003",
     "best_route_location_ids" : [ "30003", "10001", "10000", "20004" ],
     "total_uber_costs" : 125,
     "total_uber_duration" : 640,
     "total_distance" : 25.05,
     "uber_wait_time_eta" : 5 
     
}

Once all the destinations are visited the status is upated as "completed" and any other subsequent PUT requests will not change the state.

Response:
{

     "id" : "1122",
     "status" : "completed",
     "starting_from_location_id”: "999999",
     "next_destination_location_id”: "",
     "best_route_location_ids" : [ "30003", "10001", "10000", "20004" ],
     "total_uber_costs" : 125,
     "total_uber_duration" : 640,
     "total_distance" : 25.05,
     "uber_wait_time_eta" : 5 
     
}
