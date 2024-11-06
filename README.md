# LDU_Factorization
## LDU Factorization of an n*n Matrix
### Overview
This project provides a web-based tool to perform LDU (Lower-Diagonal-Upper) factorization of an n\*n matrix. The application is built using HTML, CSS, and JavaScript, and it allows users to input a matrix, visualize the steps of the factorization process, and see the resulting L, D, and U matrices.

### Features
- **Matrix Input**: Dynamically generate a matrix input form based on user-defined matrix size.

- **Detailed Steps**: Display each step of the LDU factorization process.

- **Result Visualization**: Show the resulting L, D, and U matrices after factorization.

### Installation
To get started with the project, follow these steps:

1. **Clone the repository**:
```bash
git clone https://github.com/priyadarshi-1505/LDU_factorization.git
cd LDU_factorization
```

2. **Open `index.html` in a web browser**:
      
```bash
open index.html
```
### Usage
**1. Matrix Size:** Enter the desired matrix size (n) in the input field.

**2. Generate Matrix:** Click the "Generate Matrix" button to create the input form for the matrix.

**3. Input Matrix Elements:** Fill in the elements of the matrix.

**4. Factorize:** Click the "Factorize" button to perform the LDU factorization. The detailed steps and results will be displayed below.

### Project Structure
- [index.html](index.html): The main HTML file that contains the structure of the web application.

- [style.css](style.css): The CSS file for styling the web application.

- [script.js](script.js): The JavaScript file that contains the logic for LDU factorization and user interactions.

### Code Explanation
#### HTML (`index.html`)
Provides the structure of the application, including input fields and buttons.
```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LDU Factorization</title>
    <link rel="stylesheet" href="style.css">
    <script src="script.js"></script>
</head>

<body>
    <div class="container">
        <h1>LDU Factorization of an n*n Matrix</h1>
        <div>
            <label for="matrixSize">Matrix Size (n): </label>
            <input type="number" id="matrixSize" value="3" min="1">
            <button onclick="generateMatrix()">Generate Matrix</button>
        </div>
        <div id="matrixInput"></div>
        <button onclick="factorizeLDU()">Factorize</button>
        <div id="output"></div>
    </div>
</body>

</html>
```
#### CSS (`style.css`)
Styles the web application for a clean and modern look.

```css
body {
    font-family: Arial, sans-serif;
    background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
    margin: 0;
    padding: 20px;
    min-height: 100vh;
}

.container {
    max-width: 900px;
    margin: 0 auto;
    background-color: white;
    padding: 30px;
    border-radius: 10px;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
}

h1 {
    color: #2c3e50;
    text-align: center;
    margin-bottom: 30px;
    font-size: 2.2em;
}

button {
    background-color: #3498db;
    color: white;
    border: none;
    padding: 10px 20px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
    font-size: 1em;
}

button:hover {
    background-color: #2980b9;
}

input[type="number"] {
    padding: 8px;
    border: 1px solid #ddd;
    border-radius: 4px;
    margin: 5px;
    width: 60px;
}

.matrix {
    display: inline-block;
    margin: 20px;
    padding: 15px;
    background-color: #f8f9fa;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
}

.matrix-cell {
    width: 50px;
    text-align: center;
    display: inline-block;
    margin: 3px;
    font-family: monospace;
}

.matrix-label {
    font-weight: bold;
    margin-bottom: 10px;
    color: #34495e;
}

.step {
    margin: 20px 0;
    padding: 15px;
    background-color: #fff;
    border-left: 4px solid #3498db;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
}

.step h3 {
    color: #2c3e50;
    margin-top: 0;
}

#output {
    margin-top: 30px;
}

@media (max-width: 600px) {
    .container {
        padding: 15px;
    }

    h1 {
        font-size: 1.8em;
    }

    .matrix {
        margin: 10px;
        padding: 10px;
    }
}
```
#### JavaScript (`script.js`)
Handles the logic for generating the matrix, performing the factorization, and displaying the results.

```javascript
function generateMatrix() {
    const n = parseInt(document.getElementById('matrixSize').value);
    let html = '<div class="matrix">';
    
    for(let i = 1; i <= n; i++) {
        for(let j = 1; j <= n; j++) {
            html += `<input type="number" class="matrix-cell" id="cell_${i}_${j}" value="0">`;
        }
        html += '<br>';
    }
    html += '</div>';
    document.getElementById('matrixInput').innerHTML = html;
}

function factorizeLDU() {
    const n = parseInt(document.getElementById('matrixSize').value);
    let A = [];
    let L = [];
    let D = [];
    let U = [];
    let steps = [];

    // Initialize matrices
    for(let i = 1; i <= n; i++) {
        A[i] = [];
        L[i] = Array(n + 1).fill(0);
        D[i] = Array(n + 1).fill(0);
        U[i] = Array(n + 1).fill(0);
        L[i][i] = 1;
        U[i][i] = 1;
        for(let j = 1; j <= n; j++) {
            A[i][j] = parseFloat(document.getElementById(`cell_${i}_${j}`).value);
        }
    }

    steps.push({
        step: "Initial Matrix A:",
        matrix: JSON.parse(JSON.stringify(A))
    });

    // LDU Factorization
    for(let k = 1; k <= n; k++) {
        D[k][k] = A[k][k];
        
        for(let i = k + 1; i <= n; i++) {
            L[i][k] = A[i][k] / A[k][k];
            steps.push({
                step: `Computing L[${i}][${k}] = A[${i}][${k}]/A[${k}][${k}] = ${L[i][k]}`,
                L: JSON.parse(JSON.stringify(L))
            });
        }

        for(let j = k + 1; j <= n; j++) {
            U[k][j] = A[k][j] / A[k][k];
            steps.push({
                step: `Computing U[${k}][${j}] = A[${k}][${j}]/A[${k}][${k}] = ${U[k][j]}`,
                U: JSON.parse(JSON.stringify(U))
            });
        }

        for(let i = k + 1; i <= n; i++) {
            for(let j = k + 1; j <= n; j++) {
                A[i][j] = A[i][j] - L[i][k] * D[k][k] * U[k][j];
            }
        }
    }

    // Display results
    let output = '';
    steps.forEach((step, index) => {
        output += `<div class="step">
            <h3>Step ${index + 1}</h3>
            <p>${step.step}</p>`;
        
        if(step.matrix) {
            output += displayMatrix("A", step.matrix);
        }
        if(step.L) {
            output += displayMatrix("L", step.L);
        }
        if(step.U) {
            output += displayMatrix("U", step.U);
        }
        output += '</div>';
    });

    output += '<h3>Final Results:</h3>';
    output += displayMatrix("L", L);
    output += displayMatrix("D", D);
    output += displayMatrix("U", U);

    document.getElementById('output').innerHTML = output;
}

function displayMatrix(label, matrix) {
    let html = `<div class="matrix">
        <div class="matrix-label">${label}:</div>`;
    
    for(let i = 1; i < matrix.length; i++) {
        for(let j = 1; j < matrix[i].length; j++) {
            html += `<span class="matrix-cell">${matrix[i][j].toFixed(2)}</span>`;
        }
        html += '<br>';
    }
    html += '</div>';
    return html;
}
```
### Contributing
Feel free to open issues or submit pull requests. Contributions are always welcome!
