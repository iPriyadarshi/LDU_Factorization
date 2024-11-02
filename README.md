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
    <script src="script.js"></script>
</body>
</html>
```
#### CSS (`style.css`)
Styles the web application for a clean and modern look.

```css
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

.container {
    background: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    text-align: center;
}

input[type="number"] {
    margin: 10px;
    width: 50px;
}

button {
    margin-top: 10px;
    padding: 10px 20px;
    background-color: #333;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #555;
}

table {
    margin: 20px auto;
    border-collapse: collapse;
}

td {
    padding: 5px;
    border: 1px solid #ddd;
}

.step {
    margin-top: 20px;
    padding: 10px;
    background-color: #e0e0e0;
    border-radius: 5px;
    text-align: left;
}
```
##### JavaScript (`script.js`)
Handles the logic for generating the matrix, performing the factorization, and displaying the results.

```javascript
function generateMatrix() {
    const n = parseInt(document.getElementById('matrixSize').value);
    let matrixHtml = '<table>';
    
    for (let i = 0; i < n; i++) {
        matrixHtml += '<tr>';
        for (let j = 0; j < n; j++) {
            matrixHtml += `<td><input type="number" id="cell-${i}-${j}" value="0"></td>`;
        }
        matrixHtml += '</tr>';
    }
    
    matrixHtml += '</table>';
    document.getElementById('matrixInput').innerHTML = matrixHtml;
}

function getMatrix(n) {
    let matrix = [];
    
    for (let i = 0; i < n; i++) {
        let row = [];
        for (let j = 0; j < n; j++) {
            const value = parseFloat(document.getElementById(`cell-${i}-${j}`).value);
            row.push(value);
        }
        matrix.push(row);
    }
    
    return matrix;
}

function factorizeLDU() {
    const n = parseInt(document.getElementById('matrixSize').value);
    const matrix = getMatrix(n);
    let L = Array.from({length: n}, () => Array(n).fill(0));
    let D = Array.from({length: n}, () => Array(n).fill(0));
    let U = Array.from({length: n}, () => Array(n).fill(0));
    let steps = '';
    
    for (let i = 0; i < n; i++) {
        steps += `<div class="step"><h3>Step ${i+1}</h3>`;
        
        for (let j = i; j < n; j++) {
            U[i][j] = matrix[i][j];
            for (let k = 0; k < i; k++) {
                U[i][j] -= L[i][k] * D[k][k] * U[k][j];
            }
            steps += `<p>U[${i}][${j}] = ${U[i][j].toFixed(2)}</p>`;
        }
        for (let j = i; j < n; j++) {
            if (i == j) {
                D[i][i] = U[i][i];
                U[i][i] = 1;
                steps += `<p>D[${i}][${i}] = ${D[i][i].toFixed(2)}</p>`;
            } else {
                L[j][i] = matrix[j][i];
                for (let k = 0; k < i; k++) {
                    L[j][i] -= L[j][k] * D[k][k] * U[k][i];
                }
                L[j][i] /= D[i][i];
                steps += `<p>L[${j}][${i}] = ${L[j][i].toFixed(2)}</p>`;
            }
        }
        steps += '</div>';
    }
    
    let output = '<h2>Matrix L</h2>' + matrixToHtml(L);
    output += '<h2>Matrix D</h2>' + matrixToHtml(D);
    output += '<h2>Matrix U</h2>' + matrixToHtml(U);
    output += steps;
    
    document.getElementById('output').innerHTML = output;
}

function matrixToHtml(matrix) {
    let html = '<table>';
    
    for (let row of matrix) {
        html += '<tr>';
        for (let cell of row) {
            html += `<td>${cell.toFixed(2)}</td>`;
        }
        html += '</tr>';
    }
    
    html += '</table>';
    return html;
}
```
### Contributing
Feel free to open issues or submit pull requests. Contributions are always welcome!
