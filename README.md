<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">
    <title>Product Sale App</title>
    <style>
        body {
            background-color: #f5f5f5;
        }
        .nav-link.active {
            color: #fff;
        }
    </style>
</head>
<body>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark fixed-top">
        <div class="container">
            <a class="navbar-brand" href="#products">Product Sale App</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarSupportedContent">
                <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
                    <li class="nav-item">
                        <a class="nav-link active" aria-current="page" href="#products">Products</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="#cart">Cart</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    <div class="container mt-5 pt-5">
        <div id="products">
            <h1 class="mb-4">Products</h1>
            <div class="row">
                <div class="col-lg-4 mb-4">
                    <div class="card">
                        <img src="https://via.placeholder.com/200" class="card-img-top" alt="...">
                        <div class="card-body">
                            <h5 class="card-title">Product 1</h5>
                            <p class="card-text">This is a product description.</p>
                            <button class="btn btn-primary" onclick="addToCart('Goat ', 13.99)">Add to Cart ($13.99)</button>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4 mb-4">
                    <div class="card">
                        <img src="https://via.placeholder.com/200" class="card-img-top" alt="...">
                        <div class="card-body">
                            <h5 class="card-title">Product 2</h5>
                            <p class="card-text">This is another product description.</p>
                            <button class="btn btn-primary" onclick="addToCart('goat 2', 6.99)">Add to Cart ($6.99)</button>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4 mb-4">
                    <div class="card">
                        <img src="https://via.placeholder.com/200" class="card-img-top" alt="...">
                        <div class="card-body">
                            <h5 class="card-title">Product 3</h5>
                            <p class="card-text">This is a third product description.</p>
                            <button class="btn btn-primary" onclick="addToCart('Product 3', 9.99)">Add to Cart ($9.99)</button>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div id="cart" class="mt-5 pt-5">
            <h1 class="mb-4">Your Cart</h1>
            <table class="table">
                <thead>
                    <tr>
                        <th>Product</th>
                        <th>Price</th>
                        <th>Remove</th>
                    </tr>
                </thead>
                <tbody id="cart-table-body">
                </tbody>
            </table>
            <p id="total-cost"></p>
            <button class="btn btn-primary" onclick="checkout()">Checkout</button>
        </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>
    <script>
        let cart = [];

        function addToCart(productName, price) {
            cart.push({ productName, price });
            updateCartTable();
        }

        function updateCartTable() {
            let cartTableBody = document.getElementById('cart-table-body');
            cartTableBody.innerHTML = '';
            let total = 0;
            cart.forEach((item, index) => {
                let row = document.createElement('tr');
                let productNameCell = document.createElement('td');
                productNameCell.textContent = item.productName;
                let priceCell = document.createElement('td');
                priceCell.textContent = `$${item.price}`;
                let removeCell = document.createElement('td');
                let removeButton = document.createElement('button');
                removeButton.textContent = 'Remove';
                removeButton.onclick = function() {
                    cart.splice(index, 1);
                    updateCartTable();
                };
                removeCell.appendChild(removeButton);
                row.appendChild(productNameCell);
                row.appendChild(priceCell);
                row.appendChild(removeCell);
                cartTableBody.appendChild(row);
                total += item.price;
            });
            document.getElementById('total-cost').textContent = `Total: $${total.toFixed(2)}`;
        }

        function checkout() {
            let modal = document.createElement('div');
            modal.className = 'modal fade';
            modal.id = 'checkout-modal';
            modal.tabIndex = -1;
            modal.role = 'dialog';
            let modalDialog = document.createElement('div');
            modalDialog.className = 'modal-dialog';
            let modalContent = document.createElement('div');
            modalContent.className = 'modal-content';
            let modalHeader = document.createElement('div');
            modalHeader.className = 'modal-header';
            let modalTitle = document.createElement('h5');
            modalTitle.className = 'modal-title';
            modalTitle.textContent = 'Checkout';
            let modalCloseButton = document.createElement('button');
            modalCloseButton.type = 'button';
            modalCloseButton.className = 'btn-close';
            modalCloseButton.onclick = function() {
                modal.remove();
            };
            modalHeader.appendChild(modalTitle);
            modalHeader.appendChild(modalCloseButton);
            modalContent.appendChild(modalHeader);
            let modalBody = document.createElement('div');
            modalBody.className = 'modal-body';
            let paymentMethodSelect = document.createElement('select');
            paymentMethodSelect.className = 'form-select';
            let cardOption = document.createElement('option');
            cardOption.value = 'card';
            cardOption.textContent = 'Card';
            paymentMethodSelect.appendChild(cardOption);
            let paymentForm = document.createElement('form');
            paymentForm.id = 'payment-form';
            let cardNumberLabel = document.createElement('label');
            cardNumberLabel.textContent = 'Card Number:';
            let cardNumberInput = document.createElement('input');
            cardNumberInput.type = 'text';
            cardNumberInput.className = 'form-control';
            cardNumberInput.id = 'card-number';
            let expirationDateLabel = document.createElement('label');
            expirationDateLabel.textContent = 'Expiration Date (MM/YY):';
            let expirationDateInput = document.createElement('input');
            expirationDateInput.type = 'text';
            expirationDateInput.className = 'form-control';
            expirationDateInput.id = 'expiration-date';
            let cvvLabel = document.createElement('label');
            cvvLabel.textContent = 'CVV:';
            let cvvInput = document.createElement('input');
            cvvInput.type = 'text';
            cvvInput.className = 'form-control';
            cvvInput.id = 'cvv';
            let submitButton = document.createElement('button');
            submitButton.type = 'submit';
            submitButton.className = 'btn btn-primary';
            submitButton.textContent = 'Pay Now';
            paymentForm.appendChild(cardNumberLabel);
            paymentForm.appendChild(cardNumberInput);
            paymentForm.appendChild(expirationDateLabel);
            paymentForm.appendChild(expirationDateInput);
            paymentForm.appendChild(cvvLabel);
            paymentForm.appendChild(cvvInput);
            paymentForm.appendChild(submitButton);
            paymentForm.onsubmit = function(event) {
                event.preventDefault();
                let cardNumber = document.getElementById('card-number').value;
                let expirationDate = document.getElementById('expiration-date').value;
                let cvv = document.getElementById('cvv').value;
                if (cardNumber && expirationDate && cvv) {
                    let paymentConfirmationModal = document.createElement('div');
                    paymentConfirmationModal.className = 'modal fade';
                    paymentConfirmationModal.id = 'payment-confirmation-modal';
                    paymentConfirmationModal.tabIndex = -1;
                    paymentConfirmationModal.role = 'dialog';
                    let paymentConfirmationModalDialog = document.createElement('div');
                    paymentConfirmationModalDialog.className = 'modal-dialog';
                    let paymentConfirmationModalContent = document.createElement('div');
                    paymentConfirmationModalContent.className = 'modal-content';
                    let paymentConfirmationModalHeader = document.createElement('div');
                    paymentConfirmationModalHeader.className = 'modal-header';
                    let paymentConfirmationModalTitle = document.createElement('h5');
                    paymentConfirmationModalTitle.className = 'modal-title';
                    paymentConfirmationModalTitle.textContent = 'Payment Confirmation';
                    let paymentConfirmationModalCloseButton = document.createElement('button');
                    paymentConfirmationModalCloseButton.type = 'button';
                    paymentConfirmationModalCloseButton.className = 'btn-close';
                    paymentConfirmationModalCloseButton.onclick = function() {
                        paymentConfirmationModal.remove();
                    };
                    paymentConfirmationModalHeader.appendChild(paymentConfirmationModalTitle);
                    paymentConfirmationModalHeader.appendChild(paymentConfirmationModalCloseButton);
                    paymentConfirmationModalContent.appendChild(paymentConfirmationModalHeader);
                    let paymentConfirmationModalBody = document.createElement('div');
                    paymentConfirmationModalBody.className = 'modal-body';
                    paymentConfirmationModalBody.textContent = 'Payment successful. Thank you for your purchase!';
                    paymentConfirmationModalContent.appendChild(paymentConfirmationModalBody);
                    paymentConfirmationModalDialog.appendChild(paymentConfirmationModalContent);
                    paymentConfirmationModal.appendChild(paymentConfirmationModalDialog);
                    document.body.appendChild(paymentConfirmationModal);
                    let paymentConfirmationModalInstance = new bootstrap.Modal(paymentConfirmationModal, { keyboard: false });
                    paymentConfirmationModalInstance.show();
                    modal.remove();
                    cart = [];
                    updateCartTable();
                } else {
                    alert('Please fill in all fields.');
                }
            };
            modalBody.appendChild(paymentMethodSelect);
            modalBody.appendChild(paymentForm);
            modalContent.appendChild(modalBody);
            modalDialog.appendChild(modalContent);
            modal.appendChild(modalDialog);
            document.body.appendChild(modal);
            let modalInstance = new bootstrap.Modal(modal, { keyboard: false });
            modalInstance.show();
        }
    </script>
</body>
</html>
