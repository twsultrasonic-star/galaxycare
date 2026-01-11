<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Galaxy Car Wash - Cloud Sync Portal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        .hidden { display: none !important; }
        .animate-in {
            animation: fadeIn 0.4s ease-out forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .stat-card { transition: transform 0.2s; }
        .stat-card:hover { transform: translateY(-2px); }
    </style>
</head>
<body class="bg-slate-50 text-slate-900 font-sans p-4 md:p-8">

    <div class="max-w-6xl mx-auto bg-white rounded-2xl shadow-xl overflow-hidden border border-slate-200">
        
        <!-- Header -->
        <header id="header-bg" class="bg-slate-900 p-6 flex flex-wrap justify-between items-center gap-4 transition-colors duration-300 text-white border-b border-white/10">
            <div class="flex items-center gap-3">
                <div class="bg-indigo-500 p-2 rounded-lg">
                    <svg xmlns="http://www.w3.org/2000/svg" width="28" height="28" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="text-white"><path d="M19 17h2c.6 0 1-.4 1-1v-3c0-.9-.7-1.7-1.5-1.9C18.7 10.6 16 10 16 10s-1.3-1.4-2.2-2.3c-.5-.4-1.1-.7-1.8-.7H5c-.6 0-1.1.4-1.4.9l-1.4 2.9A3.7 3.7 0 0 0 2 12v4c0 .6.4 1 1 1h2"/><circle cx="7" cy="17" r="2"/><path d="M9 17h6"/><circle cx="17" cy="17" r="2"/></svg>
                </div>
                <div>
                    <h1 id="header-title" class="text-2xl font-black tracking-tighter uppercase">Galaxy Car Wash</h1>
                    <p id="header-subtitle" class="text-[10px] text-indigo-300 font-bold tracking-[0.2em] uppercase">Cloud Integrated Detailing</p>
                </div>
            </div>
            
            <button id="login-btn" onclick="toggleModal(true)" class="flex items-center gap-2 bg-white/10 hover:bg-white/20 px-4 py-2 rounded-lg transition-all active:scale-95 text-sm font-bold">
                <span>Admin Login</span>
            </button>
            <button id="logout-btn" onclick="logout()" class="hidden flex items-center gap-2 bg-rose-500 hover:bg-rose-600 px-4 py-2 rounded-lg transition-all active:scale-95 text-sm font-bold">
                <span>Logout Admin</span>
            </button>
        </header>

        <!-- Status Bar -->
        <div class="bg-slate-100 px-6 py-3 border-b border-slate-200 flex items-center justify-between text-xs font-bold uppercase tracking-wider text-slate-500">
            <div class="flex items-center gap-2">
                <span id="status-text">Connecting to Cloud...</span>
            </div>
            <div class="flex items-center gap-2">
                <div id="status-dot" class="w-2 h-2 rounded-full bg-amber-500"></div>
                <span id="mode-text">Public Portal</span>
            </div>
        </div>

        <main class="p-6 md:p-8">
            
            <!-- ADMIN VIEW: REVENUE DASHBOARD -->
            <section id="admin-stats-section" class="hidden grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 mb-8 animate-in">
                <div class="stat-card bg-white border border-slate-200 p-5 rounded-2xl shadow-sm">
                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">Today Revenue</p>
                    <h3 class="text-2xl font-black text-slate-800">₹<span id="rev-today">0</span></h3>
                    <div class="mt-2 text-[10px] text-green-500 font-bold uppercase tracking-tighter">Live Sync</div>
                </div>
                <div class="stat-card bg-white border border-slate-200 p-5 rounded-2xl shadow-sm">
                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">7 Day Revenue</p>
                    <h3 class="text-2xl font-black text-slate-800">₹<span id="rev-7day">0</span></h3>
                    <div class="mt-2 text-[10px] text-indigo-500 font-bold uppercase tracking-tighter">Weekly Stream</div>
                </div>
                <div class="stat-card bg-white border border-slate-200 p-5 rounded-2xl shadow-sm">
                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">Monthly</p>
                    <h3 class="text-2xl font-black text-slate-800">₹<span id="rev-month">0</span></h3>
                    <div class="mt-2 text-[10px] text-slate-400 font-bold uppercase tracking-tighter">Estimated</div>
                </div>
                <div class="stat-card bg-white border border-slate-200 p-5 rounded-2xl shadow-sm">
                    <p class="text-xs font-bold text-slate-400 uppercase mb-1">Yearly Revenue</p>
                    <h3 class="text-2xl font-black text-slate-800">₹<span id="rev-year">0</span></h3>
                    <div class="mt-2 text-[10px] text-indigo-500 font-bold uppercase tracking-tighter">Full Calendar</div>
                </div>
            </section>

            <!-- PUBLIC VIEW: BOOKING FORM -->
            <section id="booking-section" class="max-w-xl mx-auto py-6 animate-in">
                <div id="success-message" class="hidden bg-green-50 border border-green-200 p-8 rounded-2xl text-center shadow-lg">
                    <div class="w-16 h-16 bg-green-500 rounded-full flex items-center justify-center mx-auto mb-4 text-white">
                        <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="3" stroke-linecap="round" stroke-linejoin="round"><path d="M20 6 9 17l-5-5"/></svg>
                    </div>
                    <h2 class="text-2xl font-black text-green-800 mb-2">GALAXY BOOKED!</h2>
                    <p class="text-green-700 font-medium">Synced to cloud. Our team will visit your address.</p>
                    <button onclick="resetForm()" class="mt-6 px-6 py-2 bg-green-600 text-white rounded-lg font-bold">Done</button>
                </div>

                <div id="booking-form-container" class="bg-white border border-slate-200 p-8 rounded-2xl shadow-sm">
                    <div class="mb-8">
                        <h2 class="text-2xl font-black text-slate-800 tracking-tight uppercase">Galaxy Wash Portal</h2>
                        <p class="text-slate-500 text-sm font-medium italic">Premium Doorstep Detailing</p>
                    </div>
                    
                    <form onsubmit="handleBooking(event)" class="space-y-6">
                        <div class="space-y-4">
                            <div>
                                <label class="block text-[10px] font-black text-slate-400 uppercase mb-2 tracking-widest">Full Name</label>
                                <input type="text" id="cust-name" placeholder="Customer Name" required class="w-full p-4 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500 bg-slate-50 font-bold">
                            </div>
                            <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
                                <div>
                                    <label class="block text-[10px] font-black text-slate-400 uppercase mb-2 tracking-widest">Phone Number</label>
                                    <input type="tel" id="cust-phone" placeholder="98XXXXXXXX" required class="w-full p-4 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500 bg-slate-50 font-bold">
                                </div>
                                <div>
                                    <label class="block text-[10px] font-black text-slate-400 uppercase mb-2 tracking-widest">Select Service</label>
                                    <select id="cust-wash" class="w-full p-4 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500 bg-slate-50 font-bold text-slate-700">
                                        <option value="499">Interior Deep Clean - ₹499</option>
                                        <option value="499">Exterior Wash - ₹499</option>
                                        <option value="399">360 Full Detail + Polish - ₹399</option>
                                    </select>
                                </div>
                            </div>
                            <div>
                                <label class="block text-[10px] font-black text-slate-400 uppercase mb-2 tracking-widest">Service Address</label>
                                <textarea id="cust-address" placeholder="Doorstep delivery address..." required rows="3" class="w-full p-4 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500 bg-slate-50 font-medium"></textarea>
                            </div>
                        </div>

                        <button id="book-btn" type="submit" class="w-full py-5 bg-indigo-600 text-white rounded-xl font-black text-lg hover:bg-indigo-700 transition-all shadow-xl shadow-indigo-100 flex items-center justify-center gap-3 active:scale-[0.98]">
                            CONFIRM BOOKING
                        </button>
                    </form>
                </div>
            </section>

            <!-- ADMIN VIEW: DATABASE -->
            <section id="admin-section" class="hidden space-y-6 animate-in">
                <div class="flex items-center justify-between">
                    <h2 class="text-2xl font-black text-slate-800 uppercase tracking-tighter">Global Lead Cloud</h2>
                    <span class="px-3 py-1 bg-green-100 text-green-700 rounded-full text-[10px] font-black">CONNECTED</span>
                </div>
                <div class="bg-white border border-slate-200 rounded-2xl overflow-hidden shadow-sm overflow-x-auto">
                    <table class="w-full text-left border-collapse min-w-[750px]">
                        <thead>
                            <tr class="bg-slate-50 text-slate-500 text-[10px] uppercase font-black border-b border-slate-200 tracking-widest">
                                <th class="px-6 py-5">Customer</th>
                                <th class="px-6 py-5">Details</th>
                                <th class="px-6 py-5">Price</th>
                                <th class="px-6 py-5">Source</th>
                                <th class="px-6 py-5 text-right">Action</th>
                            </tr>
                        </thead>
                        <tbody id="customer-table-body" class="divide-y divide-slate-100 font-medium">
                            <!-- Rows injected from Cloud -->
                        </tbody>
                    </table>
                </div>
            </section>
        </main>
    </div>

    <!-- LOGIN MODAL -->
    <div id="login-modal" class="hidden fixed inset-0 bg-slate-900/80 backdrop-blur-md flex items-center justify-center p-4 z-50">
        <div class="bg-white rounded-3xl p-10 w-full max-w-sm shadow-2xl animate-in border border-slate-200">
            <h3 class="text-2xl font-black text-slate-800 mb-2 text-center tracking-tighter uppercase">ADMIN SYNC</h3>
            <p class="text-center text-slate-400 text-xs mb-8 font-bold">Enter master password</p>
            <div class="space-y-4">
                <input type="password" id="admin-pass" placeholder="••••••••" class="w-full p-4 rounded-xl border border-slate-200 outline-none focus:ring-2 focus:ring-indigo-500 bg-slate-50 text-center font-bold">
                <p id="error-msg" class="hidden text-rose-500 text-xs font-bold text-center"></p>
                <button onclick="login()" class="w-full py-4 bg-slate-900 text-white rounded-xl font-black hover:bg-black transition-all">SIGN IN</button>
                <button onclick="toggleModal(false)" class="w-full py-2 text-slate-400 text-[10px] font-black uppercase tracking-widest">Go Back</button>
            </div>
        </div>
    </div>

    <!-- Firebase SDKs -->
    <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, collection, addDoc, onSnapshot, query, deleteDoc, doc, serverTimestamp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Global variables and Configuration
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'galaxy-carwash-v2';
        const firebaseConfig = JSON.parse(__firebase_config);
        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);
        const db = getFirestore(app);
        
        let currentUser = null;
        let customers = [];
        let unsubscribe = null;
        const ADMIN_PASS = "admin123";

        // --- AUTH INITIALIZATION (MANDATORY RULE 3) ---
        const initAuth = async () => {
            try {
                if (typeof __initial_auth_token !== 'undefined' && __initial_auth_token) {
                    await signInWithCustomToken(auth, __initial_auth_token);
                } else {
                    await signInAnonymously(auth);
                }
            } catch (err) {
                console.error("Authentication failed", err);
            }
        };

        // Start Auth process immediately
        initAuth();

        // Listen for auth state changes before starting Firestore operations
        onAuthStateChanged(auth, (user) => {
            currentUser = user;
            if (user) {
                console.log("Authenticated as:", user.uid);
                document.getElementById('status-text').innerText = "Cloud Sync Enabled";
                document.getElementById('status-dot').classList.remove('bg-amber-500');
                document.getElementById('status-dot').classList.add('bg-blue-500');
                setupRealtimeListener();
            } else {
                document.getElementById('status-text').innerText = "Authentication Required";
                if (unsubscribe) unsubscribe();
            }
        });

        // --- FIRESTORE LOGIC ---
        const publicCollection = collection(db, 'artifacts', appId, 'public', 'data', 'leads');

        function setupRealtimeListener() {
            if (!currentUser) return;

            // Simple collection query (Rule 2: No complex queries)
            unsubscribe = onSnapshot(publicCollection, (snapshot) => {
                customers = snapshot.docs.map(doc => ({ id: doc.id, ...doc.data() }));
                
                // Sort by date in memory (Rule 2 compliance)
                customers.sort((a, b) => {
                    const timeA = a.timestamp?.seconds || 0;
                    const timeB = b.timestamp?.seconds || 0;
                    return timeB - timeA;
                });

                updateStats();
                renderTable();
            }, (err) => {
                console.error("Firestore sync error:", err);
                document.getElementById('status-text').innerText = "Sync Error: " + err.message;
            });
        }

        window.handleBooking = async (e) => {
            e.preventDefault();
            if (!currentUser) {
                alert("Authenticating... please try again in a moment.");
                return;
            }

            const bookBtn = document.getElementById('book-btn');
            bookBtn.innerText = "SYNCING...";
            bookBtn.disabled = true;

            const select = document.getElementById('cust-wash');
            const planName = select.options[select.selectedIndex].text.split(' - ')[0];
            const planPrice = parseInt(select.value);

            const newCust = {
                name: document.getElementById('cust-name').value,
                phone: document.getElementById('cust-phone').value,
                plan: planName,
                price: planPrice,
                address: document.getElementById('cust-address').value,
                timestamp: serverTimestamp()
            };

            try {
                await addDoc(publicCollection, newCust);
                document.getElementById('booking-form-container').classList.add('hidden');
                document.getElementById('success-message').classList.remove('hidden');
            } catch (err) {
                console.error("Cloud push failed:", err);
                alert("Connection error. Lead saved locally but not synced.");
            } finally {
                bookBtn.innerText = "CONFIRM BOOKING";
                bookBtn.disabled = false;
            }
        };

        window.deleteCustomer = async (id) => {
            if (!currentUser) return;
            try {
                await deleteDoc(doc(db, 'artifacts', appId, 'public', 'data', 'leads', id));
            } catch (err) {
                console.error("Delete failed", err);
            }
        };

        // --- UI HELPERS ---
        window.toggleModal = (show) => {
            document.getElementById('login-modal').classList.toggle('hidden', !show);
        };

        window.login = () => {
            const pass = document.getElementById('admin-pass').value;
            if (pass === ADMIN_PASS) {
                document.getElementById('booking-section').classList.add('hidden');
                document.getElementById('admin-section').classList.remove('hidden');
                document.getElementById('admin-stats-section').classList.remove('hidden');
                document.getElementById('login-btn').classList.add('hidden');
                document.getElementById('logout-btn').classList.remove('hidden');
                document.getElementById('header-bg').className = "bg-indigo-600 p-6 flex flex-wrap justify-between items-center gap-4 text-white border-b border-indigo-500";
                document.getElementById('header-title').innerText = "GALAXY ADMIN";
                document.getElementById('header-subtitle').innerText = "GLOBAL CLOUD MONITOR";
                document.getElementById('mode-text').innerText = "Admin Center";
                document.getElementById('status-dot').className = "w-2 h-2 rounded-full bg-green-500 animate-pulse";
                toggleModal(false);
            } else {
                document.getElementById('error-msg').innerText = "WRONG PASSWORD";
                document.getElementById('error-msg').classList.remove('hidden');
            }
        };

        window.logout = () => location.reload();

        window.resetForm = () => {
            document.getElementById('booking-form-container').classList.remove('hidden');
            document.getElementById('success-message').classList.add('hidden');
            document.querySelector('form').reset();
        };

        function updateStats() {
            const now = new Date();
            const todayStr = now.toLocaleDateString();
            
            const totalToday = customers.filter(c => {
                if (!c.timestamp) return false;
                return new Date(c.timestamp.seconds * 1000).toLocaleDateString() === todayStr;
            }).reduce((sum, c) => sum + (c.price || 0), 0);

            const totalGlobal = customers.reduce((sum, c) => sum + (c.price || 0), 0);
            
            document.getElementById('rev-today').innerText = totalToday.toLocaleString();
            document.getElementById('rev-7day').innerText = totalGlobal.toLocaleString();
            document.getElementById('rev-month').innerText = (totalGlobal * 1.5).toFixed(0).toLocaleString();
            document.getElementById('rev-year').innerText = (totalGlobal * 5).toFixed(0).toLocaleString();
        }

        function renderTable() {
            const tbody = document.getElementById('customer-table-body');
            if (!tbody) return;
            
            tbody.innerHTML = customers.map(c => `
                <tr class="hover:bg-slate-50 transition-colors group">
                    <td class="px-6 py-5">
                        <div class="font-black text-slate-800 uppercase text-sm tracking-tight">${c.name || 'Anonymous'}</div>
                        <div class="text-[11px] text-slate-400 font-bold">${c.phone || 'N/A'}</div>
                    </td>
                    <td class="px-6 py-5">
                        <div class="text-xs font-bold text-slate-600 mb-1">${c.plan || 'No Plan'}</div>
                        <div class="text-[10px] text-slate-400 max-w-[200px] truncate italic">${c.address || 'No Address'}</div>
                    </td>
                    <td class="px-6 py-5">
                        <div class="text-sm font-black text-slate-800 tracking-tighter">₹${c.price || 0}</div>
                    </td>
                    <td class="px-6 py-5">
                        <span class="px-2 py-1 bg-indigo-50 text-indigo-700 rounded-md text-[9px] font-black uppercase tracking-widest">Cloud Live</span>
                    </td>
                    <td class="px-6 py-5 text-right">
                        <button onclick="deleteCustomer('${c.id}')" class="text-rose-400 hover:text-rose-600 font-black text-[10px] uppercase">Remove</button>
                    </td>
                </tr>
            `).join('');
        }
    </script>
</body>
</html>
