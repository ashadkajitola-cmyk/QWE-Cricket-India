<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cricket Central - INDIA SCHEDULE</title>
    <style>
        :root {
            --primary-green: #00875a;
            --dark-green: #006644;
            --bg-color: #f4f7f6;
            --card-bg: #ffffff;
            --text-color: #333333;
        }
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--bg-color);
            margin: 0;
            padding: 0;
            color: var(--text-color);
        }
        .app-header {
            background-color: var(--primary-green);
            color: white;
            padding: 15px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .app-header h1 {
            margin: 0;
            font-size: 20px;
        }
        .admin-btn {
            background: white;
            color: var(--primary-green);
            border: none;
            padding: 6px 12px;
            border-radius: 4px;
            font-weight: bold;
            cursor: pointer;
        }
        .nav-tabs {
            display: flex;
            background-color: var(--dark-green);
            padding: 0 15px;
        }
        .nav-tab {
            color: white;
            padding: 12px 20px;
            text-decoration: none;
            font-weight: bold;
            font-size: 14px;
            border-bottom: 3px solid transparent;
            cursor: pointer;
        }
        .nav-tab.active {
            border-bottom-color: white;
        }
        .container {
            max-width: 600px;
            margin: 15px auto;
            padding: 10px;
        }
        .card {
            background: var(--card-bg);
            border-radius: 8px;
            padding: 15px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            margin-bottom: 15px;
            border: 1px solid #e0e0e0;
        }
        .sub-container {
            border: 2px dashed var(--primary-green);
            background: #f9fbf9;
        }
        .plan-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin: 15px 0;
        }
        .plan-card {
            border: 1px solid #ccc;
            border-radius: 6px;
            padding: 12px;
            text-align: center;
            cursor: pointer;
            background: white;
            transition: 0.2s;
        }
        .plan-card:hover, .plan-card.selected {
            border-color: var(--primary-green);
            background-color: #e6f4ea;
        }
        .plan-price {
            color: var(--primary-green);
            font-weight: bold;
            font-size: 16px;
            margin-top: 5px;
        }
        .btn-pay {
            background-color: var(--primary-green);
            color: white;
            border: none;
            width: 100%;
            padding: 12px;
            border-radius: 5px;
            font-size: 16px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 5px;
        }
        .btn-pay:hover {
            background-color: var(--dark-green);
        }
        .step { display: none; }
        .step.active { display: block; }
        input[type="text"], select {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-sizing: border-box;
        }
        .loader {
            border: 4px solid #f3f3f3;
            border-radius: 50%;
            border-top: 4px solid var(--primary-green);
            width: 35px;
            height: 35px;
            animation: spin 1s linear infinite;
            margin: 15px auto;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        .match-box {
            border: 1px solid #e0e0e0;
            border-radius: 6px;
            padding: 12px;
            margin-bottom: 10px;
            background: white;
            position: relative;
        }
        .match-header {
            font-size: 13px;
            color: #666;
            margin-bottom: 8px;
            display: flex;
            justify-content: space-between;
        }
        .teams-row {
            display: flex;
            justify-content: space-between;
            align-items: center;
            font-weight: bold;
            font-size: 16px;
        }
        .match-actions {
            margin-top: 8px;
            text-align: right;
            display: none;
        }
        .match-actions button {
            padding: 4px 8px;
            font-size: 12px;
            margin-left: 5px;
            cursor: pointer;
            border-radius: 4px;
            border: none;
        }
        .btn-edit { background-color: #ffc107; color: #000; }
        .btn-delete { background-color: #dc3545; color: #fff; }
        
        #admin-modal {
            display: none;
            position: fixed;
            top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.5);
            justify-content: center;
            align-items: center;
            z-index: 1000;
        }
        .admin-content {
            background: white;
            padding: 20px;
            border-radius: 8px;
            width: 90%;
            max-width: 450px;
            max-height: 90vh;
            overflow-y: auto;
        }
    </style>
</head>
<body>

<!-- Header Section -->
<div class="app-header">
    <h1>🏏 Cricket Central</h1>
    <button class="admin-btn" onclick="openAdminPanel()">Admin</button>
</div>

<!-- Navigation Tabs -->
<div class="nav-tabs">
    <div class="nav-tab active">MATCHES</div>
    <div class="nav-tab">POINTS TABLE</div>
    <div class="nav-tab">GROUPS</div>
</div>

<div class="container">

    <!-- SUBSCRIPTION CARD -->
    <div class="card sub-container" id="subscription-card">
        <h3 style="margin-top:0; color: var(--primary-green); text-align:center;">⭐ Premium Subscription Plan</h3>
        
        <div id="step-plans" class="step active">
            <p style="font-size:13px; text-align:center; color:#555;">Subscription plan select karo aur apna mobile number dalein:</p>
            <div class="plan-grid">
                <div class="plan-card" onclick="selectPlan('Weekly', 28)">
                    <strong>Weekly</strong>
                    <div class="plan-price">₹28</div>
                    <small>7 Days Access</small>
                </div>
                <div class="plan-card" onclick="selectPlan('Half Monthly', 25)">
                    <strong>Half Monthly</strong>
                    <div class="plan-price">₹25</div>
                    <small>15 Days Access</small>
                </div>
                <div class="plan-card" onclick="selectPlan('3 Match', 17)">
                    <strong>3 Match</strong>
                    <div class="plan-price">₹17</div>
                    <small>Next 3 Matches</small>
                </div>
                <div class="plan-card" onclick="selectPlan('Yearly', 256)">
                    <strong>Yearly</strong>
                    <div class="plan-price">₹256</div>
                    <small>365 Days Access</small>
                </div>
            </div>
            <input type="text" id="userMobile" placeholder="Enter Mobile Number (e.g. 9569981484)">
            <button class="btn-pay" onclick="proceedToPayment()">Pay Payment</button>
        </div>

        <div id="step-processing" class="step" style="text-align: center;">
            <p><strong>Processing Payment...</strong></p>
            <div class="loader"></div>
            <p style="font-size: 13px; color: #666;">Please wait while we send verification code to admin.</p>
        </div>

        <div id="step-code" class="step">
            <p style="font-size: 13px; color: #333; text-align: center;">
                5-ank ka verification code admin (WhatsApp) par bhej diya gaya hai. Code admin se praapt karke yahan enter karein:
            </p>
            <input type="text" id="enteredCode" placeholder="Enter 5-Digit Code" maxlength="5">
            <button class="btn-pay" onclick="verifyCode()">Confirm Code</button>
        </div>

        <div id="step-success" class="step" style="text-align: center;">
            <h3 style="color: var(--primary-green); margin-bottom: 5px;">🎉 Subscription Active!</h3>
            <p style="font-size: 13px; color: #555;">Aapka subscription successfully activate ho gaya hai.</p>
        </div>
    </div>

    <!-- Match Schedule List -->
    <div class="card">
        <h4 style="margin-top:0; border-bottom: 2px solid #eee; padding-bottom: 8px;">Match Schedule List</h4>
        <div id="matches-container"></div>
    </div>

</div>

<!-- Admin Modal -->
<div id="admin-modal">
    <div class="admin-content">
        <h3 style="color: var(--primary-green); margin-top:0;">⚙️ Admin Control Panel</h3>
        <p id="admin-access-msg" style="font-size: 13px; color: #555;"></p>
        
        <!-- Match Scheduling Form (Only for Owner/Admin number: 9569981484) -->
        <div id="admin-form-section" style="background: #f9f9f9; padding: 10px; border-radius: 6px; margin-bottom: 15px; display: none;">
            <h4 id="form-title" style="margin-top:0; font-size:14px;">Schedule New Match</h4>
            <input type="hidden" id="editMatchIndex" value="-1">
            
            <label style="font-size: 12px; font-weight: bold;">Match Format:</label>
            <select id="matchFormat">
                <option value="T20">T20 Match</option>
                <option value="ODI">ODI Match</option>
                <option value="Test">Test Match</option>
            </select>

            <label style="font-size: 12px; font-weight: bold;">Match Venue:</label>
            <input type="text" id="matchVenue" placeholder="e.g. Wankhede Stadium, Mumbai">

            <label style="font-size: 12px; font-weight: bold;">Match Time & Date:</label>
            <input type="text" id="matchTime" placeholder="e.g. 28 Sep, 2026 - 07:30 PM">

            <label style="font-size: 12px; font-weight: bold;">Team 1:</label>
            <input type="text" id="teamA" placeholder="e.g. 🇮🇳 IND">

            <label style="font-size: 12px; font-weight: bold;">Team 2:</label>
            <input type="text" id="teamB" placeholder="e.g. WI 🌴">

            <button class="btn-pay" onclick="saveMatch()">Save Match Schedule</button>
        </div>

        <button class="btn-pay" style="background-color: #6c757d;" onclick="closeAdminPanel()">Close Panel</button>
    </div>
</div>

<script>
    let selectedPlanName = "";
    let selectedPlanPrice = 0;
    let generatedCode = "";
    const adminNumber = "9569981484";
    let isUserSubscribed = false;
    let currentUserMobile = "";

    let matches = [
        {
            format: "T20",
            venue: "International Cricket Stadium, Pune",
            time: "28 Sep, 2026 - 07:30 PM",
            teamA: "🇮🇳 IND",
            teamB: "WI 🌴"
        },
        {
            format: "ODI",
            venue: "Wankhede Stadium, Mumbai",
            time: "30 Sep, 2026 - 02:00 PM",
            teamA: "🇮🇳 IND",
            teamB: "WI 🌴"
        }
    ];

    function renderMatches() {
        let container = document.getElementById('matches-container');
        container.innerHTML = "";
        
        let canManage = (currentUserMobile === adminNumber);

        matches.forEach((match, index) => {
            let matchHtml = `
                <div class="match-box">
                    <div class="match-header">
                        <span><strong>[${match.format}]</strong> ${match.time}</span>
                        <span>📍 ${match.venue}</span>
                    </div>
                    <div class="teams-row">
                        <span>${match.teamA}</span>
                        <span style="color:#888; font-size:14px;">vs</span>
                        <span>${match.teamB}</span>
                    </div>
                    <div class="match-actions" style="display: ${canManage ? 'block' : 'none'};">
                        <button class="btn-edit" onclick="editMatch(${index})">Edit</button>
                        <button class="btn-delete" onclick="deleteMatch(${index})">Delete</button>
                    </div>
                </div>
            `;
            container.innerHTML += matchHtml;
        });
    }

    function selectPlan(name, price) {
        selectedPlanName = name;
        selectedPlanPrice = price;
        document.querySelectorAll('.plan-card').forEach(card => card.classList.remove('selected'));
        event.currentTarget.classList.add('selected');
    }

    function proceedToPayment() {
        let mobile = document.getElementById('userMobile').value.trim();

        if (!selectedPlanName) {
            alert("Kripya pehle koi ek plan select karein!");
            return;
        }
        if (!mobile || mobile.length < 10) {
            alert("Kripya apna valid mobile number enter karein!");
            return;
        }

        currentUserMobile = mobile;

        if (mobile === adminNumber) {
            isUserSubscribed = true;
            document.getElementById('step-plans').classList.remove('active');
            document.getElementById('step-success').classList.add('active');
            document.getElementById('step-success').innerHTML = `
                <h3 style="color: var(--primary-green); margin-bottom: 5px;">👑 Admin / Owner Free Access!</h3>
                <p style="font-size: 13px; color: #555;">Welcome! Aapke number (${adminNumber}) par free access active hai.</p>
            `;
            renderMatches();
            return;
        }

        document.getElementById('step-plans').classList.remove('active');
        document.getElementById('step-processing').classList.add('active');

        generatedCode = Math.floor(10000 + Math.random() * 90000).toString();

        let message = `New Subscription Request:%0APlan: ${selectedPlanName} (₹${selectedPlanPrice})%0AMobile: ${mobile}%0AVerification Code: *${generatedCode}*`;
        let whatsappURL = `https://wa.me/91${adminNumber}?text=${message}`;
        window.open(whatsappURL, '_blank');

        setTimeout(() => {
            document.getElementById('step-processing').classList.remove('active');
            document.getElementById('step-code').classList.add('active');
        }, 3000);
    }

    function verifyCode() {
        let userCode = document.getElementById('enteredCode').value.trim();

        if (userCode === generatedCode) {
            isUserSubscribed = true;
            document.getElementById('step-code').classList.remove('active');
            document.getElementById('step-success').classList.add('active');
            renderMatches();
        } else {
            alert("Galat code hai! Kripya WhatsApp par admin se mila sahi 5-ank ka code enter karein.");
        }
    }

    function openAdminPanel() {
        let msgBox = document.getElementById('admin-access-msg');
        let formSection = document.getElementById('admin-form-section');
        let canManage = (currentUserMobile === adminNumber);

        msgBox.innerHTML = "✅ Admin panel sabhi ke liye open hai. Lekin naye matches schedule karne ka adhikar sirf website owner ko hai.";

        if (canManage) {
            formSection.style.display = 'block';
        } else {
            formSection.style.display = 'none';
        }

        document.getElementById('admin-modal').style.display = 'flex';
    }

    function closeAdminPanel() {
        document.getElementById('admin-modal').style.display = 'none';
    }

    function saveMatch() {
        let format = document.getElementById('matchFormat').value;
        let venue = document.getElementById('matchVenue').value.trim();
        let time = document.getElementById('matchTime').value.trim();
        let teamA = document.getElementById('teamA').value.trim();
        let teamB = document.getElementById('teamB').value.trim();
        let editIndex = document.getElementById('editMatchIndex').value;

        if (!venue || !time || !teamA || !teamB) {
            alert("Kripya sabhi fields fill karein!");
            return;
        }

        if (editIndex == "-1") {
            matches.push({ format, venue, time, teamA, teamB });
        } else {
            matches[editIndex] = { format, venue, time, teamA, teamB };
            document.getElementById('editMatchIndex').value = "-1";
            document.getElementById('form-title').innerText = "Schedule New Match";
        }

        document.getElementById('matchVenue').value = "";
        document.getElementById('matchTime').value = "";
        document.getElementById('teamA').value = "";
        document.getElementById('teamB').value = "";

        renderMatches();
        alert("Match successfully scheduled!");
    }

    function editMatch(index) {
        let match = matches[index];
        document.getElementById('matchFormat').value = match.format;
        document.getElementById('matchVenue').value = match.venue;
        document.getElementById('matchTime').value = match.time;
        document.getElementById('teamA').value = match.teamA;
        document.getElementById('teamB').value = match.teamB;
        document.getElementById('editMatchIndex').value = index;
        document.getElementById('form-title').innerText = "Edit Match Schedule #" + (index + 1);
        document.getElementById('admin-modal').style.display = 'flex';
    }

    function deleteMatch(index) {
        if (confirm("Kya aap sach me is match ko delete karna chahte hain?")) {
            matches.splice(index, 1);
            renderMatches();
        }
    }

    window.onload = function() {
        renderMatches();
    };
</script>

</body>
</html>
