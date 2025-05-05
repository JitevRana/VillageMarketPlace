<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Local Business Promotion Widget</title>
    <style>
        :root {
            --primary-color: #4a6fa5;
            --secondary-color: #166088;
            --accent-color: #4fc3f7;
            --light-color: #f8f9fa;
            --dark-color: #343a40;
            --success-color: #28a745;
            --danger-color: #dc3545;
            --warning-color: #ffc107;
        }
        
        * {
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            margin: 0;
            padding: 15px;
            background-color: #f5f5f5;
            color: #333;
        }
        
        .widget-container {
            max-width: 1000px;
            margin: 0 auto;
            background: white;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            overflow: hidden;
        }
        
        .header {
            background-color: var(--primary-color);
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .header h2 {
            margin: 0;
            font-size: 1.5rem;
        }
        
        .auth-section {
            display: flex;
            gap: 10px;
        }
        
        input {
            padding: 8px 12px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 14px;
        }
        
        button {
            padding: 8px 15px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-size: 14px;
            transition: background-color 0.3s;
        }
        
        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }
        
        .btn-primary:hover {
            background-color: var(--secondary-color);
        }
        
        .btn-success {
            background-color: var(--success-color);
            color: white;
        }
        
        .btn-success:hover {
            background-color: #218838;
        }
        
        .btn-danger {
            background-color: var(--danger-color);
            color: white;
        }
        
        .btn-danger:hover {
            background-color: #c82333;
        }
        
        .btn-warning {
            background-color: var(--warning-color);
            color: #212529;
        }
        
        .btn-warning:hover {
            background-color: #e0a800;
        }
        
        .admin-controls {
            display: none;
            padding: 15px;
            background-color: #f8f9fa;
            border-bottom: 1px solid #ddd;
            gap: 10px;
            flex-wrap: wrap;
        }
        
        .admin-controls.active {
            display: flex;
        }
        
        .tabs {
            display: flex;
            border-bottom: 1px solid #ddd;
            background-color: #f8f9fa;
        }
        
        .tab {
            padding: 12px 20px;
            cursor: pointer;
            border-right: 1px solid #ddd;
        }
        
        .tab.active {
            background-color: white;
            border-bottom: 2px solid var(--primary-color);
            font-weight: bold;
        }
        
        .tab-content {
            display: none;
            padding: 20px;
        }
        
        .tab-content.active {
            display: block;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
        }
        
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: #f8f9fa;
            font-weight: bold;
            cursor: pointer;
            user-select: none;
        }
        
        th:hover {
            background-color: #e9ecef;
        }
        
        tr:hover {
            background-color: #f5f5f5;
        }
        
        .form-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        
        .form-modal.active {
            display: flex;
        }
        
        .modal-content {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            width: 90%;
            max-width: 500px;
            max-height: 90vh;
            overflow-y: auto;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        .form-group input, .form-group select {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        
        .form-actions {
            display: flex;
            justify-content: flex-end;
            gap: 10px;
            margin-top: 20px;
        }
        
        .publish-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        
        .publish-modal.active {
            display: flex;
        }
        
        .publish-content {
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            width: 90%;
            max-width: 800px;
            max-height: 90vh;
            overflow-y: auto;
        }
        
        .publish-content h3 {
            margin-top: 0;
            color: var(--primary-color);
        }
        
        .publish-section {
            margin-bottom: 20px;
            padding-bottom: 20px;
            border-bottom: 1px solid #eee;
        }
        
        .publish-section:last-child {
            border-bottom: none;
        }
        
        .publish-actions {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
        }
        
        @media (max-width: 768px) {
            .header {
                flex-direction: column;
                gap: 10px;
                text-align: center;
            }
            
            .auth-section {
                width: 100%;
                flex-direction: column;
            }
            
            .tabs {
                flex-wrap: wrap;
            }
            
            .tab {
                flex: 1 0 auto;
                text-align: center;
            }
            
            th, td {
                padding: 8px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <div class="widget-container">
        <div class="header">
            <h2>Local Business Promotion</h2>
            <div class="auth-section">
                <input type="text" id="username" placeholder="Username">
                <input type="password" id="password" placeholder="Password">
                <button id="login-btn" class="btn-primary">Login</button>
                <button id="logout-btn" class="btn-danger" style="display: none;">Logout</button>
            </div>
        </div>
        
        <div class="admin-controls" id="admin-controls">
            <button id="add-product" class="btn-success">Add Product</button>
            <button id="add-delivery" class="btn-success">Add Delivery Schedule</button>
            <button id="add-seller" class="btn-success">Add Seller</button>
            <button id="add-buyer" class="btn-success">Add Buyer</button>
            <button id="publish-btn" class="btn-warning">Publish Webpage</button>
        </div>
        
        <div class="tabs">
            <div class="tab active" data-tab="products">Products</div>
            <div class="tab" data-tab="delivery">Delivery Schedules</div>
            <div class="tab" data-tab="sellers">Seller Directory</div>
            <div class="tab" data-tab="buyers">Buyer Directory</div>
        </div>
        
        <div class="tab-content active" id="products-content">
            <table id="products-table">
                <thead>
                    <tr>
                        <th data-sort="name">Name</th>
                        <th data-sort="unit">Unit</th>
                        <th data-sort="price">Price/Unit</th>
                        <th data-sort="category">Category</th>
                        <th data-sort="availability">Availability</th>
                        <th class="actions">Actions</th>
                    </tr>
                </thead>
                <tbody id="products-body">
                    <!-- Products will be loaded here -->
                </tbody>
            </table>
        </div>
        
        <div class="tab-content" id="delivery-content">
            <table id="delivery-table">
                <thead>
                    <tr>
                        <th data-sort="type">Delivery Type</th>
                        <th data-sort="location">Location</th>
                        <th data-sort="fee">Fee</th>
                        <th data-sort="time">Schedule Time</th>
                        <th class="actions">Actions</th>
                    </tr>
                </thead>
                <tbody id="delivery-body">
                    <!-- Delivery schedules will be loaded here -->
                </tbody>
            </table>
        </div>
        
        <div class="tab-content" id="sellers-content">
            <table id="sellers-table">
                <thead>
                    <tr>
                        <th data-sort="name">Name</th>
                        <th data-sort="phone">Phone</th>
                        <th data-sort="address">Address</th>
                        <th data-sort="category">Product Category</th>
                        <th data-sort="priceRange">Price Range</th>
                        <th class="actions">Actions</th>
                    </tr>
                </thead>
                <tbody id="sellers-body">
                    <!-- Sellers will be loaded here -->
                </tbody>
            </table>
        </div>
        
        <div class="tab-content" id="buyers-content">
            <table id="buyers-table">
                <thead>
                    <tr>
                        <th data-sort="name">Name</th>
                        <th data-sort="phone">Phone</th>
                        <th data-sort="address">Address</th>
                        <th data-sort="interest">Interest Category</th>
                        <th data-sort="maxPrice">Max Price</th>
                        <th class="actions">Actions</th>
                    </tr>
                </thead>
                <tbody id="buyers-body">
                    <!-- Buyers will be loaded here -->
                </tbody>
            </table>
        </div>
    </div>
    
    <!-- Product Form Modal -->
    <div class="form-modal" id="product-modal">
        <div class="modal-content">
            <h3 id="product-modal-title">Add Product</h3>
            <form id="product-form">
                <input type="hidden" id="product-id">
                <div class="form-group">
                    <label for="product-name">Name</label>
                    <input type="text" id="product-name" required>
                </div>
                <div class="form-group">
                    <label for="product-unit">Unit</label>
                    <input type="text" id="product-unit" required>
                </div>
                <div class="form-group">
                    <label for="product-price">Price/Unit</label>
                    <input type="number" id="product-price" step="0.01" required>
                </div>
                <div class="form-group">
                    <label for="product-category">Category</label>
                    <input type="text" id="product-category" required>
                </div>
                <div class="form-group">
                    <label for="product-availability">Availability</label>
                    <select id="product-availability" required>
                        <option value="Yes">Yes</option>
                        <option value="No">No</option>
                    </select>
                </div>
                <div class="form-actions">
                    <button type="button" class="btn-danger" id="cancel-product">Cancel</button>
                    <button type="submit" class="btn-success">Save</button>
                </div>
            </form>
        </div>
    </div>
    
    <!-- Delivery Form Modal -->
    <div class="form-modal" id="delivery-modal">
        <div class="modal-content">
            <h3 id="delivery-modal-title">Add Delivery Schedule</h3>
            <form id="delivery-form">
                <input type="hidden" id="delivery-id">
                <div class="form-group">
                    <label for="delivery-type">Delivery Type</label>
                    <input type="text" id="delivery-type" required>
                </div>
                <div class="form-group">
                    <label for="delivery-location">Location</label>
                    <input type="text" id="delivery-location" required>
                </div>
                <div class="form-group">
                    <label for="delivery-fee">Fee</label>
                    <input type="number" id="delivery-fee" step="0.01" required>
                </div>
                <div class="form-group">
                    <label for="delivery-time">Schedule Time</label>
                    <input type="text" id="delivery-time" placeholder="e.g., Mon-Fri 9am-5pm" required>
                </div>
                <div class="form-actions">
                    <button type="button" class="btn-danger" id="cancel-delivery">Cancel</button>
                    <button type="submit" class="btn-success">Save</button>
                </div>
            </form>
        </div>
    </div>
    
    <!-- Seller Form Modal -->
    <div class="form-modal" id="seller-modal">
        <div class="modal-content">
            <h3 id="seller-modal-title">Add Seller</h3>
            <form id="seller-form">
                <input type="hidden" id="seller-id">
                <div class="form-group">
                    <label for="seller-name">Name</label>
                    <input type="text" id="seller-name" required>
                </div>
                <div class="form-group">
                    <label for="seller-phone">Phone</label>
                    <input type="text" id="seller-phone" required>
                </div>
                <div class="form-group">
                    <label for="seller-address">Address</label>
                    <input type="text" id="seller-address" required>
                </div>
                <div class="form-group">
                    <label for="seller-category">Product Category</label>
                    <input type="text" id="seller-category" required>
                </div>
                <div class="form-group">
                    <label for="seller-priceRange">Price Range</label>
                    <input type="text" id="seller-priceRange" placeholder="e.g., $10-$50" required>
                </div>
                <div class="form-actions">
                    <button type="button" class="btn-danger" id="cancel-seller">Cancel</button>
                    <button type="submit" class="btn-success">Save</button>
                </div>
            </form>
        </div>
    </div>
    
    <!-- Buyer Form Modal -->
    <div class="form-modal" id="buyer-modal">
        <div class="modal-content">
            <h3 id="buyer-modal-title">Add Buyer</h3>
            <form id="buyer-form">
                <input type="hidden" id="buyer-id">
                <div class="form-group">
                    <label for="buyer-name">Name</label>
                    <input type="text" id="buyer-name" required>
                </div>
                <div class="form-group">
                    <label for="buyer-phone">Phone</label>
                    <input type="text" id="buyer-phone" required>
                </div>
                <div class="form-group">
                    <label for="buyer-address">Address</label>
                    <input type="text" id="buyer-address" required>
                </div>
                <div class="form-group">
                    <label for="buyer-interest">Interest Category</label>
                    <input type="text" id="buyer-interest" required>
                </div>
                <div class="form-group">
                    <label for="buyer-maxPrice">Max Price</label>
                    <input type="number" id="buyer-maxPrice" step="0.01" required>
                </div>
                <div class="form-actions">
                    <button type="button" class="btn-danger" id="cancel-buyer">Cancel</button>
                    <button type="submit" class="btn-success">Save</button>
                </div>
            </form>
        </div>
    </div>
    
    <!-- Publish Modal -->
    <div class="publish-modal" id="publish-modal">
        <div class="publish-content">
            <h3>Published Business Directory</h3>
            
            <div class="publish-section">
                <h4>Available Products</h4>
                <div id="publish-products"></div>
            </div>
            
            <div class="publish-section">
                <h4>Delivery Schedules</h4>
                <div id="publish-delivery"></div>
            </div>
            
            <div class="publish-section">
                <h4>Seller Directory</h4>
                <div id="publish-sellers"></div>
            </div>
            
            <div class="publish-section">
                <h4>Buyer Directory</h4>
                <div id="publish-buyers"></div>
            </div>
            
            <div class="publish-actions">
                <button class="btn-primary" id="print-btn">Print</button>
                <button class="btn-danger" id="close-publish">Close</button>
            </div>
        </div>
    </div>

    <script>
        // Constants
        const ADMIN_USERNAME = 'jitbrindrana';
        const ADMIN_PASSWORD = 'jitji@152207#';
        
        // DOM Elements
        const loginBtn = document.getElementById('login-btn');
        const logoutBtn = document.getElementById('logout-btn');
        const usernameInput = document.getElementById('username');
        const passwordInput = document.getElementById('password');
        const adminControls = document.getElementById('admin-controls');
        
        // Tab elements
        const tabs = document.querySelectorAll('.tab');
        const tabContents = document.querySelectorAll('.tab-content');
        
        // Add buttons
        const addProductBtn = document.getElementById('add-product');
        const addDeliveryBtn = document.getElementById('add-delivery');
        const addSellerBtn = document.getElementById('add-seller');
        const addBuyerBtn = document.getElementById('add-buyer');
        const publishBtn = document.getElementById('publish-btn');
        
        // Modal elements
        const productModal = document.getElementById('product-modal');
        const deliveryModal = document.getElementById('delivery-modal');
        const sellerModal = document.getElementById('seller-modal');
        const buyerModal = document.getElementById('buyer-modal');
        const publishModal = document.getElementById('publish-modal');
        
        // Form elements
        const productForm = document.getElementById('product-form');
        const deliveryForm = document.getElementById('delivery-form');
        const sellerForm = document.getElementById('seller-form');
        const buyerForm = document.getElementById('buyer-form');
        
        // Cancel buttons
        const cancelProductBtn = document.getElementById('cancel-product');
        const cancelDeliveryBtn = document.getElementById('cancel-delivery');
        const cancelSellerBtn = document.getElementById('cancel-seller');
        const cancelBuyerBtn = document.getElementById('cancel-buyer');
        
        // Publish modal buttons
        const printBtn = document.getElementById('print-btn');
        const closePublishBtn = document.getElementById('close-publish');
        
        // Data storage keys
        const STORAGE_KEYS = {
            PRODUCTS: 'localBusinessProducts',
            DELIVERY: 'localBusinessDelivery',
            SELLERS: 'localBusinessSellers',
            BUYERS: 'localBusinessBuyers',
            AUTH: 'localBusinessAuth'
        };
        
        // Initialize data
        let products = JSON.parse(localStorage.getItem(STORAGE_KEYS.PRODUCTS)) || [];
        let deliverySchedules = JSON.parse(localStorage.getItem(STORAGE_KEYS.DELIVERY)) || [];
        let sellers = JSON.parse(localStorage.getItem(STORAGE_KEYS.SELLERS)) || [];
        let buyers = JSON.parse(localStorage.getItem(STORAGE_KEYS.BUYERS)) || [];
        
        // Current sort state
        let currentSort = {
            column: null,
            direction: 'asc'
        };
        
        // Check authentication on page load
        document.addEventListener('DOMContentLoaded', function() {
            checkAuth();
            renderAllData();
            setupEventListeners();
        });
        
        function checkAuth() {
            const auth = sessionStorage.getItem(STORAGE_KEYS.AUTH);
            if (auth === 'true') {
                adminControls.classList.add('active');
                logoutBtn.style.display = 'block';
                loginBtn.style.display = 'none';
            } else {
                adminControls.classList.remove('active');
                logoutBtn.style.display = 'none';
                loginBtn.style.display = 'block';
            }
        }
        
        function setupEventListeners() {
            // Login/logout
            loginBtn.addEventListener('click', handleLogin);
            logoutBtn.addEventListener('click', handleLogout);
            
            // Tabs
            tabs.forEach(tab => {
                tab.addEventListener('click', () => switchTab(tab.dataset.tab));
            });
            
            // Add buttons
            addProductBtn.addEventListener('click', () => openProductModal());
            addDeliveryBtn.addEventListener('click', () => openDeliveryModal());
            addSellerBtn.addEventListener('click', () => openSellerModal());
            addBuyerBtn.addEventListener('click', () => openBuyerModal());
            publishBtn.addEventListener('click', publishWebpage);
            
            // Form submissions
            productForm.addEventListener('submit', handleProductSubmit);
            deliveryForm.addEventListener('submit', handleDeliverySubmit);
            sellerForm.addEventListener('submit', handleSellerSubmit);
            buyerForm.addEventListener('submit', handleBuyerSubmit);
            
            // Cancel buttons
            cancelProductBtn.addEventListener('click', () => productModal.classList.remove('active'));
            cancelDeliveryBtn.addEventListener('click', () => deliveryModal.classList.remove('active'));
            cancelSellerBtn.addEventListener('click', () => sellerModal.classList.remove('active'));
            cancelBuyerBtn.addEventListener('click', () => buyerModal.classList.remove('active'));
            
            // Publish modal buttons
            printBtn.addEventListener('click', () => window.print());
            closePublishBtn.addEventListener('click', () => publishModal.classList.remove('active'));
            
            // Table sorting
            document.querySelectorAll('[data-sort]').forEach(header => {
                header.addEventListener('click', () => {
                    const tableId = header.closest('table').id;
                    const dataType = tableId.replace('-table', '');
                    sortTable(dataType, header.dataset.sort);
                });
            });
        }
        
        function handleLogin() {
            const username = usernameInput.value.trim();
            const password = passwordInput.value.trim();
            
            if (username === ADMIN_USERNAME && password === ADMIN_PASSWORD) {
                sessionStorage.setItem(STORAGE_KEYS.AUTH, 'true');
                checkAuth();
                usernameInput.value = '';
                passwordInput.value = '';
                alert('Login successful! Admin controls enabled.');
            } else {
                alert('Invalid username or password');
            }
        }
        
        function handleLogout() {
            sessionStorage.removeItem(STORAGE_KEYS.AUTH);
            checkAuth();
            alert('Logged out successfully. Admin controls disabled.');
        }
        
        function switchTab(tabName) {
            // Update active tab
            tabs.forEach(tab => {
                if (tab.dataset.tab === tabName) {
                    tab.classList.add('active');
                } else {
                    tab.classList.remove('active');
                }
            });
            
            // Update active content
            tabContents.forEach(content => {
                if (content.id === `${tabName}-content`) {
                    content.classList.add('active');
                } else {
                    content.classList.remove('active');
                }
            });
        }
        
        // Product functions
        function openProductModal(product = null) {
            const title = document.getElementById('product-modal-title');
            const form = document.getElementById('product-form');
            
            if (product) {
                title.textContent = 'Edit Product';
                document.getElementById('product-id').value = product.id;
                document.getElementById('product-name').value = product.name;
                document.getElementById('product-unit').value = product.unit;
                document.getElementById('product-price').value = product.price;
                document.getElementById('product-category').value = product.category;
                document.getElementById('product-availability').value = product.availability;
            } else {
                title.textContent = 'Add Product';
                form.reset();
            }
            
            productModal.classList.add('active');
        }
        
        function handleProductSubmit(e) {
            e.preventDefault();
            
            const id = document.getElementById('product-id').value;
            const name = document.getElementById('product-name').value;
            const unit = document.getElementById('product-unit').value;
            const price = parseFloat(document.getElementById('product-price').value);
            const category = document.getElementById('product-category').value;
            const availability = document.getElementById('product-availability').value;
            
            const product = { id: id || generateId(), name, unit, price, category, availability };
            
            if (id) {
                // Update existing product
                const index = products.findIndex(p => p.id === id);
                if (index !== -1) {
                    products[index] = product;
                }
            } else {
                // Add new product
                products.push(product);
            }
            
            saveProducts();
            productModal.classList.remove('active');
            renderProducts();
        }
        
        function editProduct(id) {
            const product = products.find(p => p.id === id);
            if (product) {
                openProductModal(product);
            }
        }
        
        function deleteProduct(id) {
            if (confirm('Are you sure you want to delete this product?')) {
                products = products.filter(p => p.id !== id);
                saveProducts();
                renderProducts();
            }
        }
        
        function saveProducts() {
            localStorage.setItem(STORAGE_KEYS.PRODUCTS, JSON.stringify(products));
        }
        
        // Delivery functions
        function openDeliveryModal(schedule = null) {
            const title = document.getElementById('delivery-modal-title');
            const form = document.getElementById('delivery-form');
            
            if (schedule) {
                title.textContent = 'Edit Delivery Schedule';
                document.getElementById('delivery-id').value = schedule.id;
                document.getElementById('delivery-type').value = schedule.type;
                document.getElementById('delivery-location').value = schedule.location;
                document.getElementById('delivery-fee').value = schedule.fee;
                document.getElementById('delivery-time').value = schedule.time;
            } else {
                title.textContent = 'Add Delivery Schedule';
                form.reset();
            }
            
            deliveryModal.classList.add('active');
        }
        
        function handleDeliverySubmit(e) {
            e.preventDefault();
            
            const id = document.getElementById('delivery-id').value;
            const type = document.getElementById('delivery-type').value;
            const location = document.getElementById('delivery-location').value;
            const fee = parseFloat(document.getElementById('delivery-fee').value);
            const time = document.getElementById('delivery-time').value;
            
            const schedule = { id: id || generateId(), type, location, fee, time };
            
            if (id) {
                // Update existing schedule
                const index = deliverySchedules.findIndex(d => d.id === id);
                if (index !== -1) {
                    deliverySchedules[index] = schedule;
                }
            } else {
                // Add new schedule
                deliverySchedules.push(schedule);
            }
            
            saveDeliverySchedules();
            deliveryModal.classList.remove('active');
            renderDeliverySchedules();
        }
        
        function editDelivery(id) {
            const schedule = deliverySchedules.find(d => d.id === id);
            if (schedule) {
                openDeliveryModal(schedule);
            }
        }
        
        function deleteDelivery(id) {
            if (confirm('Are you sure you want to delete this delivery schedule?')) {
                deliverySchedules = deliverySchedules.filter(d => d.id !== id);
                saveDeliverySchedules();
                renderDeliverySchedules();
            }
        }
        
        function saveDeliverySchedules() {
            localStorage.setItem(STORAGE_KEYS.DELIVERY, JSON.stringify(deliverySchedules));
        }
        
        // Seller functions
        function openSellerModal(seller = null) {
            const title = document.getElementById('seller-modal-title');
            const form = document.getElementById('seller-form');
            
            if (seller) {
                title.textContent = 'Edit Seller';
                document.getElementById('seller-id').value = seller.id;
                document.getElementById('seller-name').value = seller.name;
                document.getElementById('seller-phone').value = seller.phone;
                document.getElementById('seller-address').value = seller.address;
                document.getElementById('seller-category').value = seller.category;
                document.getElementById('seller-priceRange').value = seller.priceRange;
            } else {
                title.textContent = 'Add Seller';
                form.reset();
            }
            
            sellerModal.classList.add('active');
        }
        
        function handleSellerSubmit(e) {
            e.preventDefault();
            
            const id = document.getElementById('seller-id').value;
            const name = document.getElementById('seller-name').value;
            const phone = document.getElementById('seller-phone').value;
            const address = document.getElementById('seller-address').value;
            const category = document.getElementById('seller-category').value;
            const priceRange = document.getElementById('seller-priceRange').value;
            
            const seller = { id: id || generateId(), name, phone, address, category, priceRange };
            
            if (id) {
                // Update existing seller
                const index = sellers.findIndex(s => s.id === id);
                if (index !== -1) {
                    sellers[index] = seller;
                }
            } else {
                // Add new seller
                sellers.push(seller);
            }
            
            saveSellers();
            sellerModal.classList.remove('active');
            renderSellers();
        }
        
        function editSeller(id) {
            const seller = sellers.find(s => s.id === id);
            if (seller) {
                openSellerModal(seller);
            }
        }
        
        function deleteSeller(id) {
            if (confirm('Are you sure you want to delete this seller?')) {
                sellers = sellers.filter(s => s.id !== id);
                saveSellers();
                renderSellers();
            }
        }
        
        function saveSellers() {
            localStorage.setItem(STORAGE_KEYS.SELLERS, JSON.stringify(sellers));
        }
        
        // Buyer functions
        function openBuyerModal(buyer = null) {
            const title = document.getElementById('buyer-modal-title');
            const form = document.getElementById('buyer-form');
            
            if (buyer) {
                title.textContent = 'Edit Buyer';
                document.getElementById('buyer-id').value = buyer.id;
                document.getElementById('buyer-name').value = buyer.name;
                document.getElementById('buyer-phone').value = buyer.phone;
                document.getElementById('buyer-address').value = buyer.address;
                document.getElementById('buyer-interest').value = buyer.interest;
                document.getElementById('buyer-maxPrice').value = buyer.maxPrice;
            } else {
                title.textContent = 'Add Buyer';
                form.reset();
            }
            
            buyerModal.classList.add('active');
        }
        
        function handleBuyerSubmit(e) {
            e.preventDefault();
            
            const id = document.getElementById('buyer-id').value;
            const name = document.getElementById('buyer-name').value;
            const phone = document.getElementById('buyer-phone').value;
            const address = document.getElementById('buyer-address').value;
            const interest = document.getElementById('buyer-interest').value;
            const maxPrice = parseFloat(document.getElementById('buyer-maxPrice').value);
            
            const buyer = { id: id || generateId(), name, phone, address, interest, maxPrice };
            
            if (id) {
                // Update existing buyer
                const index = buyers.findIndex(b => b.id === id);
                if (index !== -1) {
                    buyers[index] = buyer;
                }
            } else {
                // Add new buyer
                buyers.push(buyer);
            }
            
            saveBuyers();
            buyerModal.classList.remove('active');
            renderBuyers();
        }
        
        function editBuyer(id) {
            const buyer = buyers.find(b => b.id === id);
            if (buyer) {
                openBuyerModal(buyer);
            }
        }
        
        function deleteBuyer(id) {
            if (confirm('Are you sure you want to delete this buyer?')) {
                buyers = buyers.filter(b => b.id !== id);
                saveBuyers();
                renderBuyers();
            }
        }
        
        function saveBuyers() {
            localStorage.setItem(STORAGE_KEYS.BUYERS, JSON.stringify(buyers));
        }
        
        // Render functions
        function renderAllData() {
            renderProducts();
            renderDeliverySchedules();
            renderSellers();
            renderBuyers();
        }
        
        function renderProducts() {
            const tbody = document.getElementById('products-body');
            tbody.innerHTML = '';
            
            products.forEach(product => {
                const tr = document.createElement('tr');
                
                tr.innerHTML = `
                    <td>${product.name}</td>
                    <td>${product.unit}</td>
                    <td>$${product.price.toFixed(2)}</td>
                    <td>${product.category}</td>
                    <td>${product.availability}</td>
                    <td class="actions">
                        ${sessionStorage.getItem(STORAGE_KEYS.AUTH) === 'true' ? `
                            <button onclick="editProduct('${product.id}')" class="btn-warning">Edit</button>
                            <button onclick="deleteProduct('${product.id}')" class="btn-danger">Delete</button>
                        ` : ''}
                    </td>
                `;
                
                tbody.appendChild(tr);
            });
        }
        
        function renderDeliverySchedules() {
            const tbody = document.getElementById('delivery-body');
            tbody.innerHTML = '';
            
            deliverySchedules.forEach(schedule => {
                const tr = document.createElement('tr');
                
                tr.innerHTML = `
                    <td>${schedule.type}</td>
                    <td>${schedule.location}</td>
                    <td>$${schedule.fee.toFixed(2)}</td>
                    <td>${schedule.time}</td>
                    <td class="actions">
                        ${sessionStorage.getItem(STORAGE_KEYS.AUTH) === 'true' ? `
                            <button onclick="editDelivery('${schedule.id}')" class="btn-warning">Edit</button>
                            <button onclick="deleteDelivery('${schedule.id}')" class="btn-danger">Delete</button>
                        ` : ''}
                    </td>
                `;
                
                tbody.appendChild(tr);
            });
        }
        
        function renderSellers() {
            const tbody = document.getElementById('sellers-body');
            tbody.innerHTML = '';
            
            sellers.forEach(seller => {
                const tr = document.createElement('tr');
                
                tr.innerHTML = `
                    <td>${seller.name}</td>
                    <td>${seller.phone}</td>
                    <td>${seller.address}</td>
                    <td>${seller.category}</td>
                    <td>${seller.priceRange}</td>
                    <td class="actions">
                        ${sessionStorage.getItem(STORAGE_KEYS.AUTH) === 'true' ? `
                            <button onclick="editSeller('${seller.id}')" class="btn-warning">Edit</button>
                            <button onclick="deleteSeller('${seller.id}')" class="btn-danger">Delete</button>
                        ` : ''}
                    </td>
                `;
                
                tbody.appendChild(tr);
            });
        }
        
        function renderBuyers() {
            const tbody = document.getElementById('buyers-body');
            tbody.innerHTML = '';
            
            buyers.forEach(buyer => {
                const tr = document.createElement('tr');
                
                tr.innerHTML = `
                    <td>${buyer.name}</td>
                    <td>${buyer.phone}</td>
                    <td>${buyer.address}</td>
                    <td>${buyer.interest}</td>
                    <td>$${buyer.maxPrice.toFixed(2)}</td>
                    <td class="actions">
                        ${sessionStorage.getItem(STORAGE_KEYS.AUTH) === 'true' ? `
                            <button onclick="editBuyer('${buyer.id}')" class="btn-warning">Edit</button>
                            <button onclick="deleteBuyer('${buyer.id}')" class="btn-danger">Delete</button>
                        ` : ''}
                    </td>
                `;
                
                tbody.appendChild(tr);
            });
        }
        
        // Sort functions
        function sortTable(dataType, column) {
            let data;
            let renderFunction;
            
            switch (dataType) {
                case 'products':
                    data = products;
                    renderFunction = renderProducts;
                    break;
                case 'delivery':
                    data = deliverySchedules;
                    renderFunction = renderDeliverySchedules;
                    break;
                case 'sellers':
                    data = sellers;
                    renderFunction = renderSellers;
                    break;
                case 'buyers':
                    data = buyers;
                    renderFunction = renderBuyers;
                    break;
                default:
                    return;
            }
            
            // Toggle direction if same column is clicked
            if (currentSort.column === column) {
                currentSort.direction = currentSort.direction === 'asc' ? 'desc' : 'asc';
            } else {
                currentSort.column = column;
                currentSort.direction = 'asc';
            }
            
            data.sort((a, b) => {
                let valueA = a[column];
                let valueB = b[column];
                
                // Handle numeric sorting
                if (column.includes('price') || column === 'fee' || column === 'maxPrice') {
                    valueA = parseFloat(valueA);
                    valueB = parseFloat(valueB);
                }
                
                if (valueA < valueB) {
                    return currentSort.direction === 'asc' ? -1 : 1;
                }
                if (valueA > valueB) {
                    return currentSort.direction === 'asc' ? 1 : -1;
                }
                return 0;
            });
            
            // Save sorted data
            switch (dataType) {
                case 'products':
                    products = data;
                    saveProducts();
                    break;
                case 'delivery':
                    deliverySchedules = data;
                    saveDeliverySchedules();
                    break;
                case 'sellers':
                    sellers = data;
                    saveSellers();
                    break;
                case 'buyers':
                    buyers = data;
                    saveBuyers();
                    break;
            }
            
            renderFunction();
        }
        
        // Publish functions
        function publishWebpage() {
            // Render products
            let productsHTML = '<table border="1" cellpadding="5" cellspacing="0" width="100%">';
            productsHTML += '<tr><th>Name</th><th>Unit</th><th>Price/Unit</th><th>Category</th><th>Availability</th></tr>';
            
            products.forEach(product => {
                productsHTML += `
                    <tr>
                        <td>${product.name}</td>
                        <td>${product.unit}</td>
                        <td>$${product.price.toFixed(2)}</td>
                        <td>${product.category}</td>
                        <td>${product.availability}</td>
                    </tr>
                `;
            });
            
            productsHTML += '</table>';
            document.getElementById('publish-products').innerHTML = productsHTML;
            
            // Render delivery schedules
            let deliveryHTML = '<table border="1" cellpadding="5" cellspacing="0" width="100%">';
            deliveryHTML += '<tr><th>Delivery Type</th><th>Location</th><th>Fee</th><th>Schedule Time</th></tr>';
            
            deliverySchedules.forEach(schedule => {
                deliveryHTML += `
                    <tr>
                        <td>${schedule.type}</td>
                        <td>${schedule.location}</td>
                        <td>$${schedule.fee.toFixed(2)}</td>
                        <td>${schedule.time}</td>
                    </tr>
                `;
            });
            
            deliveryHTML += '</table>';
            document.getElementById('publish-delivery').innerHTML = deliveryHTML;
            
            // Render sellers
            let sellersHTML = '<table border="1" cellpadding="5" cellspacing="0" width="100%">';
            sellersHTML += '<tr><th>Name</th><th>Phone</th><th>Address</th><th>Product Category</th><th>Price Range</th></tr>';
            
            sellers.forEach(seller => {
                sellersHTML += `
                    <tr>
                        <td>${seller.name}</td>
                        <td>${seller.phone}</td>
                        <td>${seller.address}</td>
                        <td>${seller.category}</td>
                        <td>${seller.priceRange}</td>
                    </tr>
                `;
            });
            
            sellersHTML += '</table>';
            document.getElementById('publish-sellers').innerHTML = sellersHTML;
            
            // Render buyers
            let buyersHTML = '<table border="1" cellpadding="5" cellspacing="0" width="100%">';
            buyersHTML += '<tr><th>Name</th><th>Phone</th><th>Address</th><th>Interest Category</th><th>Max Price</th></tr>';
            
            buyers.forEach(buyer => {
                buyersHTML += `
                    <tr>
                        <td>${buyer.name}</td>
                        <td>${buyer.phone}</td>
                        <td>${buyer.address}</td>
                        <td>${buyer.interest}</td>
                        <td>$${buyer.maxPrice.toFixed(2)}</td>
                    </tr>
                `;
            });
            
            buyersHTML += '</table>';
            document.getElementById('publish-buyers').innerHTML = buyersHTML;
            
            // Show the publish modal
            publishModal.classList.add('active');
        }
        
        // Helper functions
        function generateId() {
            return Date.now().toString(36) + Math.random().toString(36).substr(2);
        }
        
        // Make functions available globally for inline event handlers
        window.editProduct = editProduct;
        window.deleteProduct = deleteProduct;
        window.editDelivery = editDelivery;
        window.deleteDelivery = deleteDelivery;
        window.editSeller = editSeller;
        window.deleteSeller = deleteSeller;
        window.editBuyer = editBuyer;
        window.deleteBuyer = deleteBuyer;
    </script>
</body>
</html>
