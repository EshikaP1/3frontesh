---
layout: default
title: Titanic
---
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
<h3>Get Survival Rate</h3>

<h3>What is the Actual Weight?</h3>
<form id="formToGetSurvivalRate">
    <label for="name"><b>name:</b> </label>
     <input type="text" id="name" name="name"/> <br /><br />
    <label for="pclass"><b>pclass:</b> </label>
    <input type="text" id="pclass" name="pclass" /><br /><br />
    <label for="sex"><b>sex:</b> </label>
    <input type="text" id="sex" name="sex" /><br /><br />
    <label for="age"><b>age:</b> </label>
    <input type="text" id="age" name="age" /><br /><br />
    <label for="sibsp"><b>sibsp:</b> </label>
    <input type="text" id="sibsp" name="sibsp" /><br /><br />
    <label for="parch"><b>parch:</b> </label>
    <input type="text" id="parch" name="parch" /><br /><br />
    <label for="fare"><b>fare:</b> </label>
    <input type="text" id="fare" name="fare" /><br /><br />
    <label for="embarked"><b>embarked:</b> </label>
    <input type="text" id="embarked" name="embarked" /><br /><br />
    <label for="alone"><b>alone:</b> </label>
    <input type="text" id="alone" name="alone" /> <br /><br />
    <button type="submit" value="btnToGetSurvivalRate" id="get_SurvivalRate">GetSurvivalRate</button>
    <div id="getSurvivalRate"></div>
</form>
<br><br><br>

<script type="module">
    const createbutton = document.getElementById("get_SurvivalRate");
    createbutton.addEventListener("click", Predict);
    import { uri, options } from '{{site.baseurl}}/assets/js/api/config.js';
    function Predict(event) {
        const API_URL = uri + '/api/titanic/';
        event.preventDefault();
        //const formData = new FormData(event.target);
        //const drink = formData.get("Drink");
        //console.log("got form data Drink")
        //const calories = formData.get("Calories");

        const form = document.getElementById('formToGetSurvivalRate');
        const name = form.elements['name'].value;
        const pclass = form.elements['pclass'].value;
        const sex = form.elements['sex'].value;
        const age = form.elements['age'].value;
        const sibsp = form.elements['sibsp'].value;
        const parch = form.elements['parch'].value;
        const fare = form.elements['fare'].value;
        const embarked = form.elements['embarked'].value;
        const alone = form.elements['alone'].value;
        //const Actual_Weight = parseInt(form.elements['Actual Weight'].value)
        const payload = {
            name,
            pclass,
            sex,
            age,
            sibsp,
            parch,
            fare,
            embarked,
            alone
        };
        console.log('payload');

        const authOptions = { 
            ...options,
            method: 'POST', 
            cache: 'no-cache', 
            body: JSON.stringify(payload),
            headers: {
                'Content-Type' : 'application/json',
                'Access-Control-Allow-Origin': 'include'
            }
        };
        fetch(API_URL, {
            method: "POST",
            headers: {
                "Content-Type": "application/json",
            },
            body: JSON.stringify(payload),
        })
        .then(response => {
            if (!response.ok) {
                const errorMsg = 'Login error: ' + response.status;
                console.log(errorMsg);
                document.getElementById("error-message").innerText = errorMsg; // Display error on the screen
                return;
            } else {
                return response.json()
            }
        })
        .then((data) => {
            console.log(data) 
                if (data==0) {
                    data = "Did not survive";
                } else {
                data = "Survived";
                }
            getSurvivalRate.textContent = data
        })
        .catch((error) => console.error("Error:", error))
    }
    </script>
<style>
    /* ... (existing styles) ... */
    /* Add styles for the table cause its better that way*/
    #DrinkTable {
        width: 100%;
        border-collapse: collapse;
        margin-top: 20px;
    }
    #DrinkTable th, #DrinkTable td {
        border: 1px solid #ddd;
        padding: 8px;
        text-align: left;
    }
    #DrinkTable th {
        background-color: #f2f2f2;
    }
    /* Add styles for the background */
    body {
        background-color: #e0e0e0; /* Set your desired background color */
        font-family: Arial, sans-serif; /* Set your preferred font */
    }
    /* Adjust the modal styles */
    .modal-backdrop {
        /* ... (existing styles) ... */
    }
    .modal-content {
        /* ... (existing styles) ... */
        color: white; /* Set the text color inside the modal */
    }
    /* Add styles for form labels */
    form label {
        font-weight: bold;
        margin-bottom: 5px;
    }
    /* Add styles for buttons */
    button {
        background-color: #4caf50; /* Green background color */
        color: white;
        padding: 10px 15px;
        border: none;
        border-radius: 5px;
        cursor: pointer;
    }
    /* Style the edit and delete buttons in the table */
    #DrinkTable button {
        background-color: #2196F3; /* Blue background color */
        color: white;
        padding: 5px 10px;
        margin: 2px;
        border: none;
        border-radius: 3px;
        cursor: pointer;
    }
</style>

<style>
	.modal-backdrop {
		display: none;
		position: fixed;
		top: 0;
		left: 0;
		width: 100%;
		height: 100%;
		background-color: rgba(0, 0, 0, 0.7);
		z-index: 1;
	}

	.modal-content {
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background: #272726;
		padding: 40px;
		z-index: 2;
	}

	.close-modal {
		position: absolute;
		top: 10px;
		right: 10px;
		cursor: pointer;
		background: none;
		border: none;
		font-size: 24px;
		color: white;
	}

	.wrapper,
	section {
		max-width: 900px;
	}
</style>