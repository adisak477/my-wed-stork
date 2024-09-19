<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบจัดการสต๊อกสินค้า</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
        }
        .container {
            width: 70%;
            margin: 20px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 8px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #333;
        }
        h2 {
            color: #555;
            border-bottom: 2px solid #ddd;
            padding-bottom: 5px;
            margin-bottom: 15px;
        }
        input, button {
            margin: 5px 0;
            padding: 8px;
            border-radius: 4px;
            border: 1px solid #ccc;
        }
        button {
            background-color: #28a745;
            color: white;
            border: none;
            cursor: pointer;
        }
        button:hover {
            background-color: #218838;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 10px;
            text-align: center;
        }
        th {
            background-color: #007bff;
            color: white;
            cursor: pointer;
        }
        th:hover {
            background-color: #0056b3;
        }
        .total, .summary {
            font-weight: bold;
            text-align: right;
            margin-top: 20px;
            padding: 10px;
            background-color: #e9ecef;
            border-radius: 4px;
        }
        .total p, .summary p {
            margin: 5px 0;
        }
        label {
            display: block;
            margin-top: 10px;
            font-weight: bold;
        }
        input[type="text"], input[type="number"] {
            width: calc(100% - 20px);
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>ระบบจัดการสต๊อกสินค้า</h1>
        
        <h2>เพิ่มสินค้า</h2>
        <label for="product-name">ชื่อสินค้า:</label>
        <input type="text" id="product-name" placeholder="ชื่อสินค้า">
        <label for="product-price">ราคา:</label>
        <input type="number" id="product-price" placeholder="ราคา">
        <label for="product-quantity">จำนวน:</label>
        <input type="number" id="product-quantity" placeholder="จำนวน">
        <label for="product-sold">ขายได้:</label>
        <input type="number" id="product-sold" placeholder="ขายได้" oninput="calculateRemaining()">
        <label for="product-remaining">เหลือ:</label>
        <input type="number" id="product-remaining" placeholder="เหลือ" readonly>
        <button onclick="addProduct()">เพิ่มสินค้า</button>

        <h2>กรองสินค้า</h2>
        <label for="filter">ค้นหาสินค้า:</label>
        <input type="text" id="filter" placeholder="พิมพ์ชื่อสินค้า" onkeyup="filterProducts()">
        
        <h2>รายการสินค้า</h2>
        <table>
            <thead>
                <tr>
                    <th onclick="sortProducts('name')">ชื่อสินค้า</th>
                    <th onclick="sortProducts('price')">ราคา</th>
                    <th onclick="sortProducts('quantity')">จำนวน</th>
                    <th onclick="sortProducts('sold')">ขายได้</th>
                    <th>เหลือ</th>
                    <th>เพิ่มสต๊อก</th>
                    <th>ลดสต๊อก</th>
                    <th>ลบ</th>
                </tr>
            </thead>
            <tbody id="product-list">
                <!-- รายการสินค้าจะแสดงที่นี่ -->
            </tbody>
        </table>

        <div class="summary">
            <p>ยอดรวมที่ขายได้: <span id="total-sold-amount">0</span> บาท</p>
            <p>จำนวนที่ขายได้ทั้งหมด: <span id="total-sold-quantity">0</span> ชิ้น</p>
            <p>ยอดรวมที่เหลือ: <span id="total-remaining-amount">0</span> บาท</p>
            <p>จำนวนที่เหลือทั้งหมด: <span id="total-remaining-quantity">0</span> ชิ้น</p>
        </div>

        <div class="total">
            <p>ยอดรวมราคาทั้งหมด: <span id="total-amount">0</span> บาท</p>
            <p>ยอดรวมจำนวนสินค้าทั้งหมด: <span id="total-quantity">0</span> ชิ้น</p>
        </div>
    </div>

    <script>
        let products = [];

        window.onload = function() {
            const storedProducts = localStorage.getItem('products');
            if (storedProducts) {
                products = JSON.parse(storedProducts);
                renderProducts();
                calculateTotal();
                calculateSummary();
            }
        }

        function calculateRemaining() {
            const quantity = parseInt(document.getElementById('product-quantity').value) || 0;
            const sold = parseInt(document.getElementById('product-sold').value) || 0;
            const remaining = quantity - sold;
            if (remaining >= 0) {
                document.getElementById('product-remaining').value = remaining;
            } else {
                alert('จำนวนขายไม่สามารถเกินจำนวนที่มีได้');
                document.getElementById('product-sold').value = 0;
                document.getElementById('product-remaining').value = quantity;
            }
        }

        function addProduct() {
            const name = document.getElementById('product-name').value;
            const price = parseFloat(document.getElementById('product-price').value);
            const quantity = parseInt(document.getElementById('product-quantity').value);
            const sold = parseInt(document.getElementById('product-sold').value);
            const remaining = parseInt(document.getElementById('product-remaining').value);

            if (name && price && quantity && sold >= 0 && remaining >= 0) {
                products.push({ name, price, quantity, sold });
                saveToLocalStorage();
                renderProducts();
                calculateTotal();
                calculateSummary();
                clearFields();
            } else {
                alert('กรุณากรอกข้อมูลให้ครบถ้วน');
            }
        }

        function renderProducts() {
            const productList = document.getElementById('product-list');
            productList.innerHTML = '';

            products.forEach((product, index) => {
                const remaining = product.quantity - product.sold;
                const row = `<tr>
                    <td>${product.name}</td>
                    <td>${product.price}</td>
                    <td>${product.quantity}</td>
                    <td>${product.sold}</td>
                    <td>${remaining}</td>
                    <td><button onclick="updateStock(${index}, 1)">เพิ่ม</button></td>
                    <td><button onclick="updateStock(${index}, -1)">ลด</button></td>
                    <td><button onclick="deleteProduct(${index})">ลบ</button></td>
                </tr>`;
                productList.innerHTML += row;
            });
        }

        function updateStock(index, change) {
            if (products[index].quantity + change >= 0) {
                products[index].quantity += change;
                saveToLocalStorage();
                renderProducts();
                calculateTotal();
                calculateSummary();
            } else {
                alert('จำนวนสต๊อกไม่เพียงพอ');
            }
        }

        function deleteProduct(index) {
            if (confirm('คุณต้องการลบสินค้านี้หรือไม่?')) {
                products.splice(index, 1);
                saveToLocalStorage();
                renderProducts();
                calculateTotal();
                calculateSummary();
            }
        }

        function clearFields() {
            document.getElementById('product-name').value = '';
            document.getElementById('product-price').value = '';
            document.getElementById('product-quantity').value = '';
            document.getElementById('product-sold').value = '';
            document.getElementById('product-remaining').value = '';
        }

        function calculateTotal() {
            let totalAmount = 0;
            let totalQuantity = 0;
            products.forEach(product => {
                totalAmount += product.price * product.quantity;
                totalQuantity += product.quantity;
            });
            document.getElementById('total-amount').innerText = totalAmount;
            document.getElementById('total-quantity').innerText = totalQuantity;
        }

        function calculateSummary() {
            let totalSoldAmount = 0;
            let totalSoldQuantity = 0;
            let totalRemainingAmount = 0;
            let totalRemainingQuantity = 0;

            products.forEach(product => {
                const soldAmount = product.price * product.sold;
                const remainingAmount = product.price * (product.quantity - product.sold);

                totalSoldAmount += soldAmount;
                totalSoldQuantity += product.sold;
                totalRemainingAmount += remainingAmount;
                totalRemainingQuantity += (product.quantity - product.sold);
            });

            document.getElementById('total-sold-amount').innerText = totalSoldAmount;
            document.getElementById('total-sold-quantity').innerText = totalSoldQuantity;
            document.getElementById('total-remaining-amount').innerText = totalRemainingAmount;
            document.getElementById('total-remaining-quantity').innerText = totalRemainingQuantity;
        }

        function saveToLocalStorage() {
            localStorage.setItem('products', JSON.stringify(products));
        }

        function filterProducts() {
            const filter = document.getElementById('filter').value.toLowerCase();
            const filteredProducts = products.filter(product => product.name.toLowerCase().includes(filter));
            const productList = document.getElementById('product-list');
            productList.innerHTML = '';

            filteredProducts.forEach((product, index) => {
                const remaining = product.quantity - product.sold;
                const row = `<tr>
                    <td>${product.name}</td>
                    <td>${product.price}</td>
                    <td>${product.quantity}</td>
                    <td>${product.sold}</td>
                    <td>${remaining}</td>
                    <td><button onclick="updateStock(${index}, 1)">เพิ่ม</button></td>
                    <td><button onclick="updateStock(${index}, -1)">ลด</button></td>
                    <td><button onclick="deleteProduct(${index})">ลบ</button></td>
                </tr>`;
                productList.innerHTML += row;
            });
        }

        function sortProducts(key) {
            products.sort((a, b) => {
                if (typeof a[key] === 'string') {
                    return a[key].localeCompare(b[key]);
                } else {
                    return a[key] - b[key];
                }
            });
            saveToLocalStorage();
            renderProducts();
            calculateTotal();
            calculateSummary();
        }
    </script>
</body>
</html>
