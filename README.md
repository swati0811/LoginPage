The code is written to login to a webpage using google or facebook.
I have used bootstrap to change the css of some elements.

For google sign-in:
 
<meta name="google-signin-client_id" content="YOUR_CLIENT_ID.apps.googleusercontent.com">
This is in the header part with my client-id which I generated from Google Developer Console.(https://console.cloud.google.com/home/dashboard?project=upbeat-climate-343322)

<div class="g-signin2" data-onsuccess="onSignIn"></div>
This is in the body section to get the sign-in button

<div class="data">  
          <p>Name</p>
          <p id="name" class="alert alert-success"></p>
          <p>Image</p>
          <img id="image" class="rounded-circle" width="100" height="100" />
          <p>Email</p>
          <p id="email" class="alert alert-danger"></p>
          <button type="button" class="btn btn-danger" onclick="signOut();">Sign Out</button>
      </div>
 This is written in body section to retrieve profile information for a user and get the sign out button.
 
 function onSignIn(googleUser) {
    var profile = googleUser.getBasicProfile(); //get data of the user after logging in
    $("#name").text(profile.getName());
    $("#email").text(profile.getEmail());
    $("#image").attr('src', profile.getImageUrl());
    $(".data").css("display", "block");
    $(".g-signin2").css("display", "none");    
     $(".fb-login-button").css("display", "none");   
}
This is in the javascript file to get data of the user after logging in with the use of jQuery.

function signOut() {
    var auth2 = gapi.auth2.getAuthInstance();
    auth2.signOut().then(function () {
        alert("You have been signed out successfully"); //alert after signing out
        $(".data").css("display", "none");
        $(".g-signin2").css("display", "block");       //displaying the hidden button
         $(".fb-login-button").css("display", "block");
    });
}
To create a sign-out link, attach a function that calls the GoogleAuth.signOut() method to the link's onclick event.
This is in the javascript file for the functions after logging out with the use of jQuery.

For Facebook login :

<div id="fb-root"></div>
<script async defer crossorigin="anonymous" src="https://connect.facebook.net/en_GB/sdk.js#xfbml=1&version=v13.0&appId=300248888872360&autoLogAppEvents=1" 
        nonce="jLjUd6wf"></script>  
It include the JavaScript SDK on the html page.
We enter the api-id generated from facebook developers console.

div class="fb-login-button" data-width="" data-size="large" data-button-type="login_with" data-layout="default" 
          data-auto-logout-link="true" data-use-continue-as="false"></div>
This is written in the body part where the button is to be inserted.

<script async defer crossorigin="anonymous" src="https://connect.facebook.net/en_US/sdk.js"></script>
Loads the JS SDK asynchronously

function statusChangeCallback(response) { 
    console.log('statusChangeCallback');
    console.log(response);                   
    if (response.status === 'connected') {   
      testAPI();  
    } else {                                 
      document.getElementById('status').innerHTML = 'Please log ' +
        'into this webpage.';
    }
  }
  The results from FB.getLoginStatus() is used to call the above function and console logged the current status.
  If the user is logged into the webpage, testAPI() function is executed or else a message is displayed.
  
  function checkLoginState() {               
    FB.getLoginStatus(function(response) {   
      statusChangeCallback(response);
    });
  }
  This function is called when the user has pressed the login button.
  
 window.fbAsyncInit = function() {
    FB.init({
      appId      : '300248888872360',
      cookie     : true,                   
      xfbml      : true,                     
      version    : 'v13.0'          
    });
   The api-id is entered along with the Graph API version for this call.
   
   FB.getLoginStatus(function(response) {   
      statusChangeCallback(response);       
    });
  };
  This function is called after the JS SDK has been initialized and the statusChangeCallback(response) function retuens the login status.
  
  function testAPI() {                                       
    console.log('Welcome!  Fetching your information.... ');
    FB.api('/me', function(response) {
      console.log('Successful login for: ' + response.name);
      document.getElementById('status').innerHTML =
        'Thanks for logging in, ' + response.name + '!';
    });
  }
This function tests the Graph Api after login and statusChangeCallback() for when this call is made and accordingly statements are console logged along with the the name of the user.
