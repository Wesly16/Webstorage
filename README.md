# index.html

<!DOCTYPE html>
<html>
  <head>
    <title>Form Data Storage</title>
    <link rel="stylesheet" href="/style.css" />
  </head>
  <body>
    <form id="myForm">
      <label for="firstName">First Name:</label>
      <input type="text" id="firstName" name="firstName" /><br /><br />

      <label for="lastName">Last Name:</label>
      <input type="text" id="lastName" name="lastName" /><br /><br />

      <label for="gender">Gender:</label>git push -u origin main
      <select id="gender" name="gender">
        <option value="male">Male</option>
        <option value="female">Female</option></select
      ><br /><br />

      <label for="address">Address:</label>
      <textarea id="address" name="address"></textarea><br /><br />

      <label for="storageType">Storage Type:</label>
      <select id="storageType" name="storageType">
        <option value="session">Session Storage</option>
        <option value="cookie">Cookie Storage</option></select
      ><br /><br />

      <button type="button" onclick="saveData()">Save Data</button>
    </form>

    <button type="button" onclick="displayData()">Display Data</button>
    <button type="button" onclick="deleteData()">Delete Data</button>

    <div id="data"></div>

    <script src="script.js"></script>
  </body>
</html>

# Style.css
/* Set font and color for headings */
h1 {
  font-family: Arial, sans-serif;
  color: #333;
}

/* Style form elements */
label {
  display: block;
  font-family: Arial, sans-serif;
  font-weight: bold;
  margin-bottom: 5px;
}

input[type="text"],
select,
textarea {
  font-family: Arial, sans-serif;
  font-size: 14px;
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-sizing: border-box;
  width: 100%;
  margin-bottom: 10px;
}

select option {
  font-family: Arial, sans-serif;
}

input[type="submit"] {
  background-color: #824caf;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-family: Arial, sans-serif;
  font-size: 14px;
  padding: 10px 20px;
}

input[type="submit"]:hover {
  background-color: #824caf;
}

/* Style buttons */
button {
  background-color: #6e6075;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-family: Arial, sans-serif;
  font-size: 14px;
  margin-bottom: 15px;
  padding: 15px 20px;
  margin-right: 10px;
}

button:hover {
  background-color: #824caf;
}

/* Style data output */
#data {
  font-family: Arial, sans-serif;
  font-size: 14px;
  margin-top: 20px;
}

#data br {
  display: none;
}

# script.js
// Array to store form data
var formDataArray = [];

// Function to save form data
function saveData() {
  // Get form values
  var firstName = document.getElementById("firstName").value;
  var lastName = document.getElementById("lastName").value;
  var gender = document.getElementById("gender").value;
  var address = document.getElementById("address").value;
  var storageType = document.getElementById("storageType").value;

  // Create data object
  var data = {
    firstName: firstName,
    lastName: lastName,
    gender: gender,
    address: address,
  };

  // Save data to array
  formDataArray.push(data);

  // Save data to storage
  if (storageType === "session") {
    sessionStorage.setItem("formData", JSON.stringify(formDataArray));
  } else if (storageType === "cookie") {
    document.cookie = "formData=" + JSON.stringify(formDataArray);
  }

  // Reset form
  document.getElementById("myForm").reset();
}

// Function to delete data
function deleteData() {
  sessionStorage.removeItem("formData");
  document.cookie = "formData=; expires=Thu, 01 Jan 1970 00:00:00 UTC;";

  // Clear displayed data
  document.getElementById("data").innerHTML = "";
}

// Function to display data
function displayData() {
  var storedData = sessionStorage.getItem("formData") || getCookie("formData");

  if (storedData) {
    var data = JSON.parse(storedData);

    var output = "";

    for (var i = 0; i < data.length; i++) {
      output += "Data " + (i + 1) + ":<br>" + "First Name: " + data[i].firstName + "<br>" + "Last Name: " + data[i].lastName + "<br>" + "Gender: " + data[i].gender + "<br>" + "Address: " + data[i].address + "<br><br>";
    }

    document.getElementById("data").innerHTML = output;
  } else {
    document.getElementById("data").innerHTML = "No data found.";
  }
}

function getCookie(name) {
  var cookieArr = document.cookie.split(";");

  // Loop through cookies to find the one with the specified name
  for (var i = 0; i < cookieArr.length; i++) {
    var cookiePair = cookieArr[i].split("=");
    if (name === cookiePair[0].trim()) {
      return decodeURIComponent(cookiePair[1]);
    }
  }

  // If cookie not found, return null
  return null;
}

