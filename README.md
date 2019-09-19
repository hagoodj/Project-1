# Project 1: ROUTEstaurants

## About the project:
  ROUTEstaurants displays the user’s intended route and a marker that moves along the route. When the marker’s position is changed, a list of restaurants is returned at the bottom of the screen. 

## Page preview: 
![](screen-shot-1.png)
![](screen-shot-2.png)


## Techologies and Libraries used to build:
  * HTML
  * CSS
  * Bootstrap
  * JavaScript
  * JQuery
  * Google Maps API
  * Open Cage Data API
  * Yelp API
  * AJAX
  * Firebase Realtime Database
  * Font Awesome


## Methodology:
  * Our goal was to create an application where a user can define a starting point and and an ending destination and the applicaton would return restaurants along the way withi a 4 block radius. We planned to leverage Google Maps, Open Cage and Yelp APIs for the information.


## Code Snippets:

Open Cage API:

* The following code snippet uses the Open Cage API to convert addresses and locations entered by the user into latitude and longitude coordinates that the Google API can use to get a route.
```js
$("#routebtn").on("click", function() {
    
    var startLoc = $("#start").val().trim();
    var endLoc = $("#end").val().trim();
    var queryURLStart = "https://api.opencagedata.com/geocode/v1/json?q=" + startLoc + "&key=5de65d8b73e24f3c873540d0515d0f0a";
    var queryURLEnd = "https://api.opencagedata.com/geocode/v1/json?q=" + endLoc + "&key=5de65d8b73e24f3c873540d0515d0f0a";
    var latitudeStart;
    var longitudeStart;
    var latitudeEnd;
    var longitudeEnd;
    
    getLocationData(queryURLStart, queryURLEnd);
});
function getLocationData(URLStart, URLEnd) {
    $.ajax({
    url: URLStart,
    method: "GET"
    })
    .then(function(response) {
        var results = response.results;
        latitudeStart = results[0].geometry.lat;
        longitudeStart = results[0].geometry.lng;
        $.ajax({
        url: URLEnd,
        method: "GET"
        })
        .then(function(response2) {
            var results2 = response2.results;
            latitudeEnd = results2[0].geometry.lat;
            longitudeEnd = results2[0].geometry.lng;
            initialize(latitudeStart, longitudeStart, latitudeEnd, longitudeEnd);
        });
        
    });
    }


Google Maps:

  * This code snippetm shows how the Google Maps API is accessed and used to display the map on the page and it calls another Google Maps function from the documentation.

```js
    function initialize(latStart, lngStart, latEnd, lngEnd) {
        directionsDisplay = new google.maps.DirectionsRenderer();
        map = new google.maps.Map(
            document.getElementById("map_canvas"), {
            center: new google.maps.LatLng(37.4419, -122.1419),
            zoom: 13,
            mapTypeId: google.maps.MapTypeId.ROADMAP
            });
        calcRoute(latStart, lngStart, latEnd, lngEnd);
        directionsDisplay.setMap(map);
    }
```

Yelp API:

  * This code snippet shows the AJAX call to the Yelp API and the response function collecting the various business data values.

  ```js
    $.ajax({
      url: 'https://api.yelp.com/v3/businesses/search?term=' + preference + '&limit=10' + '&radius=400' + '&latitude=' + latinput + '&longitude=' + lnginput,
      method: "GET",
      headers: {
          authorization: "Bearer fK0vJ2qKBkGDlGiX4KDOi2XxOMIpBObt6DSJDg_rFafxhqhwGy-M29URQaFS3V5fYSgCUJRqT2PueV-QOtYFTti2XIXvnULht3c1Gk6zCd81eE7UAG6U4u1GYeR7XXYx"
      }
  }).then(function (response) {
      console.log(response)
      $("#foodContainer").empty();
      for (var i = 0; i < response.businesses.length; i++) {
          $("#foodContainer").append('<div class = "row justify-content-center"><div class = "col-md-6"><img class = "restImage image-fluid" src="' + response.businesses[i].image_url + '"/></div><div class = "col-md-6"><div class = "row"><h2 class="name"><a href = ' + response.businesses[i].url + '>' + response.businesses[i].name + '</a></h2></div><div class = "row"><p class="phone">' + response.businesses[i].display_phone + '</p></div><div class = "row"><p class="address">' + response.businesses[i].location.address1 + ', ' + response.businesses[i].location.city + ' ' + response.businesses[i].location.zip_code + '</p></div></div><hr></div>')
      }
    });
  }
  ```

Firebase:

  * This code snippet shows the capture of the user preference data and then pushing that data into the firebase real-time database. Using the firebase shapshot method, the data is returned back to the DOM

  ```js
    $('#routebtn').on('click', function(event) {
    event.preventDefault();
    var prefInput = $('#pref-input').val().trim();
    var addPreference = {
        preference: prefInput,
    };
    database.ref().push(addPreference);
    console.log(addPreference.preference)
    $('#pref-input').val('');
    });
    database.ref().on('child_added', function(childSnapshot) {
    console.log(childSnapshot.val());
    var prefInput = childSnapshot.val().preference;
    console.log(prefInput);
    var tableRow = $('<tr>');
    tableRow.append(
        $('<td>').text(prefInput),
    )
    var preferences = $('#for-append');
    preferences.append(tableRow);
});
```

## Author Links:

Jordan Hagood
https://github.com/hagoodj

Ron Melendres 
https://github.com/RonMelendres

Raj Sodhi 
https://github.com/Rajsodhi1

Oliver Sun
https://github.com/Olisun



