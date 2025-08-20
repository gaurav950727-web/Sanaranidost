sanatanidost
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Learn About Countries</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 30px; background: #f3f3f3; }
        h1 { font-size: 2rem; }
        .container { background: #fff; padding: 20px; border-radius: 8px; max-width: 500px; margin: auto; }
        #result { margin-top: 20px; }
        img { width: 80px; }
        input, button { padding: 8px; font-size: 1rem; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Country Facts Generator</h1>
        <p>Enter a country name to learn about it:</p>
        <input type="text" id="countryInput" placeholder="eg. India, France, USA" />
        <button onclick="getCountryInfo()">Search</button>
        <div id="result"></div>
    </div>

    <script>
    function getCountryInfo() {
        const name = document.getElementById('countryInput').value.trim();
        const result = document.getElementById('result');
        if (!name) {
            result.innerHTML = '<p>Please enter a country name.</p>';
            return;
        }
        fetch(`https://restcountries.com/v3.1/name/${name}`)
            .then(res => res.json())
            .then(data => {
                if (!data || data.status === 404) {
                    result.innerHTML = '<p>No results found.</p>';
                    return;
                }
                const country = data[0];
                result.innerHTML = `
                    <h2>${country.name.common}</h2>
                    <img src="${country.flags.svg}" alt="Flag of ${country.name.common}">
                    <p><strong>Capital:</strong> ${country.capital ? country.capital : 'N/A'}</p>
                    <p><strong>Population:</strong> ${country.population.toLocaleString()}</p>
                    <p><strong>Region:</strong> ${country.region}</p>
                    <p><strong>Currency:</strong> ${country.currencies ? Object.values(country.currencies).name : 'N/A'}</p>
                    <p><strong>Languages:</strong> ${country.languages ? Object.values(country.languages).join(', ') : 'N/A'}</p>
                `;
            })
            .catch(() => {
                result.innerHTML = '<p>Error fetching data. Please check the country name and try again.</p>';
            });
    }
    </script>
</body>
</html>
