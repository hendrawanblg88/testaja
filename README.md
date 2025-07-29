<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>🎁 Mystery Box - Ultimaslot 🎁</title>
  <style>
    body {
      background: linear-gradient(to right, #f1c232, #fff0c4, #f1c232);
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      color: white;
      text-align: center;
      padding: 20px;
      min-height: 100vh;
      margin: 0;
    }
    input, button {
      padding: 10px;
      margin: 5px;
      font-size: 16px;
    }
    #boxContainer {
      display: none;
      display: grid;
      grid-template-columns: repeat(3, 1fr);
      gap: 20px;
      justify-items: center;
      margin-top: 20px;
    }
    .box {
      width: 100px;
      height: 100px;
      background-color: red;
      border-radius: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      cursor: pointer;
      color: white;
      font-weight: bold;
    }
    .selected {
      background-color: gold;
      color: black;
    }
  </style>
</head>
<body>
  <h1>🎁 Mystery Box ULTIMASLOT 🎁</h1>

  <div id="form">
    <input type="text" id="id" placeholder="Masukkan ID Anda">
    <input type="text" id="kupon" placeholder="Masukkan Kode Kupon">
    <button onclick="verifikasi()">Mulai Game</button>
  </div>

  <div id="boxContainer"></div>
  <div id="result"></div>

  <script>
    const endpoint = "https://script.google.com/macros/s/AKfycbz18VrmRMZgmFiukKbJveShsJdynNa9SeFpae_CgRur1_jIRwbDQGWQJ6uL9IJhbiLO/exec";
    let rowIndex = null;
    let hadiahList = [1000, 2000, 3000, 5000, 8000, 10000, 100000, 1000000, 10000000];

    async function verifikasi() {
      const id = document.getElementById('id').value.trim();
      const kupon = document.getElementById('kupon').value.trim();
      if (!id || !kupon) {
        alert("Masukkan ID dan Kupon!");
        return;
      }

      try {
        const res = await fetch(`${endpoint}?id=${encodeURIComponent(id)}&kupon=${encodeURIComponent(kupon)}`);
        const data = await res.json();

        if (data.status === 'valid') {
          rowIndex = data.row;
          document.getElementById('form').style.display = 'none';
          document.getElementById('boxContainer').style.display = 'grid';
          mulaiGame();
        } else if (data.status === 'used') {
          alert('Kupon sudah digunakan.');
        } else {
          alert('ID atau kupon tidak valid.');
        }
      } catch (e) {
        alert('Gagal menghubungi server.');
        console.error(e);
      }
    }

    function mulaiGame() {
      hadiahList = shuffle(hadiahList);
      const container = document.getElementById('boxContainer');
      container.innerHTML = '';

      for (let i = 0; i < 9; i++) {
        const box = document.createElement('div');
        box.className = 'box';
        box.textContent = "🎁";
        box.dataset.hadiah = hadiahList[i];
        box.onclick = () => bukaKotak(box);
        container.appendChild(box);
      }
    }

    function bukaKotak(box) {
      const hadiah = box.dataset.hadiah;
      box.classList.add('selected');
      box.textContent = `Rp ${parseInt(hadiah).toLocaleString()}`;

      // Simpan hasil ke GAS
      fetch(endpoint, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded'
        },
        body: `row=${rowIndex}&hadiah=${encodeURIComponent(hadiah)}`
      });

      document.getElementById('result').textContent = `Selamat! Kamu mendapatkan Rp ${parseInt(hadiah).toLocaleString()}`;

      // Nonaktifkan semua kotak lain
      document.querySelectorAll('.box').forEach(b => b.onclick = null);
    }

    function shuffle(arr) {
      let currentIndex = arr.length, randomIndex;
      while (currentIndex !== 0) {
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex--;
        [arr[currentIndex], arr[randomIndex]] = [arr[randomIndex], arr[currentIndex]];
      }
      return arr;
    }
  </script>
</body>
</html>
