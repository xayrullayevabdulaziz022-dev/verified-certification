<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Verified Certification</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
body { margin:0; font-family: Arial, Helvetica, sans-serif; background:#fff; color:#1f2937; }
header { padding:20px 40px; border-bottom:1px solid #e5e7eb; font-weight:bold; font-size:20px; color:#2563eb;}
main { max-width:900px; margin:60px auto; padding:0 20px; text-align:center; }
h1 { font-size:36px; margin-bottom:10px; }
p { color:#6b7280; font-size:16px; }
.box { margin-top:40px; padding:40px; border:1px solid #e5e7eb; border-radius:12px; }
input[type="text"], input[type="file"], input[type="password"] { width:100%; padding:12px; margin-top:10px; margin-bottom:20px; border-radius:8px; border:1px solid #d1d5db; font-size:16px; }
button { padding:12px 30px; background:#2563eb; color:white; border:none; border-radius:8px; font-size:16px; cursor:pointer; }
button:hover { background:#1d4ed8; }
.result { margin-top:30px; font-size:18px; font-weight:bold; }
.admin-section { margin-top:50px; border-top:1px solid #e5e7eb; padding-top:30px; text-align:left; }
.admin-section h2 { color:#2563eb; margin-bottom:20px; }
table { width:100%; border-collapse: collapse; margin-top:20px; }
th, td { border:1px solid #d1d5db; padding:10px; text-align:center; }
th { background:#f3f4f6; }
footer { margin-top:80px; padding:20px; font-size:14px; color:#9ca3af; border-top:1px solid #e5e7eb; }
.cert-image { margin-top:20px; max-width:100%; border:1px solid #e5e7eb; border-radius:8px; }
</style>
</head>
<body>

<header>Verified Certification</header>

<main>
<h1>Verify a Certificate</h1>
<p>Upload a certificate or scan the QR code to verify authenticity.</p>

<div class="box">
  <h3>Upload Certificate (PDF)</h3>
  <input type="file" accept="application/pdf">

  <hr style="margin:40px 0; border:none; border-top:1px solid #e5e7eb;">

  <h3>Verify by Certificate ID</h3>
  <input type="text" id="certId" placeholder="Example: VC-001">
  <button onclick="verify()">Verify Certificate</button>

  <div id="result" class="result"></div>
  <img id="certImage" class="cert-image" style="display:none;" />
</div>

<div class="admin-section">
  <h2>Admin Panel (Demo Password: admin123)</h2>
  <input type="password" id="adminPass" placeholder="Enter admin password">
  <button onclick="loginAdmin()">Login</button>

  <div id="adminControls" style="display:none;">
    <h3>Add Certificate</h3>
    <input type="text" id="newCertId" placeholder="Certificate ID e.g. VC-101">
    <input type="text" id="newCertName" placeholder="Full Name">
    <input type="text" id="newCertImg" placeholder="Image URL (hosted online)">
    <button onclick="addCert()">Add Certificate</button>

    <h3>Current Certificates</h3>
    <table id="certTable">
      <tr><th>ID</th><th>Name</th><th>Action</th></tr>
    </table>
  </div>
</div>

</main>

<footer>© 2026 Verified Certification. All rights reserved.</footer>

<script>
// ===== DATABASE =====
let certificates = [
  {id:"VC-001", name:"John Smith", img:"https://i.imgur.com/abc123.png"},
  {id:"VC-002", name:"Anna Brown", img:"https://i.imgur.com/def456.png"},
  {id:"VC-003", name:"Michael Lee", img:"https://i.imgur.com/ghi789.png"}
  // Shu yerga VC-100 gacha qo‘shing va img URL berishingiz mumkin
];

// ===== VERIFY FUNCTION =====
function verify(){
  const id = document.getElementById("certId").value.trim().toUpperCase();
  const result = document.getElementById("result");
  const img = document.getElementById("certImage");
  img.style.display="none";

  if(!id) return;

  const cert = certificates.find(c => c.id === id);
  if(cert){
    result.style.color="green";
    result.innerHTML = `✅ Certificate is valid: ${cert.id} - ${cert.name}`;
    if(cert.img){
      img.src = cert.img;
      img.style.display="block";
    }
  } else {
    result.style.color="red";
    result.innerHTML = "❌ Certificate not found or invalid.";
  }
}

// ===== QR AUTO-READ =====
const params = new URLSearchParams(window.location.search);
const certFromQR = params.get("cert");
if(certFromQR){
  document.getElementById("certId").value = certFromQR.toUpperCase();
  verify();
}

// ===== ADMIN LOGIN =====
function loginAdmin(){
  const pass = document.getElementById("adminPass").value;
  if(pass==="admin123"){
    document.getElementById("adminControls").style.display="block";
    alert("Admin logged in!");
    renderTable();
  } else {
    alert("Incorrect password!");
  }
}

// ===== ADMIN ADD CERTIFICATE =====
function addCert(){
  const id = document.getElementById("newCertId").value.trim().toUpperCase();
  const name = document.getElementById("newCertName").value.trim();
  const img = document.getElementById("newCertImg").value.trim();
  if(!id || !name){ alert("Fill ID and Name!"); return; }

  if(certificates.some(c=>c.id===id)){ alert("Certificate ID already exists"); return; }

  certificates.push({id:id, name:name, img:img});
  alert(`Certificate ${id} added`);
  renderTable();
}

// ===== RENDER TABLE =====
function renderTable(){
  const table = document.getElementById("certTable");
  table.innerHTML = "<tr><th>ID</th><th>Name</th><th>Action</th></tr>";
  certificates.forEach((c,i)=>{
    const row = table.insertRow();
    row.insertCell(0).innerText = c.id;
    row.insertCell(1).innerText = c.name;
    const btnCell = row.insertCell(2);
    const delBtn = document.createElement("button");
    delBtn.innerText="Delete";
    delBtn.style.background="#ef4444";
    delBtn.onclick = ()=>{ certificates.splice(i,1); renderTable(); };
    btnCell.appendChild(delBtn);
  });
}
</script>

</body>
</html>