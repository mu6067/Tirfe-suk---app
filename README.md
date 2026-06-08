# Tirfe-suk---app```html
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

```
