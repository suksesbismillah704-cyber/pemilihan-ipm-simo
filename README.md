<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Pemilihan Formatur PC IPM Simo 2026-2028</title>
  <style>
    body {
      margin: 0;
      font-family: 'Trebuchet MS', sans-serif;
      background: linear-gradient(135deg, #fff8dc, #ffffff);
      color: #333;
      overflow: hidden;
    }
    .slide {
      width: 100vw;
      height: 100vh;
      display: none;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      text-align: center;
      background-image: url('https://i.imgur.com/8QZQZQZ.png'); /* nuansa anime Naruto */
      background-size: cover;
      background-position: center;
    }
    .slide.active { display: flex; }
    h1, h2 { color: #f4b400; text-shadow: 2px 2px 4px #000; }
    .card {
      background: rgba(255,255,255,0.9);
      padding: 20px;
      border-radius: 15px;
      width: 90%;
      max-width: 600px;
    }
    input, button {
      padding: 10px;
      margin: 8px;
      width: 80%;
      border-radius: 8px;
      border: 1px solid #ccc;
    }
    button {
      background: #f4b400;
      color: #fff;
      font-weight: bold;
      cursor: pointer;
    }
    button:hover { background: #d39e00; }
    .candidates label {
      display: block;
      text-align: left;
      margin: 5px 0;
    }
    img.logo { width: 120px; margin-bottom: 10px; }
  </style>
</head>
<body>

  <!-- Slide 1 -->
  <div class="slide active" id="slide1">
    <div class="card">
      <img src="https://upload.wikimedia.org/wikipedia/id/3/3c/Logo_IPM.png" alt="Logo IPM" class="logo">
      <h1>Pemilihan Formatur</h1>
      <h2>PC IPM Kecamatan Simo<br>Periode 2026-2028</h2>
      <input type="text" id="nama" placeholder="Masukkan Nama" />
      <input type="password" id="kode" placeholder="Masukkan Kode Pemilih" />
      <button onclick="nextSlide()">Lanjut</button>
    </div>
  </div>

  <!-- Slide 2 -->
  <div class="slide" id="slide2">
    <div class="card">
      <h2>Pilih Maksimal 9 Kandidat</h2>
      <div class="candidates" id="candidates"></div>
      <button onclick="submitVote()">Kirim Suara</button>
    </div>
  </div>

  <!-- Admin Login -->
    <!-- Admin Panel -->
  <div class="slide" id="admin">
    <div class="card">
      <h2>Panel Admin</h2>
      <input type="password" id="adminPass" placeholder="Password Admin" />
      <button onclick="adminLogin()">Login</button>
      <hr>
      <div id="adminPanel" style="display:none; text-align:left;">
        <h3>Perolehan Suara</h3>
        <div id="hasil"></div>
        <hr>
        <h3>Edit Website</h3>
        <label>Judul Website</label>
        <input type="text" id="editJudul" placeholder="Edit Judul" />
        <button onclick="updateJudul()">Simpan Judul</button>
        <label>Upload Foto Kandidat (URL)</label>
        <input type="text" id="fotoKandidat" placeholder="URL Foto" />
        <input type="text" id="namaKandidat" placeholder="Nama Kandidat" />
        <button onclick="addCandidate()">Tambah Kandidat</button>
      </div>
    </div>
  </div>
    </div>
  </div>

<script>
  const kandidat = [
    'Kandidat 1','Kandidat 2','Kandidat 3','Kandidat 4','Kandidat 5',
    'Kandidat 6','Kandidat 7','Kandidat 8','Kandidat 9','Kandidat 10','Kandidat 11'
  ];

  const maxPilihan = 9;
  const votes = JSON.parse(localStorage.getItem('votes') || '{}');

  const container = document.getElementById('candidates');
  kandidat.forEach(k => {
    container.innerHTML += `<label><input type="checkbox" value="${k}" /> ${k}</label>`;
  });

  function nextSlide() {
    if(localStorage.getItem('sudahMemilih')){
      alert('Anda sudah pernah memilih!');
      return;
    }
    document.getElementById('slide1').classList.remove('active');
    document.getElementById('slide2').classList.add('active');
  }

  function submitVote() {
    const checked = document.querySelectorAll('input[type=checkbox]:checked');
    if(checked.length > maxPilihan){
      alert('Maksimal memilih 9 kandidat');
      return;
    }
    checked.forEach(c => {
      votes[c.value] = (votes[c.value] || 0) + 1;
    });
    localStorage.setItem('votes', JSON.stringify(votes));
    localStorage.setItem('sudahMemilih', 'true');
    alert('Terima kasih, suara Anda berhasil disimpan!');
    location.reload();
  }

  function adminLogin(){
    const pass = document.getElementById('adminPass').value;
    if(pass !== 'admin123'){
      alert('Password salah'); return;
    }
    document.getElementById('adminPanel').style.display = 'block';
    let hasil = '';
    for(const k in votes){ hasil += `<p>${k}: ${votes[k]} suara</p>`; }
    document.getElementById('hasil').innerHTML = hasil;
  }

  function updateJudul(){
    const judul = document.getElementById('editJudul').value;
    if(judul){
      localStorage.setItem('judulWebsite', judul);
      alert('Judul diperbarui, refresh halaman');
    }
  }

  function addCandidate(){
    const nama = document.getElementById('namaKandidat').value;
    const foto = document.getElementById('fotoKandidat').value;
    if(!nama || !foto){ alert('Lengkapi data'); return; }
    const data = JSON.parse(localStorage.getItem('kandidatCustom') || '[]');
    data.push({nama,foto});
    localStorage.setItem('kandidatCustom', JSON.stringify(data));
    alert('Kandidat ditambahkan, refresh halaman');
  }

  document.addEventListener('DOMContentLoaded',()=>{
    const judul = localStorage.getItem('judulWebsite');
    if(judul){ document.querySelector('h1').innerText = judul; }
    const custom = JSON.parse(localStorage.getItem('kandidatCustom')||'[]');
    custom.forEach(c=>{
      container.innerHTML += `<label><input type="checkbox" value="${c.nama}" /> <img src="${c.foto}" width="40"/> ${c.nama}</label>`;
    });
  });
(){
    const pass = document.getElementById('adminPass').value;
    if(pass !== 'admin123'){
      alert('Password salah'); return;
    }
    let hasil = '<h3>Perolehan Suara</h3>';
    for(const k in votes){ hasil += `<p>${k}: ${votes[k]} suara</p>`; }
    document.getElementById('hasil').innerHTML = hasil;
  }

  // Shortcut admin: tekan A
  document.addEventListener('keydown', e => {
    if(e.key === 'a'){
      document.querySelectorAll('.slide').forEach(s=>s.classList.remove('active'));
      document.getElementById('admin').classList.add('active');
    }
  });
</script>
</body>
</html>
