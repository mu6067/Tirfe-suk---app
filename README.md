```html
<!DOCTYPE html>
<html lang="am">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ትርፌ አፕ (Tirfe App) - ማዕከላዊ የኪራይ ሥርዓት</title>
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
    
    <style>
        :root {
            --bg-color: #0b132b;
            --card-bg: #1c2541;
            --primary: #4a90e2;
            --success: #2ec4b6;
            --danger: #e63946;
            --warning: #ffb703;
            --purple: #6c5ce7;
            --text-main: #ffffff;
            --text-muted: #afb5c0;
            --border-color: rgba(255, 255, 255, 0.08);
            --base-font-size: 14px;
        }

        /* ገጽታዎች (Themes) */
        .theme-deepblue {
            --bg-color: #0b132b;
            --card-bg: #1c2541;
            --primary: #4a90e2;
            --success: #2ec4b6;
        }
        .theme-emerald {
            --bg-color: #061e1a;
            --card-bg: #0b3c34;
            --primary: #2ec4b6;
            --success: #4ade80;
        }
        .theme-charcoal {
            --bg-color: #121212;
            --card-bg: #1e1e1e;
            --primary: #9b59b6;
            --success: #2ecc71;
        }
        .theme-purple {
            --bg-color: #1a0933;
            --card-bg: #2d1254;
            --primary: #a29bfe;
            --success: #00cec9;
        }

        * { 
            box-sizing: border-box; 
            margin: 0; 
            padding: 0; 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            transition: background-color 0.3s ease, font-size 0.2s ease;
        }
        
        body { 
            background-color: var(--bg-color); 
            color: var(--text-main); 
            padding: 15px; 
            font-size: var(--base-font-size); 
            line-height: 1.6;
        }
        
        .container { max-width: 1200px; margin: 0 auto; }
        .hidden { display: none !important; }
        
        /* ረዳት ክፍሎች */
        .text-success { color: var(--success) !important; }
        .text-danger { color: var(--danger) !important; }
        .text-warning { color: var(--warning) !important; }
        .text-purple { color: var(--purple) !important; }
        .flex-between { display: flex; justify-content: space-between; align-items: center; }
        
        /* የመግቢያ ክፍል */
        .login-box {
            max-width: 400px; margin: 80px auto;
            background-color: var(--card-bg); padding: 35px;
            border-radius: 16px; box-shadow: 0 15px 35px rgba(0,0,0,0.6);
            text-align: center; border: 1px solid var(--border-color);
        }
        .login-box h2 { color: var(--success); font-size: 1.8rem; margin-bottom: 8px; font-weight: 800; }
        .login-box p { color: var(--text-muted); font-size: 0.9rem; margin-bottom: 25px; }

        input, select {
            width: 100%; padding: 12px 15px; margin: 8px 0;
            border-radius: 8px; border: 1px solid #3a506b;
            background-color: rgba(0,0,0,0.2); color: white; font-size: 0.95rem;
            outline: none; transition: border-color 0.3s;
        }
        input:focus, select:focus { border-color: var(--primary); box-shadow: 0 0 5px rgba(74, 144, 226, 0.5); }

        /* አዝራሮች (Buttons) */
        .btn-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 10px; margin: 20px 0; }
        button { 
            padding: 12px 16px; border: none; border-radius: 8px; cursor: pointer; 
            font-weight: bold; font-size: 0.9rem; text-align: center; 
            display: inline-flex; align-items: center; justify-content: center; gap: 6px;
            transition: all 0.2s ease;
        }
        button:active { transform: scale(0.96); }
        .btn-block { width: 100%; display: flex; }
        .btn-primary { background-color: var(--primary); color: white; }
        .btn-primary:hover { opacity: 0.9; }
        .btn-success { background-color: var(--success); color: white; }
        .btn-success:hover { opacity: 0.9; }
        .btn-danger { background-color: var(--danger); color: white; }
        .btn-danger:hover { opacity: 0.9; }
        .btn-warning { background-color: var(--warning); color: #000; }
        .btn-warning:hover { opacity: 0.9; }
        .btn-purple { background-color: var(--purple); color: white; }
        .btn-purple:hover { opacity: 0.9; }
        .btn-sm { padding: 5px 10px; font-size: 0.8rem; border-radius: 4px; }

        /* ካርዶች እና ዳሽቦርድ */
        .welcome-card { 
            background: linear-gradient(135deg, var(--card-bg), #3a506b); 
            padding: 25px; border-radius: 16px; text-align: center; margin-bottom: 20px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.3); border: 1px solid var(--border-color);
        }
        .welcome-card h1 { color: var(--success); font-size: 1.8rem; font-weight: 800; }
        .welcome-card p { font-size: 0.95rem; color: var(--text-muted); margin-top: 6px; }
        
        .meta-bar { 
            background-color: rgba(255, 255, 255, 0.03); padding: 12px; border-radius: 8px; 
            display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 10px; 
            font-size: 0.85rem; margin-bottom: 20px; text-align: center; border: 1px solid var(--border-color);
        }
        .meta-bar div { display: flex; align-items: center; justify-content: center; gap: 5px; }
        
        .dashboard-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; margin-bottom: 20px; }
        @media (min-width: 768px) { .dashboard-grid { grid-template-columns: repeat(4, 1fr); } }
        
        .card { 
            background-color: var(--card-bg); padding: 18px; border-radius: 12px; text-align: center; 
            border-left: 5px solid var(--primary); box-shadow: 0 4px 10px rgba(0,0,0,0.2);
            border-top: 1px solid var(--border-color); border-right: 1px solid var(--border-color); border-bottom: 1px solid var(--border-color);
        }
        .card h3 { font-size: 0.8rem; color: var(--text-muted); margin-bottom: 8px; text-transform: uppercase; letter-spacing: 0.5px; }
        .card p { font-size: 1.4rem; font-weight: 800; }

        .section-box { 
            background-color: var(--card-bg); padding: 20px; border-radius: 14px; margin-bottom: 20px; 
            box-shadow: 0 4px 10px rgba(0,0,0,0.2); border: 1px solid var(--border-color);
        }
        .section-box h2 { 
            font-size: 1.1rem; margin-bottom: 15px; border-bottom: 2px solid var(--border-color); 
            padding-bottom: 8px; color: var(--primary); display: flex; align-items: center; gap: 8px;
        }

        /* የፍለጋ ሳጥን */
        .search-container { position: relative; margin-bottom: 15px; }
        .search-container input { padding-left: 35px; }
        .search-icon { position: absolute; left: 12px; top: 18px; color: var(--text-muted); }

        /* ሰንጠረዦች (Tables) */
        .table-responsive { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; border-radius: 8px; border: 1px solid var(--border-color); }
        table { width: 100%; border-collapse: collapse; min-width: 700px; }
        th, td { padding: 12px 15px; border-bottom: 1px solid var(--border-color); text-align: left; font-size: 0.9rem; white-space: nowrap; }
        th { background-color: rgba(255,255,255,0.03); color: var(--text-muted); font-weight: 600; }
        tr:hover { background-color: rgba(255,255,255,0.01); }
        
        .status-badge { padding: 4px 10px; border-radius: 20px; font-weight: bold; font-size: 0.75rem; display: inline-flex; align-items: center; gap: 4px; }
        .status-badge.status-active { background-color: rgba(46, 196, 182, 0.15); color: var(--success); }
        .status-badge.status-blocked { background-color: rgba(230, 57, 70, 0.15); color: var(--danger); }
        .action-btns { display: flex; gap: 6px; }

        /* ፕሮፋይል እና መቼት ዲዛይን */
        .setting-controls { display: flex; flex-wrap: wrap; gap: 10px; margin-top: 10px; }
        .profile-data { background: rgba(0,0,0,0.2); padding: 12px; border-radius: 8px; margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; border: 1px solid var(--border-color); }
        .profile-data label { font-weight: bold; color: var(--text-muted); }
        .profile-data span { color: var(--warning); font-weight: bold; }

        /* የሞዳል ተደራቢ (Custom Popups Overlay) */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0, 0, 0, 0.85); backdrop-filter: blur(5px);
            display: flex; justify-content: center; align-items: center;
            z-index: 1000; opacity: 1; transition: opacity 0.3s ease;
        }
        .modal-card {
            background-color: var(--card-bg); border-radius: 16px;
            width: 90%; max-width: 480px; padding: 25px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.6);
            border: 1px solid var(--border-color);
            transform: translateY(0); transition: transform 0.3s ease;
        }
        .modal-header { margin-bottom: 15px; border-bottom: 1px solid var(--border-color); padding-bottom: 10px; }
        .modal-header h3 { font-size: 1.2rem; color: var(--success); }
        .modal-body { margin-bottom: 20px; font-size: 0.95rem; }
        .modal-footer { display: flex; justify-content: flex-end; gap: 10px; }
    </style>
</head>
<body class="theme-deepblue">

<div class="container">

    <!-- መግቢያ ገጽ -->
    <div id="loginPage" class="login-box">
        <h2>ትርፌ አፕ (Tirfe)</h2>
        <p>የኪራይና የሱቅ መቆጣጠሪያ ማዕከላዊ ሲስተም</p>
        <input type="text" id="loginUser" placeholder="የተጠቃሚ ስም (Username)">
        <input type="password" id="loginPass" placeholder="የይለፍ ቃል (Password)">
        <button class="btn-success btn-block" onclick="handleLogin()">🔒 ደህንነቱ የተጠበቀ መግቢያ</button>
        <p id="loginError" style="color: var(--danger); margin-top: 15px; font-size: 0.85rem; font-weight: bold;"></p>
    </div>

    <!-- የባለቤቱ ማዕከላዊ መቆጣጠሪያ ገጽ (ADMIN) -->
    <div id="adminPage" class="hidden">
        <div class="welcome-card">
            <h1>የባለቤቱ ማዕከላዊ ቁጥጥር ፓነል</h1>
            <p>እዚህ ገጽ ላይ አዳዲስ ተከራዮችን መመዝገብ፣ ጊዜያዊ ማገድ፣ መረጃ ማየት እና ሙሉ በሙሉ መሰረዝ ይችላሉ።</p>
            <button class="btn-danger" style="margin-top: 15px; padding: 8px 16px;" onclick="logout()">🚪 ከሲስተሙ ውጣ</button>
        </div>

        <div class="section-box">
            <h2>👤 አዲስ ተከራይ መመዝገቢያ</h2>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 10px;">
                <input type="text" id="newShopName" placeholder="የሱቅ ስም (ምሳሌ፡ አልፋ)">
                <input type="text" id="newUsername" placeholder="መግቢያ ስም (Username)">
                <input type="text" id="newPassword" placeholder="የይለፍ ቃል (Password)">
            </div>
            <button class="btn-primary btn-block" style="margin-top: 10px;" onclick="registerTenant()">➕ ተከራይ በቋሚነት መዝግብ</button>
        </div>

        <div class="section-box">
            <h2>📈 የተከራዮች እና የኪራይ ሁኔታ መከታተያ</h2>
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>የሱቅ ስም</th>
                            <th>Username</th>
                            <th>Password</th>
                            <th>የኪራይ ሁኔታ</th>
                            <th>ማዕከላዊ እርምጃዎች</th>
                        </tr>
                    </thead>
                    <tbody id="tenantTableBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- የተከራይ የሱቅ መቆጣጠሪያ ገጽ (APP) -->
    <div id="appPage" class="hidden">
        <div class="welcome-card">
            <h1 id="shopTitle">"ትርፌ" መቆጣጠሪያ መተግበሪያ</h1>
            <p>ቅልጥፍና እና አስተማማኝነት ለሱቅዎ</p>
        </div>

        <div class="meta-bar">
            <div>📅 ቀን፡ <span id="metaDate">--/--/----</span></div>
            <div>🕒 ሰዓት፡ <span id="metaTime">00:00:00 AM/PM</span></div>
            <div>👤 ተከራይ፡ <span id="metaStaff" class="text-success">mulugeta</span></div>
        </div>

        <div class="dashboard-grid">
            <div class="card"><h3>የሱቅ ጠቅላላ ካፒታል</h3><p id="dispCapital">0.00</p></div>
            <div class="card"><h3>የዛሬ የተጣራ ትርፍ</h3><p id="dispDailyProfit" style="color: var(--success);">0.00</p></div>
            <div class="card"><h3>የወር የተጣራ ትርፍ</h3><p id="dispMonthlyProfit" style="color: var(--success);">0.00</p></div>
            <div class="card"><h3>ከሳጥን የወጣ ጠቅላላ</h3><p id="dispDrawer" style="color: var(--purple);">0.00</p></div>
        </div>

        <div class="btn-grid">
            <button class="btn-primary" onclick="focusInventoryInput()">📦 አዲስ ዕቃ ጨምር</button>
            <button class="btn-danger" onclick="openExpenseModal()">📉 የወጪ መዝገብ</button>
            <button class="btn-warning" onclick="openDebtModal()">💳 አዲስ የዕዳ መዝገብ</button>
            <button class="btn-purple" onclick="openDrawerModal()">💸 ከሳጥን ብር ውሰድ</button>
        </div>

        <!-- የእቃዎች ዝርዝር እና የክምችት ቁጥጥር -->
        <div class="section-box">
            <h2>📦 የዕቃዎች ዝርዝር እና የክምችት ቁጥጥር</h2>
            
            <!-- አዲስ ዕቃ ማስገቢያ ፎርም -->
            <div style="background: rgba(255, 255, 255, 0.02); padding: 15px; border-radius: 8px; margin-bottom: 15px; border: 1px solid var(--border-color);">
                <h3 style="font-size: 0.9rem; margin-bottom: 10px; color: var(--success);">አዲስ እቃ ማስገቢያ</h3>
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 8px;">
                    <input type="text" id="itemName" placeholder="የእቃ ስም">
                    <input type="number" id="itemCost" placeholder="መግዣ ዋጋ (Cost)">
                    <input type="number" id="itemPrice" placeholder="መሸጫ ዋጋ (Price)">
                    <input type="number" id="itemQty" placeholder="የመጀመሪያ ብዛት (Qty)">
                </div>
                <button class="btn-success btn-block" style="margin-top: 10px;" onclick="addItem()">📥 ዕቃውን በዝርዝር አስገባ</button>
            </div>

            <!-- ዕቃዎችን መፈለጊያ -->
            <div class="search-container">
                <span class="search-icon">🔍</span>
                <input type="text" id="inventorySearch" placeholder="ዕቃዎችን በስም እዚህ ይፈልጉ..." oninput="renderInventoryTable()">
            </div>
            
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>የዕቃ ስም</th>
                            <th>መግዣ (ETB)</th>
                            <th>መሸጫ (ETB)</th>
                            <th>የመጀመሪያ ክምችት</th>
                            <th>የተሸጠ ብዛት</th>
                            <th>ቀሪ ክምችት</th>
                            <th>ያፈራው ትርፍ (ETB)</th>
                            <th>እርምጃዎች</th>
                        </tr>
                    </thead>
                    <tbody id="inventoryTable"></tbody>
                </table>
            </div>
        </div>

        <!-- የዕዳ መዝገብ -->
        <div class="section-box">
            <h2>💳 የደንበኞች የዕዳ መዝገብ</h2>
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>የደንበኛ ስም</th>
                            <th>የዕቃ/የአገልግሎት ዝርዝር</th>
                            <th>የዕዳ መጠን (ETB)</th>
                            <th>አስተዳደር</th>
                        </tr>
                    </thead>
                    <tbody id="debtTable"></tbody>
                </table>
            </div>
        </div>

        <!-- ከሳጥን የወጣ ገንዘብ መቆጣጠሪያ -->
        <div class="section-box">
            <h2>💸 ከሳጥን የተነሳ የገንዘብ መቆጣጠሪያ</h2>
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>ምክንያት/ማብራሪያ</th>
                            <th>የተወሰደው ብር መጠን</th>
                            <th>ሁኔታ</th>
                        </tr>
                    </thead>
                    <tbody id="drawerTable"></tbody>
                </table>
            </div>
        </div>

        <!-- መቼቶች እና የተጠቃሚ ፕሮፋይል -->
        <div class="section-box">
            <h2>⚙️ መቼቶች እና የተጠቃሚ ፕሮፋይል (Settings & Profile)</h2>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 15px; margin-top: 10px;">
                <!-- የፕሮፋይል ማሳያ እና የይለፍ ቃል መቀየሪያ -->
                <div>
                    <h3 style="font-size: 0.95rem; color: var(--success); margin-bottom: 10px;">👤 የእኔ ፕሮፋይል</h3>
                    <div class="profile-data"><label>የሱቅ ስም፡</label> <span id="profShopName">-</span></div>
                    <div class="profile-data"><label>Username፡</label> <span id="profUsername">-</span></div>
                    <div class="profile-data"><label>Password፡</label> <span id="profPassword">-</span></div>
                    <button class="btn-primary btn-sm btn-block" onclick="openEditProfileModal()">✏️ መግቢያ መረጃ ቀይር</button>
                </div>
                <!-- የፅሁፍ መጠን እና ገጽታ (Theme) ማስተካከያ -->
                <div>
                    <h3 style="font-size: 0.95rem; color: var(--primary); margin-bottom: 10px;">🔍 የማሳያ መጠን (Zoom Settings)</h3>
                    <p style="font-size: 0.8rem; color: var(--text-muted); margin-bottom: 10px;">የአፑን የፅሁፍ መጠኖች ለእይታዎ ምቹ ለማድረግ ይጠቀሙ።</p>
                    <div class="setting-controls">
                        <button class="btn-primary btn-sm" onclick="adjustFontSize(1)">➕ አሳድግ (Zoom In)</button>
                        <button class="btn-warning btn-sm" onclick="adjustFontSize(-1)">➖ ቀንስ (Zoom Out)</button>
                        <button class="btn-danger btn-sm" onclick="resetFontSize()">🔄 ወደ መደበኛ መልስ</button>
                    </div>

                    <h3 style="font-size: 0.95rem; color: var(--primary); margin-top: 20px; margin-bottom: 10px;">🎨 የአፕሊኬሽኑ ገጽታ (Themes)</h3>
                    <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 6px;">
                        <button class="btn-primary btn-sm" onclick="changeTheme('deepblue')">Deep Blue</button>
                        <button class="btn-success btn-sm" onclick="changeTheme('emerald')">Emerald Green</button>
                        <button class="btn-purple btn-sm" onclick="changeTheme('purple')">Royal Purple</button>
                        <button class="btn-warning btn-sm" style="color:#000;" onclick="changeTheme('charcoal')">Charcoal Dark</button>
                    </div>
                </div>
            </div>
        </div>
        
        <button class="btn-danger btn-block" style="margin-top: 25px; padding: 15px; font-size: 1rem;" onclick="logout()">🚪 ከአፑ በሰላም ውጣ (Logout)</button>
    </div>

</div>

<!-- ማዕከላዊ የሞዳል ሲስተም (CUSTOM MODALS OVERLAYS) -->
<div id="modalOverlay" class="modal-overlay hidden">
    <!-- የማሳወቂያ ሞዳል (Alert Modal) -->
    <div id="alertModal" class="modal-card hidden">
        <div class="modal-header">
            <h3 id="alertTitle" class="text-warning">መልዕክት</h3>
        </div>
        <div class="modal-body">
            <p id="alertMessage">እባክዎ መረጃዎችን በትክክል ያስገቡ!</p>
        </div>
        <div class="modal-footer">
            <button class="btn-primary" onclick="closeActiveModal()">እሺ ተረዳሁ</button>
        </div>
    </div>

    <!-- የማረጋገጫ ሞዳል (Confirm Modal) -->
    <div id="confirmModal" class="modal-card hidden">
        <div class="modal-header">
            <h3 id="confirmTitle" class="text-danger">እርግጠኛ ኖት?</h3>
        </div>
        <div class="modal-body">
            <p id="confirmMessage">ይህ እርምጃ ሊመለስ አይችልም። መቀጠል ይፈልጋሉ?</p>
        </div>
        <div class="modal-footer">
            <button class="btn-danger" id="confirmYesBtn">አዎ ይቀጥል</button>
            <button class="btn-primary" onclick="closeActiveModal()">አይ ይቅር</button>
        </div>
    </div>

    <!-- የወጪ ማስገቢያ ሞዳል (Expense Modal) -->
    <div id="expenseModal" class="modal-card hidden">
        <div class="modal-header">
            <h3>📉 አዲስ የወጪ መመዝገቢያ</h3>
        </div>
        <div class="modal-body">
            <label style="font-size:0.85rem; color: var(--text-muted)">የወጪው ምክንያት ወይም ማብራሪያ፡</label>
            <input type="text" id="expenseReason" placeholder="ምሳሌ፡ ለኤሌክትሪክ፣ ለኪራይ፣ ለሻይ...">
            <label style="font-size:0.85rem; color: var(--text-muted); margin-top:10px; display:block;">የብር መጠን (ETB)፡</label>
            <input type="number" id="expenseAmount" placeholder="ምሳሌ፡ 250.00">
        </div>
        <div class="modal-footer">
            <button class="btn-success" onclick="saveExpense()">ወጪውን መዝግብ</button>
            <button class="btn-danger" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>

    <!-- የዕዳ ማስገቢያ ሞዳል (Debt Modal) -->
    <div id="debtModal" class="modal-card hidden">
        <div class="modal-header">
            <h3>💳 አዲስ የተበዳሪ መመዝገቢያ</h3>
        </div>
        <div class="modal-body">
            <label style="font-size:0.85rem; color: var(--text-muted)">የደንበኛ ስም፡</label>
            <input type="text" id="debtName" placeholder="ምሳሌ፡ አቶ አበበ">
            <label style="font-size:0.85rem; color: var(--text-muted); margin-top:10px; display:block;">የተወሰደበት እቃ ወይም ዝርዝር፡</label>
            <input type="text" id="debtDetail" placeholder="ምሳሌ፡ ልብስ፣ ጫማ...">
            <label style="font-size:0.85rem; color: var(--text-muted); margin-top:10px; display:block;">የዕዳው ጠቅላላ መጠን (ETB)፡</label>
            <input type="number" id="debtAmount" placeholder="ምሳሌ፡ 1200.00">
        </div>
        <div class="modal-footer">
            <button class="btn-warning" onclick="saveDebt()">ዕዳውን መዝግብ</button>
            <button class="btn-danger" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>

    <!-- የዕዳ አከፋፈል ሞዳል (Debt Settlement Modal) -->
    <div id="settleDebtModal" class="modal-card hidden">
        <div class="modal-header">
            <h3>💳 የዕዳ አከፋፈል ማስተካከያ</h3>
        </div>
        <div class="modal-body">
            <p>የደንበኛ ስም፡ <b id="settleCustName" class="text-warning">-</b></p>
            <p style="margin-bottom:15px;">ያለበት ዕዳ፡ <b id="settleCustAmt" class="text-danger">0.00 ETB</b></p>
            <label style="font-size:0.85rem; color: var(--text-muted)">የሚከፈልበት የብር መጠን (ETB)፡</label>
            <input type="number" id="settlePayAmount" placeholder="ምሳሌ፡ 500.00">
        </div>
        <div class="modal-footer">
            <button class="btn-success" id="settleSubmitBtn">ክፍያውን ፈጽም</button>
            <button class="btn-danger" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>

    <!-- ከሳጥን ገንዘብ መውሰጃ ሞዳል (Drawer Modal) -->
    <div id="drawerModal" class="modal-card hidden">
        <div class="modal-header">
            <h3>💸 ከሳጥን ገንዘብ መውሰጃ</h3>
        </div>
        <div class="modal-body">
            <label style="font-size:0.85rem; color: var(--text-muted)">ገንዘቡ የተወሰደበት ምክንያት፡</label>
            <input type="text" id="drawerReason" placeholder="ምሳሌ፡ ለዕቃ ግዢ፣ ለትርፍ ክፍፍል...">
            <label style="font-size:0.85rem; color: var(--text-muted); margin-top:10px; display:block;">የብር መጠን (ETB)፡</label>
            <input type="number" id="drawerAmount" placeholder="ምሳሌ፡ 5000">
        </div>
        <div class="modal-footer">
            <button class="btn-purple" onclick="saveDrawer()">ገንዘብ ወጪ መዝግብ</button>
            <button class="btn-danger" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>

    <!-- ፕሮፋይል ማስተካከያ ሞዳል (Edit Profile Modal) -->
    <div id="editProfileModal" class="modal-card hidden">
        <div class="modal-header">
            <h3>✏️ የመግቢያ መረጃ መቀየሪያ</h3>
        </div>
        <div class="modal-body">
            <label style="font-size:0.85rem; color: var(--text-muted)">የሱቅ ስም፡</label>
            <input type="text" id="editShopName">
            <label style="font-size:0.85rem; color: var(--text-muted); margin-top:10px; display:block;">የይለፍ ቃል (Password)፡</label>
            <input type="text" id="editPassword">
        </div>
        <div class="modal-footer">
            <button class="btn-success" onclick="saveProfileChanges()">ማስተካከያውን መዝግብ</button>
            <button class="btn-danger" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>

    <!-- የዕቃ መረጃ ማስተካከያ ሞዳል (Edit Inventory Item Modal) -->
    <div id="editInventoryModal" class="modal-card hidden">
        <div class="modal-header">
            <h3>✏️ የዕቃ መረጃ ማስተካከያ</h3>
        </div>
        <div class="modal-body">
            <input type="hidden" id="editItemIndex">
            <label style="font-size:0.85rem; color: var(--text-muted)">የዕቃ ስም፡</label>
            <input type="text" id="editItemName">
            <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 8px;">
                <div>
                    <label style="font-size:0.85rem; color: var(--text-muted)">መግዣ ዋጋ (Cost)፡</label>
                    <input type="number" id="editItemCost">
                </div>
                <div>
                    <label style="font-size:0.85rem; color: var(--text-muted)">መሸጫ ዋጋ (Price)፡</label>
                    <input type="number" id="editItemPrice">
                </div>
            </div>
            <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 8px; margin-top: 10px;">
                <div>
                    <label style="font-size:0.85rem; color: var(--text-muted)">ጠቅላላ ብዛት፡</label>
                    <input type="number" id="editItemQty">
                </div>
                <div>
                    <label style="font-size:0.85rem; color: var(--text-muted)">የተሸጠው መጠን፡</label>
                    <input type="number" id="editItemSold" readonly style="background: rgba(255,255,255,0.05);">
                </div>
            </div>
        </div>
        <div class="modal-footer">
            <button class="btn-success" onclick="saveInventoryChanges()">አሻሽል</button>
            <button class="btn-danger" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>
</div>

<script>
    // 1. Firebase ማስጀመሪያ
    const firebaseConfig = {
        databaseURL: "https://tirfe-app-v2-300c2-default-rtdb.firebaseio.com/" 
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    let localDB = { tenants: {} };
    let currentTenant = null;
    let currentFontSize = 14;

    // የቀጥታ ሰዓት እና ቀን መቆጣጠሪያ
    setInterval(() => {
        const now = new Date();
        const optionsDate = { year: 'numeric', month: 'long', day: 'numeric' };
        const formattedDate = now.toLocaleDateString('am-ET', optionsDate);
        
        const optionsTime = { hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: true };
        const formattedTime = now.toLocaleTimeString('am-ET', optionsTime);
        
        const mDate = document.getElementById('metaDate');
        const mTime = document.getElementById('metaTime');
        if (mDate) mDate.innerText = formattedDate;
        if (mTime) mTime.innerText = formattedTime;
    }, 1000);

    // 2. የሪልታይም ዳታቤዝ ክትትል (Realtime Database listener)
    db.ref('tirfe_system').on('value', (snapshot) => {
        if(snapshot.exists()) {
            localDB = snapshot.val();
            if(!localDB.tenants) localDB.tenants = {};
            
            if(currentTenant) {
                let checkTenant = localDB.tenants[currentTenant.username];
                if(!checkTenant) {
                    showCustomAlert("❌ የይዞታ መጥፋት", "ይህ አካውንት በባለቤቱ ተሰርዟል!", () => {
                        logout();
                    });
                    return;
                }
                if(checkTenant.status === "blocked") {
                    showCustomAlert("🔒 የኪራይ ማብቂያ", "የኪራይ ጊዜዎ አብቅቷል! እባክዎ ባለቤቱን ያነጋግሩ።", () => {
                        logout();
                    });
                    return;
                }
                currentTenant = checkTenant;
                loadTenantApp();
            }
            if(!document.getElementById('adminPage').classList.contains('hidden')) {
                renderAdminPanel();
            }
        } else {
            localDB = { tenants: {} };
        }
    });

    function pushToFirebase() {
        db.ref('tirfe_system').set(localDB);
    }

    // የሞዳል መቆጣጠሪያዎች (Custom Modal handlers)
    function openModalContainer() {
        document.getElementById('modalOverlay').classList.remove('hidden');
    }

    function closeActiveModal() {
        document.getElementById('modalOverlay').classList.add('hidden');
        const modals = document.querySelectorAll('.modal-card');
        modals.forEach(m => m.classList.add('hidden'));
    }

    function showCustomAlert(title, message, callback) {
        document.getElementById('alertTitle').innerText = title;
        document.getElementById('alertMessage').innerText = message;
        
        openModalContainer();
        document.getElementById('alertModal').classList.remove('hidden');
        
        const okBtn = document.querySelector('#alertModal .btn-primary');
        okBtn.onclick = function() {
            closeActiveModal();
            if(callback) callback();
        };
    }

    function showCustomConfirm(title, message, onConfirm) {
        document.getElementById('confirmTitle').innerText = title;
        document.getElementById('confirmMessage').innerText = message;
        
        openModalContainer();
        document.getElementById('confirmModal').classList.remove('hidden');
        
        const yesBtn = document.getElementById('confirmYesBtn');
        yesBtn.onclick = function() {
            closeActiveModal();
            if(onConfirm) onConfirm();
        };
    }

    // የመግቢያ ሲስተም (Login)
    function handleLogin() {
        let user = document.getElementById('loginUser').value.trim();
        let pass = document.getElementById('loginPass').value.trim();
        let error = document.getElementById('loginError');

        if(user === "admin" && pass === "admin123") {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('adminPage').classList.remove('hidden');
            renderAdminPanel();
            return;
        }

        if (!localDB.tenants) {
            error.innerText = "❌ ምንም የተመዘገበ ተከራይ የለም!";
            return;
        }

        let tenant = localDB.tenants[user];
        if(tenant && String(tenant.password).trim() === pass) {
            if(tenant.status === "blocked") {
                error.innerText = "🔒 ኪራይዎ በባለቤቱ ታግዷል!";
                return;
            }
            currentTenant = tenant;
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('appPage').classList.remove('hidden');
            document.getElementById('shopTitle').innerText = tenant.shopName + " - መቆጣጠሪያ";
            
            document.getElementById('metaStaff').innerText = tenant.username;
            updateProfileSectionUI();
            loadTenantApp();
        } else {
            error.innerText = "❌ የተጠቃሚ ስም ወይም የይለፍ ቃል ስህተት ነው!";
        }
    }

    function updateProfileSectionUI() {
        if(currentTenant) {
            document.getElementById('profShopName').innerText = currentTenant.shopName;
            document.getElementById('profUsername').innerText = currentTenant.username;
            document.getElementById('profPassword').innerText = currentTenant.password;
        }
    }

    // የፅሁፍ መጠን ማስተካከያ (Zoom)
    function adjustFontSize(value) {
        currentFontSize += value;
        if(currentFontSize < 11) currentFontSize = 11;
        if(currentFontSize > 22) currentFontSize = 22;
        document.documentElement.style.setProperty('--base-font-size', currentFontSize + 'px');
    }

    function resetFontSize() {
        currentFontSize = 14;
        document.documentElement.style.setProperty('--base-font-size', '14px');
    }

    // የቀለም ገጽታ መቀየሪያ (Theme selector)
    function changeTheme(themeName) {
        document.body.className = '';
        document.body.classList.add('theme-' + themeName);
    }

    // የባለቤት ስራዎች (Admin/Owner control panel)
    function registerTenant() {
        let shop = document.getElementById('newShopName').value.trim();
        let user = document.getElementById('newUsername').value.trim().toLowerCase(); 
        let pass = document.getElementById('newPassword').value.trim();

        if(!shop || !user || !pass) { 
            showCustomAlert("⚠️ ስህተት", "እባክዎ ሁሉንም መረጃዎች ይሙሉ!"); 
            return; 
        }
        if(localDB.tenants && localDB.tenants[user]) { 
            showCustomAlert("⚠️ ስህተት", "ይህ የተጠቃሚ ስም ቀድሞ ተወስዷል!"); 
            return; 
        }

        if(!localDB.tenants) localDB.tenants = {};

        localDB.tenants[user] = {
            shopName: shop, username: user, password: pass, status: "active",
            data: { capital: 0, dailyProfit: 0, monthlyProfit: 0, drawerAmt: 0, inventory: [], expenses: [], debts: [], drawerLog: [] }
        };
        
        pushToFirebase();
        showCustomAlert("✅ ስኬት", "ተከራይ በስኬት ተመዝግቧል!");
        
        document.getElementById('newShopName').value = '';
        document.getElementById('newUsername').value = '';
        document.getElementById('newPassword').value = '';
        renderAdminPanel();
    }

    function toggleStatus(user) {
        localDB.tenants[user].status = localDB.tenants[user].status === "active" ? "blocked" : "active";
        pushToFirebase();
    }

    function deleteTenant(user) {
        showCustomConfirm(
            "⚠️ እርግጠኛ ነዎት?", 
            `"${localDB.tenants[user].shopName}" የተባለውን ተከራይና ጠቅላላ ዳታውን ሙሉ በሙሉ ማጥፋት ይፈልጋሉ?`,
            () => {
                delete localDB.tenants[user];
                pushToFirebase();
                showCustomAlert("🧹 ስኬት", "ተከራዩ ሙሉ በሙሉ ተሰርዟል!");
                renderAdminPanel();
            }
        );
    }

    function renderAdminPanel() {
        let tbody = document.getElementById('tenantTableBody');
        tbody.innerHTML = '';
        if(!localDB.tenants) return;

        Object.keys(localDB.tenants).forEach(key => {
            let t = localDB.tenants[key];
            let badge = t.status === 'active' ? '<span class="status-badge status-active">ስራ ላይ</span>' : '<span class="status-badge status-blocked">የታገደ</span>';
            let btnText = t.status === 'active' ? 'Block' : 'Unblock';
            let btnClass = t.status === 'active' ? 'btn-warning' : 'btn-success';

            tbody.innerHTML += `
                <tr>
                    <td><b>${t.shopName}</b></td>
                    <td>${t.username}</td>
                    <td>${t.password}</td>
                    <td>${badge}</td>
                    <td>
                        <div class="action-btns">
                            <button class="${btnClass} btn-sm" onclick="toggleStatus('${t.username}')">${btnText}</button>
                            <button class="btn-danger btn-sm" onclick="deleteTenant('${t.username}')">Delete</button>
                        </div>
                    </td>
                </tr>
            `;
        });
    }

    // የተከራይ መተግበሪያ ዋና ተግባራት
    function loadTenantApp() {
        let d = currentTenant.data;
        if(!d) {
            d = { capital: 0, dailyProfit: 0, monthlyProfit: 0, drawerAmt: 0, inventory: [], expenses: [], debts: [], drawerLog: [] };
            currentTenant.data = d;
        }
        if(!d.inventory) d.inventory = [];
        if(!d.expenses) d.expenses = [];
        if(!d.debts) d.debts = [];
        if(!d.drawerLog) d.drawerLog = [];

        document.getElementById('dispCapital').innerText = Number(d.capital).toFixed(2) + " ETB";
        document.getElementById('dispDailyProfit').innerText = Number(d.dailyProfit).toFixed(2) + " ETB";
        document.getElementById('dispMonthlyProfit').innerText = Number(d.monthlyProfit).toFixed(2) + " ETB";
        document.getElementById('dispDrawer').innerText = Number(d.drawerAmt).toFixed(2) + " ETB";

        renderInventoryTable();
        renderDrawerTable();
        renderDebtTable();
    }

    function focusInventoryInput() {
        document.getElementById('itemName').focus();
    }

    function addItem() {
        let name = document.getElementById('itemName').value.trim();
        let cost = parseFloat(document.getElementById('itemCost').value) || 0;
        let price = parseFloat(document.getElementById('itemPrice').value) || 0;
        let qty = parseInt(document.getElementById('itemQty').value) || 0;

        if(!name || cost <= 0 || price <= 0 || qty <= 0) { 
            showCustomAlert("⚠️ ስህተት", "እባክዎ ሁሉንም የዕቃ መረጃዎች በትክክል ያስገቡ!"); 
            return; 
        }

        if(!currentTenant.data.inventory) currentTenant.data.inventory = [];
        currentTenant.data.inventory.push({ name, cost, price, qty, sold: 0, profit: 0 });
        
        recalculateCapital();
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();

        document.getElementById('itemName').value = '';
        document.getElementById('itemCost').value = '';
        document.getElementById('itemPrice').value = '';
        document.getElementById('itemQty').value = '';
        showCustomAlert("📦 ስኬት", "ዕቃው በተሳካ ሁኔታ ተመዝግቧል!");
    }

    function renderInventoryTable() {
        let tbody = document.getElementById('inventoryTable');
        tbody.innerHTML = '';
        let inv = currentTenant.data.inventory || [];
        let searchTerm = document.getElementById('inventorySearch').value.toLowerCase().trim();

        inv.forEach((i, idx) => {
            if(searchTerm && !i.name.toLowerCase().includes(searchTerm)) {
                return; // ፍለጋውን ካላለፈ መዝለል
            }
            let remaining = i.qty - i.sold;
            tbody.innerHTML += `
                <tr>
                    <td><b>${i.name}</b></td>
                    <td>${i.cost.toFixed(2)}</td>
                    <td>${i.price.toFixed(2)}</td>
                    <td>${i.qty}</td>
                    <td>
                        <button class="btn-success btn-sm" style="margin-right:8px;" onclick="sellItem(${idx})">➕ 1 ሽጥ</button> 
                        <b>${i.sold}</b>
                    </td>
                    <td>${remaining}</td>
                    <td style="color:var(--success); font-weight:bold;">${i.profit.toFixed(2)}</td>
                    <td>
                        <div class="action-btns">
                            <button class="btn-primary btn-sm" onclick="openEditInventoryModal(${idx})">✏️ አሻሽል</button>
                            <button class="btn-danger btn-sm" onclick="deleteInventoryItem(${idx})">🗑️ ሰርዝ</button>
                        </div>
                    </td>
                </tr>
            `;
        });
    }

    function sellItem(idx) {
        let item = currentTenant.data.inventory[idx];
        if((item.qty - item.sold) <= 0) { 
            showCustomAlert("⚠️ ክምችት አልቋል", "ይህ ዕቃ በሱቁ ውስጥ አልቋል!"); 
            return; 
        }
        
        item.sold += 1;
        let netProfit = item.price - item.cost;
        item.profit += netProfit;
        
        currentTenant.data.dailyProfit += netProfit;
        currentTenant.data.monthlyProfit += netProfit;
        
        recalculateCapital();
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
    }

    function deleteInventoryItem(idx) {
        showCustomConfirm(
            "⚠️ ዕቃ ለመሰረዝ", 
            `የመረጡትን "${currentTenant.data.inventory[idx].name}" ሙሉ በሙሉ መሰረዝ ይፈልጋሉ?`, 
            () => {
                currentTenant.data.inventory.splice(idx, 1);
                recalculateCapital();
                localDB.tenants[currentTenant.username] = currentTenant;
                pushToFirebase();
                showCustomAlert("🧹 ስኬት", "ዕቃው ተሰርዟል!");
            }
        );
    }

    function openEditInventoryModal(idx) {
        let item = currentTenant.data.inventory[idx];
        document.getElementById('editItemIndex').value = idx;
        document.getElementById('editItemName').value = item.name;
        document.getElementById('editItemCost').value = item.cost;
        document.getElementById('editItemPrice').value = item.price;
        document.getElementById('editItemQty').value = item.qty;
        document.getElementById('editItemSold').value = item.sold;

        openModalContainer();
        document.getElementById('editInventoryModal').classList.remove('hidden');
    }

    function saveInventoryChanges() {
        let idx = parseInt(document.getElementById('editItemIndex').value);
        let name = document.getElementById('editItemName').value.trim();
        let cost = parseFloat(document.getElementById('editItemCost').value) || 0;
        let price = parseFloat(document.getElementById('editItemPrice').value) || 0;
        let qty = parseInt(document.getElementById('editItemQty').value) || 0;

        if(!name || cost <= 0 || price <= 0 || qty < 0) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ የተሻሻሉትን መረጃዎች በትክክል ያስገቡ!");
            return;
        }

        let item = currentTenant.data.inventory[idx];
        item.name = name;
        item.cost = cost;
        item.price = price;
        item.qty = qty;

        // የዕቃ ትርፍ ድምር ማስተካከያ
        item.profit = (price - cost) * item.sold;

        recalculateDailyAndMonthlyProfit();
        recalculateCapital();
        
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
        closeActiveModal();
        showCustomAlert("✅ ስኬት", "የዕቃ መረጃው ተስተካክሏል!");
    }

    function recalculateDailyAndMonthlyProfit() {
        let inv = currentTenant.data.inventory || [];
        let exp = currentTenant.data.expenses || [];
        let totalProfit = 0;
        inv.forEach(i => { totalProfit += i.profit; });
        
        let totalExpenses = 0;
        exp.forEach(e => { totalExpenses += e.amt; });

        currentTenant.data.dailyProfit = totalProfit - totalExpenses; 
        currentTenant.data.monthlyProfit = totalProfit - totalExpenses;
    }

    function recalculateCapital() {
        let inv = currentTenant.data.inventory || [];
        let totalAsset = 0;
        inv.forEach(i => { totalAsset += (i.qty - i.sold) * i.cost; });
        currentTenant.data.capital = totalAsset + currentTenant.data.monthlyProfit;
    }

    // የወጪዎች አስተዳደር
    function openExpenseModal() {
        document.getElementById('expenseReason').value = '';
        document.getElementById('expenseAmount').value = '';
        openModalContainer();
        document.getElementById('expenseModal').classList.remove('hidden');
    }

    function saveExpense() {
        let reason = document.getElementById('expenseReason').value.trim();
        let amt = parseFloat(document.getElementById('expenseAmount').value) || 0;

        if(!reason || amt <= 0) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ ትክክለኛ የወጪ ምክንያት እና የብር መጠን ያስገቡ!");
            return;
        }

        currentTenant.data.dailyProfit -= amt; 
        currentTenant.data.monthlyProfit -= amt; 
        currentTenant.data.capital -= amt;
        
        if(!currentTenant.data.expenses) currentTenant.data.expenses = [];
        currentTenant.data.expenses.push({ reason, amt }); 
        
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
        closeActiveModal();
        showCustomAlert("📉 ስኬት", "ወጪው ተመዝግቧል። ትርፍ እና ካፒታል ቀንሷል!");
    }

    // የዕዳዎች አስተዳደር
    function openDebtModal() {
        document.getElementById('debtName').value = '';
        document.getElementById('debtDetail').value = '';
        document.getElementById('debtAmount').value = '';
        openModalContainer();
        document.getElementById('debtModal').classList.remove('hidden');
    }

    function saveDebt() {
        let name = document.getElementById('debtName').value.trim();
        let detail = document.getElementById('debtDetail').value.trim();
        let amt = parseFloat(document.getElementById('debtAmount').value) || 0;

        if(!name || amt <= 0) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ የተበዳሪ ስም እና ትክክለኛ የብር መጠን ያስገቡ!");
            return;
        }

        if(!currentTenant.data.debts) currentTenant.data.debts = [];
        currentTenant.data.debts.push({ name, detail, amt }); 
        
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
        closeActiveModal();
        showCustomAlert("💳 ስኬት", "የደንበኛ ዕዳ በተሳካ ሁኔታ ተመዝግቧል!");
    }

    function renderDebtTable() {
        let tbody = document.getElementById('debtTable'); 
        tbody.innerHTML = '';
        let debts = currentTenant.data.debts || [];
        
        debts.forEach((d, idx) => { 
            tbody.innerHTML += `
                <tr>
                    <td><b>${d.name}</b></td>
                    <td>${d.detail || '-'}</td>
                    <td class="text-danger font-weight-bold">${d.amt.toFixed(2)}</td>
                    <td>
                        <div class="action-btns">
                            <button class="btn-success btn-sm" onclick="openSettleDebtModal(${idx})">💵 ክፈል/ቀንስ</button>
                            <button class="btn-danger btn-sm" onclick="deleteDebtItem(${idx})">🗑️ ሰርዝ</button>
                        </div>
                    </td>
                </tr>
            `; 
        });
    }

    function openSettleDebtModal(idx) {
        let debt = currentTenant.data.debts[idx];
        document.getElementById('settleCustName').innerText = debt.name;
        document.getElementById('settleCustAmt').innerText = debt.amt.toFixed(2) + " ETB";
        document.getElementById('settlePayAmount').value = '';

        openModalContainer();
        document.getElementById('settleDebtModal').classList.remove('hidden');

        document.getElementById('settleSubmitBtn').onclick = function() {
            let payAmt = parseFloat(document.getElementById('settlePayAmount').value) || 0;
            if(payAmt <= 0 || payAmt > debt.amt) {
                showCustomAlert("⚠️ ስህተት", "እባክዎ በትክክል ካለባቸው ዕዳ የማይበልጥ የክፍያ መጠን ያስገቡ!");
                return;
            }

            debt.amt -= payAmt;
            
            // የከፈለውን ገንዘብ ሱቁ የተጣራ ትርፍ ላይ መደመር
            currentTenant.data.dailyProfit += payAmt;
            currentTenant.data.monthlyProfit += payAmt;
            recalculateCapital();

            if(debt.amt <= 0) {
                currentTenant.data.debts.splice(idx, 1);
                showCustomAlert("🎉 ሙሉ በሙሉ ተከፈለ", "ደንበኛው ዕዳውን ሙሉ በሙሉ ጨርሷል!");
            } else {
                showCustomAlert("✅ ዕዳ ተቀንሷል", `ደንበኛው ${payAmt.toFixed(2)} ETB ከፍሏል። ቀሪ ዕዳ፡ ${debt.amt.toFixed(2)} ETB`);
            }

            localDB.tenants[currentTenant.username] = currentTenant;
            pushToFirebase();
            closeActiveModal();
        };
    }

    function deleteDebtItem(idx) {
        showCustomConfirm(
            "⚠️ ዕዳ ለመሰረዝ", 
            "የመረጡትን የተበዳሪ መዝገብ ያለክፍያ ሙሉ በሙሉ መሰረዝ ይፈልጋሉ?", 
            () => {
                currentTenant.data.debts.splice(idx, 1);
                localDB.tenants[currentTenant.username] = currentTenant;
                pushToFirebase();
                showCustomAlert("🧹 ስኬት", "የተበዳሪው መዝገብ ተሰርዟል!");
            }
        );
    }

    // ከሳጥን ገንዘብ ማውጫ
    function openDrawerModal() {
        document.getElementById('drawerReason').value = '';
        document.getElementById('drawerAmount').value = '';
        openModalContainer();
        document.getElementById('drawerModal').classList.remove('hidden');
    }

    function saveDrawer() {
        let reason = document.getElementById('drawerReason').value.trim();
        let amt = parseFloat(document.getElementById('drawerAmount').value) || 0;

        if(!reason || amt <= 0) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ የተወሰደበትን ምክንያት እና የብር መጠን ያስገቡ!");
            return;
        }

        currentTenant.data.drawerAmt += amt;
        if(!currentTenant.data.drawerLog) currentTenant.data.drawerLog = [];
        currentTenant.data.drawerLog.push({ reason, amt, status: 'ወጣ' }); 
        
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
        closeActiveModal();
        showCustomAlert("💸 ስኬት", "ከሳጥን የወጣው ገንዘብ በተሳካ ሁኔታ ተመዝግቧል!");
    }

    function renderDrawerTable() {
        let tbody = document.getElementById('drawerTable'); 
        tbody.innerHTML = '';
        let logs = currentTenant.data.drawerLog || [];
        logs.forEach(l => { 
            tbody.innerHTML += `
                <tr>
                    <td>${l.reason}</td>
                    <td class="text-purple font-weight-bold">${l.amt.toFixed(2)} ETB</td>
                    <td><span class="status-badge status-blocked">${l.status}</span></td>
                </tr>
            `; 
        });
    }

    // የፕሮፋይል መረጃ ማሻሻያ
    function openEditProfileModal() {
        document.getElementById('editShopName').value = currentTenant.shopName;
        document.getElementById('editPassword').value = currentTenant.password;
        
        openModalContainer();
        document.getElementById('editProfileModal').classList.remove('hidden');
    }

    function saveProfileChanges() {
        let shop = document.getElementById('editShopName').value.trim();
        let pass = document.getElementById('editPassword').value.trim();

        if(!shop || !pass) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ ሁሉንም መረጃዎች ይሙሉ!");
            return;
        }

        currentTenant.shopName = shop;
        currentTenant.password = pass;

        document.getElementById('shopTitle').innerText = shop + " - መቆጣጠሪያ";
        updateProfileSectionUI();

        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
        closeActiveModal();
        showCustomAlert("✅ ስኬት", "የእርስዎ መገለጫ መረጃ ተስተካክሏል!");
    }

    // ከሲስተም መውጫ (Logout)
    function logout() {
        currentTenant = null;
        resetFontSize();
        document.getElementById('loginUser').value = '';
        document.getElementById('loginPass').value = '';
        document.getElementById('adminPage').classList.add('hidden');
        document.getElementById('appPage').classList.add('hidden');
        document.getElementById('loginPage').classList.remove('hidden');
    }
</script>
</body>
</html>

```
