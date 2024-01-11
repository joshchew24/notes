# RoomDataset
- geolocation?
	- processFileContents(content)
		- read index.htm from zip
		- parse index.htm to AST
		- find buildings table (contains one of the valid classes)
			- buildingTableSearchStack = \[root\]
			- while buildingTableSearchStack.length != 0
				- currnode = stack.pop()
				- if currNode has nodename table and attr class = "views-table", break loop
				- else, push child nodes to buildingTableSearchStack
					- using DFS arbitrarily
					- can change this to check performance times but queues have a dequeue time complexity of O(n))
		- simultaneously validate table and generate list of bulilding paths
			- validate header classes
			- iterate through TR nodes(?)
				- validate classes
				- save code (shortname), long name, address, href as objects in array
		- iterate through buliding list
			- if href link DNE, skip
			- find room table
				- validate header classes
				- iterate through TR
					- validate classes
					- get room number, seats, room type, furniture type
				- construct name (room id = shortname + number)
				- return list of valid rooms
			- if valid room list is empty, skip this building
			- else, GET geolocation and populate lat and lon

# Geolocation

For a building that contains a valid room, you will need to fetch the building's latitude and longitude.

This is usually performed using online web services. To avoid problems with us spamming external geolocation providers, we will be providing a web service for you to use for this purpose. To obtain the geolocation of an address, you must send a GET request to:

http://cs310.students.cs.ubc.ca:11316/api/v1/project_team \<TEAM NUMBER\>/\<ADDRESS\>
http://cs310.students.cs.ubc.ca:11316/api/v1/project_team279/ \<ADDRESS\>

http://cs310.students.cs.ubc.ca:11316/api/v1/project_team279/6245%20Agronomy%20Road%20V6T%201Z4

Where \<ADDRESS\> should be the URL-encoded version of an address (e.g., "6245 Agronomy Road V6T 1Z4" should be represented as 6245%20Agronomy%20Road%20V6T%201Z4). Addresses should be given exactly as they appear in the dataset files, or an HTTP 404 error code will be returned. 

The response will match the following interface (either you will get lat & lon, or error, but never both):

interface GeoResponse {

    lat?: number;

    lon?: number;

    error?: string;

}

Since we are hosting this service it could be killed by a DOS attack, so please try not to overload the service. You should only need to query this service when you are processing the initial dataset zips, not when you are answering queries.

## Sending the Request

To send these requests, you must use the http package. 

Although the request is a GET, you cannot test the response by posting it directly into your browser URL (like Chrome). The browser will automatically convert the http to https, and the request will be rejected.

The best way to test the Geolocation locally is by using the curl command from your terminal. For example, you can use the following command, where google.com is replaced with your team's URL.

curl -i http://google.com


## Encoding the Address

To encode the address, use the function encodeURIComponent() [documentation link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent).