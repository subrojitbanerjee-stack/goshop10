# goshop10
Goshop 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>goshop | Fresh Finds</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@3/dist/email.min.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/dist/css/all.min.css">
    <style>
        .no-scrollbar::-webkit-scrollbar { display: none; }
        .hero-bg { background-image: url('https://i.ibb.co/HfyQ1LYr/20260227-232513.jpg'); background-size: cover; background-position: center; }
        .cart-modal { transform: translateX(100%); transition: transform 0.3s ease-in-out; }
        .cart-modal.open { transform: translateX(0); }
    </style>
</head>
<body class="bg-gray-50 font-sans">
 <header class="relative h-screen flex items-center justify-center text-center text-white hero-bg">
        <div class="absolute inset-0 bg-black opacity-50"></div>
        <div class="relative z-10 px-4">
            <h1 class="text-6xl font-black uppercase tracking-tighter mb-2">goshop</h1>
            <p class="text-xl font-light mb-8">Fresh Finds, Best Prices</p>
            <a href="#products" class="bg-white text-black px-10 py-4 font-bold rounded-full hover:bg-yellow-400 transition transform hover:scale-105">SHOP NOW</a>
        </div>
    </header>

  <section id="products" class="py-16 px-4 max-w-7xl mx-auto">
        <h2 class="text-3xl font-black mb-8 border-l-8 border-black pl-4 uppercase">Featured Goods</h2>
        
  <div id="carousel" class="flex overflow-x-auto gap-6 no-scrollbar snap-x pb-8">
            </div>
    </section>

 <div id="cartDrawer" class="fixed inset-y-0 right-0 w-full md:w-96 bg-white z-50 shadow-2xl cart-modal p-6 flex flex-col">
        <div class="flex justify-between items-center border-b pb-4 mb-4">
            <h2 class="text-2xl font-black">YOUR CART</h2>
            <button onclick="toggleCart()" class="text-2xl">&times;</button>
        </div>
        <div id="cartItems" class="flex-grow overflow-y-auto space-y-4">
            </div>
        <div class="border-t pt-4">
            <form id="orderForm" class="space-y-3">
                <input type="text" id="custName" placeholder="Your Name" required class="w-full border p-2 rounded">
                <input type="tel" id="custPhone" placeholder="Phone Number" required class="w-full border p-2 rounded">
                <div class="text-xl font-bold mb-4">Total: $<span id="cartTotal">0</span></div>
                <button type="submit" class="w-full bg-black text-white py-3 font-bold hover:bg-gray-800 transition">SEND ORDER VIA EMAIL</button>
            </form>
        </div>
    </div>

 <div class="fixed bottom-6 right-6 flex flex-col gap-4 z-40">
        <a href="https://wa.me/9732072792" target="_blank" class="bg-green-500 text-white w-14 h-14 rounded-full flex items-center justify-center shadow-lg hover:scale-110 transition">
            <i class="fab fa-whatsapp text-3xl"></i>
        </a>
        <button onclick="toggleCart()" class="bg-black text-white w-14 h-14 rounded-full flex items-center justify-center shadow-lg hover:scale-110 transition relative">
            <i class="fas fa-shopping-cart text-xl"></i>
            <span id="cartCount" class="absolute -top-1 -right-1 bg-red-500 text-xs w-6 h-6 rounded-full flex items-center justify-center border-2 border-white">0</span>
        </button>
    </div>
    <script>
        // EmailJS Init
        (function() {
            emailjs.init("YOUR_PUBLIC_KEY");
        })();
    const products = [
            { id: 1, name: "Premium Pick", price: 25, img: "https://i.ibb.co/bR8R0BvV/IMG-0895.jpg" },
            { id: 2, name: "Urban Essential", price: 40, img: "https://i.ibb.co/yFr0P33p/image.jpg" },
            { id: 3, name: "Daily Blend", price: 15, img: "https://i.ibb.co/chKzpf73/image.jpg" },
            { id: 4, name: "Classic Item", price: 30, img: "https://i.ibb.co/PG9tSjFw/image.jpg" },
            { id: 5, name: "Sleek Gadget", price: 55, img: "https://i.ibb.co/8ngXmC92/515-R4-CLGOHL-AC-UF894-1000-QL80-FMwebp.webp" },
            { id: 6, name: "Organic Pack", price: 20, img: "https://i.ibb.co/gMF8wMMH/d9a73e0a7d79e654af7353c1e33744b2.jpg" }
        ];
      let cart = [];
        // Render Products
        const carousel = document.getElementById('carousel');
        products.forEach(p => {
            carousel.innerHTML += `
                <div class="min-w-[280px] snap-center bg-white rounded-2xl overflow-hidden shadow-sm hover:shadow-xl transition duration-300">
                    <img src="${p.img}" alt="${p.name}" class="w-full h-64 object-cover">
                    <div class="p-5">
                        <h3 class="font-bold text-lg uppercase">${p.name}</h3>
                        <p class="text-gray-500 mb-4">$${p.price}</p>
                        <button onclick="addToCart(${p.id})" class="w-full border-2 border-black py-2 font-bold hover:bg-black hover:text-white transition">ADD TO CART</button>
                    </div>
                </div>
            `;
        });
        function addToCart(id) {
            const product = products.find(p => p.id === id);
            cart.push(product);
            updateCart();
            if(window.innerWidth > 768) toggleCart(true);
        }
    function updateCart() {
            document.getElementById('cartCount').innerText = cart.length;
            const cartList = document.getElementById('cartItems');
            const totalEl = document.getElementById('cartTotal');
            cartList.innerHTML = '';
            let total = 0;
 cart.forEach((item, index) => {
                total += item.price;
                cartList.innerHTML += `
                    <div
                    class="flex items-center gap-4 bg-gray-50 p-2 rounded">
                        <img src="${item.img}" class="w-16 h-16 object-cover rounded">
                        <div class="flex-grow">
                            <h4 class="font-bold text-sm">${item.name}</h4>
                            <p>$${item.price}</p>
                        </div>
                        <button onclick="removeFromCart(${index})" class="text-red-500">&times;</button>
                    </div>
                `;
            });
            totalEl.innerText = total;
        }
        function removeFromCart(index) {
            cart.splice(index, 1);
            updateCart();
        }
        function toggleCart(forceOpen = false) {
            const drawer = document.getElementById('cartDrawer');
            if(forceOpen) drawer.classList.add('open');
            else drawer.classList.toggle('open');
        }
        // Email Submission
        document.getElementById('orderForm').addEventListener('submit', function(e) {
            e.preventDefault();
            if(cart.length === 0) return alert("Cart is empty!");
  const itemList = cart.map(i => `${i.name} ($${i.price})`).join(", ");
            const templateParams = {
                customer_name: document.getElementById('custName').value,
                phone: document.getElementById('custPhone').value,
                items: itemList,
                to_email: 'subrojitbanerjee@gmail.com'
            };
            emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', templateParams)
                .then(() => {
                    alert("Order sent successfully!");
                    cart = [];
                    updateCart();
                    toggleCart();
                }, (err) => {
                    alert("Failed to send order. Check EmailJS config.");
                    console.log(err);
                });
        });
    </script>
</body>
</html>
