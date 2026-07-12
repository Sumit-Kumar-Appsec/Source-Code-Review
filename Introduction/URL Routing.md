URL Routing

What is a URL?
When you type something like this in a browser:
https://amazon.com/products/shoes/123
It has 3 parts:

https:// → the protocol
amazon.com → the server's address (which machine to go to)
/products/shoes/123 → the path (tells the server "this is what I want")

What is Routing?
When your browser sends a request to amazon.com/products/shoes/123, Amazon's server (the backend) has to figure out:

"Which code should run for this path?"

This decision-making process is called routing.
Think of the backend as an office building, and the URL path as an address that tells you which department to go to:

/products → goes to the Products department's code
/cart → goes to the Cart department's code
/login → goes to the Login department's code

Inside the backend, there is a "router" that keeps a list like this:
GET  /products        → run showAllProducts()
GET  /products/:id    → run showOneProduct()
POST /login            → run loginUser()
POST /cart/add         → run addToCart()
When a request comes in, the router matches it against this list and calls the right function.
Real Example (using a framework like Node.js/Express)
jsapp.get('/products', function(req, res) {
   // code to show all products runs here
   res.send("Here are all the products");
});

app.get('/products/:id', function(req, res) {
   // :id is a variable, picked up from the URL
   let productId = req.params.id;
   res.send("Product number: " + productId);
});
Here, :id in /products/:id is a placeholder. If you go to /products/123, the backend receives id = 123 — and uses that to search the database.
Why GET and POST matter
Along with the URL, there is also a "method" that tells you what action to take:

GET → asking for data (like viewing a page)
POST → sending data (like submitting a form, logging in)
PUT → updating data
DELETE → deleting data

On the same URL /products/123:

GET /products/123 → show details of that product
DELETE /products/123 → delete that product

The router treats these two differently, even though the path is the same.
The Full Flow (step by step)

Browser sends a request: GET /products/123
The request reaches the server
The router checks: "It's a GET request, path is /products/:id" → match found
Router calls the matching function, and passes id=123 to it
The function fetches product 123's details from the database
The response is sent back to the browser (as HTML or JSON)
