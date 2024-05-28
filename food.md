---
layout: default
comments: true
title: food
---

<div style="text-align: center; padding: 20px;">
    <h1 style="color: #ff66cc; font-size: 36px;">Welcome to the FOOD page!</h1>
</div>

<div style="display: flex; justify-content: center; margin-bottom: 30px;">
    <div style="width: 50%; background-color: #ffecf2; padding: 20px; border-radius: 15px;">
        <h2 style="color: #ff66cc;">Enter your favorite foods to find out your diet type</h2>
        <form id="formToGetFoodDetail">
            <label for="Foods" style="color: #ff66cc;">Enter a list of your favorite foods (separated by commas):</label><br />
            <input type="text" id="Foods" name="Foods" style="padding: 8px; border-radius: 5px; margin-bottom: 10px; width: 100%;" placeholder="e.g., Pizza, Sushi, Ice Cream"><br />
            <button type="submit" value="btnToGetFoodDetail" id="get_foods" style="background-color: #ff66cc; color: white; padding: 10px 15px; border: none; border-radius: 5px; cursor: pointer;">Submit</button>
        </form>
        <div id="foodLists" style="margin-top: 20px; color: #ff66cc;">
            <h3>Click on foods to add them to your list:</h3>
            <div>
                <h4>Dairy</h4>
                <ul id="dairyList"></ul>
            </div>
            <div>
                <h4>Protein</h4>
                <ul id="proteinList"></ul>
            </div>
            <div>
                <h4>Grains</h4>
                <ul id="grainsList"></ul>
            </div>
            <div>
                <h4>Vegetables</h4>
                <ul id="vegetablesList"></ul>
            </div>
            <div>
                <h4>Fruits</h4>
                <ul id="fruitsList"></ul>
            </div>
        </div>
        <div id="results" style="margin-top: 20px; color: #ff66cc;"></div>
    </div>
</div>

<script>
// Food categories
const dairy = ["milk", "cheese", "yogurt", "kefir", "butter", "sour cream"];
const protein = ["lean meats", "beans legumes", "tofu tempeh", "eggs", "nuts seeds"];
const grains = ["whole wheat bread", "brown rice", "quinoa", "oats", "barley", "whole wheat pasta", "corn tortillas", "millet"];
const vegetables = ["leafy greens", "cruciferous vegetables", "allium vegetables", "root vegetables", "starchy vegetables", "bell peppers", "mushrooms", "asparagus", "eggplant", "zucchini"];
const fruits = ["berries", "citrus fruits", "melons", "stone fruits", "tropical fruits", "apples", "pears", "bananas"];

const categories = {
    dairy: dairy,
    protein: protein,
    grains: grains,
    vegetables: vegetables,
    fruits: fruits
};

// Create food lists using forEach (Conventional method)
Object.keys(categories).forEach(category => {
    const ul = document.getElementById(category + 'List');
    categories[category].forEach(food => {
        const li = document.createElement('li');
        li.textContent = food;
        li.style.cursor = 'pointer';
        li.addEventListener('click', () => addFoodToList(food));
        ul.appendChild(li);
    });
});

// Add food to input list
function addFoodToList(food) {
    const foodInput = document.getElementById('Foods');
    const currentFoods = foodInput.value.split(',').map(f => f.trim()).filter(f => f !== '');
    if (!currentFoods.includes(food)) {
        currentFoods.push(food);
    }
    foodInput.value = currentFoods.join(', ');
}

// Form submission and food comparison
document.getElementById('formToGetFoodDetail').addEventListener('submit', function(event) {
    event.preventDefault();

    const userFoods = document.getElementById('Foods').value.toLowerCase().split(',').map(food => food.trim());

    const counts = {
        dairy: 0,
        protein: 0,
        grains: 0,
        vegetables: 0,
        fruits: 0
    };

    // 2D Iteration example: Iterate over each category and its foods
    userFoods.forEach(food => {
        for (const category in categories) {
            for (let i = 0; i < categories[category].length; i++) {
                if (categories[category][i] === food) {
                    counts[category]++;
                }
            }
        }
    });

    const resultsDiv = document.getElementById('results');
    resultsDiv.innerHTML = `
        <p>Dairy: ${counts.dairy}</p>
        <p>Protein: ${counts.protein}</p>
        <p>Grains: ${counts.grains}</p>
        <p>Vegetables: ${counts.vegetables}</p>
        <p>Fruits: ${counts.fruits}</p>
    `;
});

// List comprehension example (Processing a list)
const allFoods = [...dairy, ...protein, ...grains, ...vegetables, ...fruits];
const uniqueFoods = [...new Set(allFoods)];  // Using list comprehension to remove duplicates
console.log(uniqueFoods);

// Sorting example (Algorithmic)
uniqueFoods.sort();  // Simple alphabetical sort
console.log('Sorted Foods:', uniqueFoods);

// Big(O) notation example
// Time Complexity: O(n * m) for 2D iteration, where n is the number of user foods and m is the number of categories
// Space Complexity: O(n) for storing user foods and the count objects
</script>
