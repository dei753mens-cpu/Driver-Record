# Driver-Record
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Driver Pro Log | Responsive Fixed Structure</title>
    <style>
        :root { --primary: #1e293b; --accent: #2563eb; --ot: #ea580c; --warning: #dc2626; --bg: #f1f5f9; }
        
        /* Forces the page to respect the structure on all screens */
        html, body { 
            width: 100%;
            margin: 0;
            padding: 0;
            overflow-x: hidden; /* Prevents whole-page side-scrolling */
            background: var(--bg);
            font-family: -apple-system, system-ui, sans-serif;
            color: #334155;
        }

        .container { 
            width: 96%;
            max-width: 1200px;
            margin: 0 auto;
            padding: 15px 0;
        }

        .card { 
            background: white; 
            padding: 20px; 
            border-radius: 12px; 
            box-shadow: 0 4px 6px rgba(0,0,0,0.05); 
            margin-bottom: 20px;
            box-sizing: border-box;
        }

        h2 { margin: 0 0 15px 0; font-size: 1.2rem; color: var(--primary); border-bottom: 2px solid var(--bg); padding-bottom: 10px; }
        
        label { display: block; font-size: 0.75rem; font-weight: 700; margin-bottom: 4px; color: #64748b; text-transform: uppercase; }
        input, textarea { 
            width: 100% !important; 
            padding: 12px; 
            border: 1px solid #cbd5e1; 
            border-radius: 8px; 
            box-sizing: border-box; 
            font-size: 1rem; 
            margin-bottom: 10px; 
        }
        
        /* Grid maintains columns on desktop, stacks on mobile but keeps spacing */
        .filter-grid { 
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); 
            gap: 15px; 
            margin-bottom: 15px; 
        }
        
        .row-flex { display: flex; gap: 8px; margin-bottom: 8px; }
        
        .btn { border: none; border-radius: 8px; padding: 12px; font-weight: bold; cursor: pointer; transition: 0.2s; display: inline-flex; align-items: center; justify-content: center; }
        .btn-plus { background: #e2e8f0; min-width: 45px; color: var(--accent); font-size: 1.2rem; }
        .btn-main { background: var(--accent); color: white; width: 100%; font-size: 1rem; }
        .btn-edit { background: #f1f5f9; color: var(--accent); font-size: 0.75rem; padding: 8px; }
        .btn-del { background: #fee2e2; color: var(--warning); font-size: 0.75rem; padding: 8px; }
        .btn-download { background: #10b981; color: white; font-size: 0.85rem; padding: 8px 15px; }

        .week-nav { 
            display: flex; 
            justify-content: space-between; 
            align-items: center; 
            margin-bottom: 15px; 
            background: var(--primary); 
            color: white; 
            padding: 12px; 
            border-radius: 8px; 
        }

        .stats-grid { display: flex; gap: 15px; margin-bottom: 15px; }
        .stat-card { flex: 1; background: white; border-bottom: 4px solid var(--accent); padding: 15px; border-radius: 10px; text-align: center; }
        .stat-card b { display: block; font-size: 1.6rem; }

        /* The "Stretch" Logic for the Table */
        .table-outer-wrap {
            width: 100%;
            overflow: hidden; /* Clips children */
            border-radius: 8px;
            border: 1px solid #e2e8f0;
        }

        .table-wrap { 
            overflow-x: auto; /* Internal scrolling only */
            width: 100%;
            background: white;
        }

        table { 
            width: 100%; 
            border-collapse: collapse; 
            font-size: 0.85rem; 
            min-width: 950px; /* Maintains columns structure even on tiny screens */
        }

        th { background: #f8fafc; text-align: left; padding: 15px; border-bottom: 2px solid #e2e8f0; color: #64748b; white-space: nowrap; }
        td { padding: 12px 15px; border-bottom: 1px solid #f1f5f9; vertical-align: middle; }
        
        .detail-item { display: inline-block; background: #f8fafc; border: 1px solid #e2e8f0; padding: 2px 6px; border-radius: 4px; margin: 2px; font-family: monospace; font-size: 0.75rem; }
        .time-box { background: #f1f5f9; padding: 4px 8px; border-radius: 4px; font-weight: 600; color: var(--primary); display: inline-block; white-space: nowrap; }
        .hidden { display: none; }
    </style>
</head>
<body>

<div class="container">
    <div class="card">
        <h2 id="formTitle">Record New Shift</h2>
        <input type="hidden" id="editIndex" value="-1">
        
        <div class="filter-grid" style="grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));">
            <div><label>Date</label><input type="date" id="date"></div>
            <div><label>Start Time</label><input type="time" id="start"></div>
            <div><label>End Time</label><input type="time" id="end"></div>
        </div>

        <div class="filter-grid">
            <div id="vGroup"><label>Vehicles</label><div class="row-flex"><input type="text" class="v-in" placeholder="Reg #"><button class="btn btn-plus" onclick="addIn('vGroup','v-in')">+</button></div></div>
            <div id="tGroup"><label>Trailers</label><div class="row-flex"><input type="text" class="t-in" placeholder="Trailer #"><button class="btn btn-plus" onclick="addIn('tGroup','t-in')">+</button></div></div>
            <div id="lGroup"><label>Locations</label><div class="row-flex"><input type="text" class="l-in" placeholder="City/Site"><button class="btn btn-plus" onclick="addIn('lGroup','l-in')">+</button></div></div>
        </div>

        <label>Shift Notes</label>
        <textarea id="notes" placeholder="Delays, fuel, notes..."></textarea>
        
        <button class="btn btn-main" id="saveBtn" onclick="save()">Save Shift Entry</button>
        <button class="btn hidden" id="cancelBtn" onclick="resetForm()" style="width:100%; margin-top:10px; background:#e2e8f0; color:#334155;">Cancel Edit</button>
    </div>

    <div class="week-nav">
        <button class="btn" style="background:none; color:white; border:1px solid white;" onclick="changeWeek(-1)">◀ Prev</button>
        <span id="weekDisplay" style="font-weight: bold;">This Week</span>
        <button class="btn" style="background:none; color:white; border:1px solid white;" onclick="changeWeek(1)">Next ▶</button>
    </div>

    <div class="stats-grid">
        <div class="stat-card"><small>Normal Hours</small><b id="viewNormal">0.00</b></div>
        <div class="stat-card" style="border-bottom-color: var(--ot);"><small>Overtime</small><b id="viewOT" style="color:var(--ot);">0.00</b></div>
    </div>

    <div class="card">
        <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 15px; flex-wrap: wrap; gap: 10px;">
            <h2 style="border: none; margin: 0;">History Search</h2>
            <button class="btn btn-download" onclick="downloadCSV(shifts, 'Driver_Logs')">Download</button>
        </div>
        
        <div class="filter-grid">
            <div style="flex:2"><label>Search Everything</label><input type="text" id="searchKeyword" placeholder="Search..." onkeyup="updateUI()"></div>
            <div><label>From Date</label><input type="date" id="searchFrom" onchange="updateUI()"></div>
            <div><label>To Date</label><input type="date" id="searchTo" onchange="updateUI()"></div>
        </div>
        
        <div class="table-outer-wrap">
            <div class="table-wrap">
                <table>
                    <thead>
                        <tr>
                            <th width="120">Date</th>
                            <th width="180">Shift Times</th>
                            <th width="100">Total Hours</th>
                            <th>Vehicles</th>
                            <th>Trailers</th>
                            <th>Route/Stops</th>
                            <th width="120">Actions</th>
                        </tr>
                    </thead>
                    <tbody id="logBody"></tbody>
                </table>
            </div>
        </div>
    </div>
</div>

<script>
    let shifts = JSON.parse(localStorage.getItem('proLogs')) || [];
    let weekOffset = 0;

    function addIn(gid, cls, val='') {
        const group = document.getElementById(gid);
        const d = document.createElement('div');
        d.className = 'row-flex';
        d.innerHTML = `<input type="text" class="${cls}" value="${val}"><button class="btn btn-plus" onclick="addIn('${gid}','${cls}')">+</button>`;
        group.querySelectorAll('.btn-plus').forEach(btn => btn.style.visibility = 'hidden');
        group.appendChild(d);
        d.querySelector('.btn-plus').style.visibility = 'visible';
    }

    function save() {
        const dVal = document.getElementById('date').value;
        const sVal = document.getElementById('start').value;
        const eVal = document.getElementById('end').value;
        if(!dVal || !sVal || !eVal) return alert("Missing details");

        const shift = {
            d: dVal, s: sVal, e: eVal,
            v: Array.from(document.querySelectorAll('.v-in')).map(i=>i.value).filter(x=>x.trim()),
            t: Array.from(document.querySelectorAll('.t-in')).map(i=>i.value).filter(x=>x.trim()),
            l: Array.from(document.querySelectorAll('.l-in')).map(i=>i.value).filter(x=>x.trim()),
            n: document.getElementById('notes').value.trim()
        };

        let [sH, sM] = shift.s.split(':').map(Number);
        let [eH, eM] = shift.e.split(':').map(Number);
        let d1 = new Date(0,0,0, sH, sM);
        let d2 = new Date(0,0,0, eH, eM);
        if (d2 <= d1) d2.setDate(d2.getDate() + 1);
        shift.h = ((d2 - d1) / 3600000).toFixed(2);

        const idx = document.getElementById('editIndex').value;
        if(idx === "-1") shifts.push(shift); else shifts[idx] = shift;

        localStorage.setItem('proLogs', JSON.stringify(shifts));
        resetForm();
        updateUI();
    }

    function resetForm() {
        document.getElementById('editIndex').value = "-1";
        document.getElementById('formTitle').innerText = "Record New Shift";
        document.getElementById('saveBtn').innerText = "Save Shift Entry";
        document.getElementById('cancelBtn').classList.add('hidden');
        document.getElementById('date').value = "";
        document.getElementById('start').value = "";
        document.getElementById('end').value = "";
        document.getElementById('notes').value = "";
        ['vGroup','tGroup','lGroup'].forEach(id => {
            const label = id === 'vGroup' ? 'Vehicles' : id === 'tGroup' ? 'Trailers' : 'Locations';
            const cls = id === 'vGroup' ? 'v-in' : id === 'tGroup' ? 't-in' : 'l-in';
            document.getElementById(id).innerHTML = `<label>${label}</label><div class="row-flex"><input type="text" class="${cls}" placeholder="..."><button class="btn btn-plus" onclick="addIn('${id}','${cls}')">+</button></div>`;
        });
    }

    function getFilteredShifts() {
        const keyword = document.getElementById('searchKeyword').value.toLowerCase();
        const fromDate = document.getElementById('searchFrom').value;
        const toDate = document.getElementById('searchTo').value;
        
        return shifts.filter(s => {
            const searchStr = `${s.v} ${s.t} ${s.l} ${s.n}`.toLowerCase();
            return searchStr.includes(keyword) && (!fromDate || s.d >= fromDate) && (!toDate || s.d <= toDate);
        });
    }

    function updateUI() {
        const body = document.getElementById('logBody');
        body.innerHTML = "";

        shifts.sort((a,b) => new Date(a.d) - new Date(b.d));
        let weekCounts = {}; 
        
        shifts.forEach(s => {
            let d = new Date(s.d);
            let diff = d.getDate() - d.getDay() + (d.getDay() === 0 ? -6 : 1);
            let mon = new Date(d.setDate(diff)).toDateString();
            if(!weekCounts[mon]) weekCounts[mon] = 0;
            weekCounts[mon]++;
            let baseOT = parseFloat(s.h) > 9.25 ? parseFloat(s.h) - 9.25 : 0;
            if(weekCounts[mon] > 5) { s.calculatedOT = parseFloat(s.h); s.calculatedNormal = 0; } 
            else { s.calculatedOT = baseOT; s.calculatedNormal = parseFloat(s.h) - baseOT; }
        });

        const range = getWeekRange(weekOffset);
        document.getElementById('weekDisplay').innerText = weekOffset === 0 ? "This Week" : range.start.toLocaleDateString() + " - " + range.end.toLocaleDateString();
        
        let statsNormal = 0, statsOT = 0;

        getFilteredShifts().slice().reverse().forEach((s) => {
            const originalIndex = shifts.indexOf(s);
            const r = body.insertRow();
            r.innerHTML = `
                <td><b>${s.d.split('-').reverse().join('/')}</b>${s.n ? '<span class="note-marker" title="'+s.n+'">📝</span>' : ''}</td>
                <td><span class="time-box">${s.s}</span> to <span class="time-box">${s.e}</span></td>
                <td><b>${s.h}h</b>${s.calculatedOT > 0 ? '<br><small style="color:var(--ot);">+'+s.calculatedOT.toFixed(2)+' OT</small>' : ''}</td>
                <td>${s.v.map(x=>`<span class="detail-item">${x}</span>`).join('')}</td>
                <td>${s.t.map(x=>`<span class="detail-item">${x}</span>`).join('')}</td>
                <td>${s.l.map(x=>`<span class="detail-item">${x}</span>`).join('')}</td>
                <td><div style="display:flex; gap:5px;"><button class="btn btn-edit" onclick="editShift(${originalIndex})">Edit</button><button class="btn btn-del" onclick="deleteShift(${originalIndex})">X</button></div></td>
            `;
        });

        shifts.forEach(s => {
            const sDate = new Date(s.d);
            if(sDate >= range.start && sDate <= range.end) { statsNormal += (s.calculatedNormal || 0); statsOT += (s.calculatedOT || 0); }
        });

        document.getElementById('viewNormal').innerText = statsNormal.toFixed(2);
        document.getElementById('viewOT').innerText = statsOT.toFixed(2);
    }

    function downloadCSV(dataArray, filename) {
        if(dataArray.length === 0) return alert("No entries to download.");

        let csv = "Date,Start,End,Duration,Normal,OT,Vehicles,Trailers,Locations,Notes\n";
        dataArray.forEach(s => {
            let row = [
                s.d, s.s, s.e, s.h, (s.calculatedNormal || 0).toFixed(2), (s.calculatedOT || 0).toFixed(2),
                `"${s.v.join(', ')}"`, `"${s.t.join(', ')}"`, `"${s.l.join(' > ')}"`, `"${(s.n || '').replace(/"/g, '""')}"`
            ];
            csv += row.join(",") + "\n";
        });

        const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' });
        const link = document.createElement("a");
        link.href = URL.createObjectURL(blob);
        link.setAttribute("download", `${filename}_${new Date().toISOString().slice(0,10)}.csv`);
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
    }

    function getWeekRange(offset) {
        let now = new Date();
        let diff = now.getDate() - now.getDay() + (now.getDay() === 0 ? -6 : 1);
        let start = new Date(now.setDate(diff + (offset * 7)));
        start.setHours(0,0,0,0);
        let end = new Date(start); end.setDate(end.getDate() + 6); end.setHours(23,59,59,999);
        return { start, end };
    }

    function editShift(i) {
        const s = shifts[i];
        document.getElementById('editIndex').value = i;
        document.getElementById('date').value = s.d;
        document.getElementById('start').value = s.s;
        document.getElementById('end').value = s.e;
        document.getElementById('notes').value = s.n || "";
        ['vGroup','tGroup','lGroup'].forEach(id => {
            document.getElementById(id).innerHTML = `<label>${id === 'vGroup' ? 'Vehicles' : id === 'tGroup' ? 'Trailers' : 'Locations'}</label>`;
            if(s[id[0]].length) s[id[0]].forEach(v => addIn(id, id[0]+'-in', v)); else addIn(id, id[0]+'-in');
        });
        document.getElementById('formTitle').innerText = "Edit Entry";
        document.getElementById('saveBtn').innerText = "Update Entry";
        document.getElementById('cancelBtn').classList.remove('hidden');
        window.scrollTo(0,0);
    }

    function changeWeek(dir) { weekOffset += dir; updateUI(); }
    function deleteShift(i) { if(confirm("Delete?")) { shifts.splice(i, 1); localStorage.setItem('proLogs', JSON.stringify(shifts)); updateUI(); } }
    updateUI();
</script>
</body>
</html>
