<!-- ADMIN DASHBOARD - Add this before </body> -->
<div id="adminPanel" style="display: none; position: fixed; top: 20px; right: 20px; background: white; border: 2px solid #333; border-radius: 10px; padding: 20px; max-width: 400px; max-height: 80vh; overflow-y: auto; z-index: 9999;">
  <div style="display: flex; justify-content: space-between; margin-bottom: 15px;">
    <h3 style="color: #000;">👥 Registered Members</h3>
    <button onclick="closeAdminPanel()" style="background: none; border: none; font-size: 1.5rem; cursor: pointer;">&times;</button>
  </div>
  <div id="membersList"></div>
  <button onclick="exportMembers()" style="width: 100%; padding: 10px; background: #000; color: white; border: none; border-radius: 5px; margin-top: 10px;">📥 Export to Excel</button>
</div>

<!-- Admin Button (click to view members) -->
<button onclick="openAdminPanel()" style="position: fixed; bottom: 20px; right: 20px; background: #000; color: white; border: none; border-radius: 50px; padding: 12px 20px; cursor: pointer; z-index: 9998; box-shadow: 0 4px 10px rgba(0,0,0,0.2);">
  👑 Admin
</button>

<script>
// Admin Functions
function openAdminPanel() {
  const users = JSON.parse(localStorage.getItem('cueUsers') || '{}');
  const membersList = document.getElementById('membersList');
  
  let html = '<table style="width:100%; border-collapse: collapse;">';
  html += '<tr style="background:#f0f0f0;"><th>Name</th><th>Phone</th><th>Country</th><th>Date</th></tr>';
  
  Object.values(users).forEach(user => {
    html += `<tr style="border-bottom:1px solid #ddd;">
      <td style="padding:8px;">${user.fullName || 'N/A'}</td>
      <td style="padding:8px;">${user.phone || 'N/A'}</td>
      <td style="padding:8px;">${user.country || 'N/A'}</td>
      <td style="padding:8px;">${user.registeredDate || 'N/A'}</td>
    </tr>`;
  });
  
  html += '</table>';
  membersList.innerHTML = html;
  document.getElementById('adminPanel').style.display = 'block';
}

function closeAdminPanel() {
  document.getElementById('adminPanel').style.display = 'none';
}

function exportMembers() {
  const users = JSON.parse(localStorage.getItem('cueUsers') || '{}');
  let csv = "Name,Phone,Country,Registration Date\n";
  
  Object.values(users).forEach(user => {
    csv += `"${user.fullName || ''}","${user.phone || ''}","${user.country || ''}","${user.registeredDate || ''}"\n`;
  });
  
  const blob = new Blob([csv], { type: 'text/csv' });
  const url = window.URL.createObjectURL(blob);
  const a = document.createElement('a');
  a.href = url;
  a.download = 'registered_members.csv';
  a.click();
}
</script>￼Enter
