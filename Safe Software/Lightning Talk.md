# Lightning Talk

Hello, this is Joshua from the python team. In this lightning talk I'm going to present a new format for background maps in Data Inspector, Tiled Web Map XYZ. This new background map format is will ship with FME 2024.2. 

This new format can be used to display background maps from any arbitrary source URL that follows the XYZ placeholder OpenStreetMap standard.

This is what the format looks like when adding a new background map. I'll input a tile URL from openstreetmap that contains the XYZ placeholders. This is a service that Data Inspector currently does not have a built-in integration for.

Next I'll input the appropriate copyright information in the attribution text field.

I can specify the minimum and maximum zoom levels that data inspector will request from the server. I'll just use the defaults values.

To use a service that requires authentication, I can include an API key in the URL. For example, Azure Maps, another new map format for Data Inspector.

I can also include hyperlinks in the attribution text by using html tags like so.

