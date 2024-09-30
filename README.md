# -Readme.md-
This project is a simple web application that allows users to enter an IP address and fetch its geographical location using the IP2Location Web Service.
ip2location-project/
├── src/
│   ├── index.html        # Main HTML file
│   ├── script.js         # JavaScript file for API calls
│   ├── style.css         # CSS for styling
│   ├── error.html        # Error handling page
│   └── history.json      # Store search history
├── README.md             # Project description and instructions
└── LICENSE               # Open source license (e.g., MIT)

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IP2Location Finder</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>IP Location Finder</h1>
    <input type="text" id="ipInput" placeholder="Enter IP Address">
    <button id="submitBtn">Get Location</button>
    <div id="loading" class="hidden">Loading...</div>
    <div id="result"></div>
    <h2>Search History</h2>
    <ul id="history"></ul>
    <script src="script.js"></script>
</body>
</html>

document.addEventListener('DOMContentLoaded', function() {
    loadHistory();

    document.getElementById('submitBtn').addEventListener('click', function() {
        const ip = document.getElementById('ipInput').value;
        if (!validateIP(ip)) {
            displayError("Invalid IP Address");
            return;
        }
        fetchLocation(ip);
    });
});

function validateIP(ip) {
    const regex = /^(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$/;
    return regex.test(ip);
}

function fetchLocation(ip) {
    const apiKey = '2B119C358F19C2C10999B3E2EE1B9152'; // Replace with your IP2Location API key
    const apiUrl = `https://api.ip2location.com/v2/?ip=${ip}&key=${apiKey}&package=WS1&format=json`;
    
    showLoading();

    fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
            if (data.response !== 'OK') {
                displayError(data.message);
                return;
            }
            displayResult(data);
            saveHistory(ip);
        })
        .catch(error => displayError("Error fetching data."));
}

function displayResult(data) {
    const resultDiv = document.getElementById('result');
    resultDiv.innerHTML = `
        <h2>Location Details:</h2>
        <p>Country: ${data.country_name}</p>
        <p>Region: ${data.region_name}</p>
        <p>City: ${data.city_name}</p>
        <p>ISP: ${data.isp}</p>
    `;
    hideLoading();
}

function displayError(message) {
    const resultDiv = document.getElementById('result');
    resultDiv.innerHTML = `<p style="color:red;">${message}</p>`;
    hideLoading();
}

function showLoading() {
    document.getElementById('loading').classList.remove('hidden');
}

function hideLoading() {
    document.getElementById('loading').classList.add('hidden');
}

function saveHistory(ip) {
    let history = JSON.parse(localStorage.getItem('ipHistory')) || [];
    if (!history.includes(ip)) {
        history.push(ip);
        localStorage.setItem('ipHistory', JSON.stringify(history));
        loadHistory();
    }
}

function loadHistory() {
    const historyDiv = document.getElementById('history');
    const history = JSON.parse(localStorage.getItem('ipHistory')) || [];
    historyDiv.innerHTML = '';
    history.forEach(ip => {
        const li = document.createElement('li');
        li.textContent = ip;
        li.addEventListener('click', () => {
            document.getElementById('ipInput').value = ip;
            fetchLocation(ip);
        });
        historyDiv.appendChild(li);
    });
}


body {
    font-family: Arial, sans-serif;
    text-align: center;
    padding: 20px;
}

input {
    padding: 10px;
    margin: 10px;
    width: 200px;
}

button {
    padding: 10px 15px;
    cursor: pointer;
}

#result {
    margin-top: 20px;
}

#loading {
    margin-top: 20px;
    font-size: 18px;
}

.hidden {
    display: none;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    cursor: pointer;
    color: blue;
    text-decoration: underline;
}



<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Error</title>
</head>
<body>
    <h1>Error</h1>
    <p>Sorry, there was an error fetching the location data. Please try again later.</p>
    <a href="index.html">Go Back</a>
</body>
</html>


# IP2Location Example

This project is a simple web application that fetches location details based on a given IP address using the IP2Location Web Service.

## How to Use

1. Clone the repository to your local machine.
2. Open `src/index.html` in a web browser.
3. Enter an IP address in the input box and click "Get Location".
4. The location details will be displayed below.
5. Click on any IP in the search history to fetch its details again.

## Requirements

- A valid IP2Location API key (replace `YOUR_API_KEY` in `src/script.js`).

## License

This project is licensed under the MIT License.


MIT License

Copyright (c) [ip2location] [2024]

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

...

[Include rest of MIT license text]

