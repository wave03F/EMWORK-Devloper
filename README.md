<!-- Part 1: ระบบขออนุญาตลาหยุด -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Leave Request System</title>
    <link rel="stylesheet" href="styles.css">
    <script src="script.js" defer></script>
</head>
<body>
    <h1>ระบบขออนุญาตลาหยุด</h1>

    <!-- ฟอร์มบันทึกการลาหยุด -->
    <form id="leaveForm">
        <label>ชื่อ - นามสกุล: <input type="text" id="fullName" required></label><br>
        <label>สังกัด/ตำแหน่ง: <input type="text" id="department"></label><br>
        <label>อีเมล์: <input type="email" id="email"></label><br>
        <label>เบอร์โทรศัพท์: <input type="tel" id="phone" required></label><br>
        <label>ประเภทการลา:
            <select id="leaveType">
                <option value="ลาป่วย">ลาป่วย</option>
                <option value="ลากิจ">ลากิจ</option>
                <option value="พักร้อน">พักร้อน</option>
                <option value="อื่นๆ">อื่นๆ</option>
            </select>
        </label><br>
        <label>สาเหตุการลา: <textarea id="reason" required></textarea></label><br>
        <label>วันที่ขอลา: <input type="date" id="startDate" required></label> ถึง
        <label><input type="date" id="endDate" required></label><br>
        <button type="submit">บันทึก</button>
    </form>

    <!-- รายการขอลาหยุด -->
    <h2>รายการขอลาหยุด</h2>
    <table id="leaveTable">
        <thead>
            <tr>
                <th>ชื่อ - นามสกุล</th>
                <th>ประเภทการลา</th>
                <th>วันที่ขอลา</th>
                <th>สถานะ</th>
                <th>การจัดการ</th>
            </tr>
        </thead>
        <tbody>
            <!-- Rows will be populated dynamically -->
        </tbody>
    </table>

    <!-- Part 2: แสดงผล UX/UI -->
    <h2>แสดงผลแบบ UX/UI</h2>
    <div id="temple" class="templeall">
        <img src="2024-11-01 07_25_57-ดำเนินการจัดทำการแสดงผลด้วย HTML.docx  -  มุมมองที่ได้รับการป้องกัน - Word.png" alt="Temple by Region" style="width:100%; height:auto;">
    </div>
    
    <button onclick="toggleAutoPlay()">Toggle Auto Play</button>

    <script>
        // Part 1: Script สำหรับการจัดการฟอร์มลาหยุด
        document.getElementById('leaveForm').addEventListener('submit', function(event) {
            event.preventDefault();

            const fullName = document.getElementById('fullName').value;
            const leaveType = document.getElementById('leaveType').value;
            const startDate = new Date(document.getElementById('startDate').value);
            const endDate = new Date(document.getElementById('endDate').value);
            const today = new Date();

            // Validate leave type: พักร้อน
            if (leaveType === 'พักร้อน') {
                const diffDays = (startDate - today) / (1000 * 60 * 60 * 24);
                const duration = (endDate - startDate) / (1000 * 60 * 60 * 24);

                if (diffDays < 3) {
                    alert('การพักร้อนต้องลาล่วงหน้าอย่างน้อย 3 วัน');
                    return;
                }

                if (duration > 2) {
                    alert('การพักร้อนลาติดต่อกันได้ไม่เกิน 2 วัน');
                    return;
                }
            }

            // Validate non-retroactive dates
            if (startDate < today) {
                alert('ไม่อนุญาตให้บันทึกวันลาย้อนหลัง');
                return;
            }

            // Append new leave request to the table
            const table = document.getElementById('leaveTable').querySelector('tbody');
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${fullName}</td>
                <td>${leaveType}</td>
                <td>${startDate.toISOString().split('T')[0]} - ${endDate.toISOString().split('T')[0]}</td>
                <td>รอพิจารณา</td>
                <td><button onclick="approveLeave(this)">อนุมัติ</button> <button onclick="rejectLeave(this)">ไม่อนุมัติ</button></td>
            `;
            table.appendChild(row);
        });

        // Approve leave request
        function approveLeave(button) {
            const row = button.parentNode.parentNode;
            row.cells[3].innerText = 'อนุมัติ';
        }

        // Reject leave request
        function rejectLeave(button) {
            const row = button.parentNode.parentNode;
            row.cells[3].innerText = 'ไม่อนุมัติ';
        }

        // Part 2: Script สำหรับ UX/UI
        let autoPlay = true;
        function toggleAutoPlay() {
            autoPlay = !autoPlay;
            alert(autoPlay ? 'Auto Play ON' : 'Auto Play OFF');
        }
    </script>
</body>
</html>
