
<html lang="vi">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Quản Lý Tài Khoản</title>
  <style>
      body { font-family: Arial, sans-serif; background-color: #f4f4f4; text-align: center; padding: 20px; }
      .container { background: white; padding: 20px; max-width: 400px; margin: auto; border-radius: 8px; box-shadow: 0 0 10px rgba(0, 0, 0, 0.1); }
      table { width: 100%; margin-top: 10px; border-collapse: collapse; }
      th, td { border: 1px solid #ddd; padding: 8px; text-align: left; }
      th { background-color: #007bff; color: white; }
      input, button { margin: 5px; padding: 8px; width: 90%; }
  </style>
</head>
<body>

<div class="container">
  <h2>Quản Lý Tài Khoản</h2>
  
  <h3>Tìm tài khoản</h3>
  <input type="email" id="email" placeholder="Nhập email tài khoản" oninput="searchAccount()">

  <div id="accountInfo" style="display: none;">
      <h3>Thông tin tài khoản</h3>
      <table>
          <tr><th>Tài khoản</th><td id="tk"></td></tr>
          <tr><th>Thời gian bắt đầu</th><td id="batdau"></td></tr>
          <tr><th>Thời gian kết thúc</th><td id="ketthuc"></td></tr>
          <tr><th>Số lần gia hạn</th><td id="giahan"></td></tr>
      </table>
  </div>

  <h3>Thêm tài khoản mới</h3>
  <input type="email" id="newEmail" placeholder="Nhập email mới">
  <input type="date" id="newBatDau">
  <input type="date" id="newKetThuc">
  <input type="number" id="newGiaHan" placeholder="Số lần gia hạn">
  <!-- Thêm trường mật khẩu admin -->
  <input type="password" id="adminPass" placeholder="Nhập mật khẩu admin">
  <button onclick="addAccount()">Thêm Tài Khoản</button>
</div>

<script>
  // Mật khẩu admin được định nghĩa cố định (chỉ mang tính chất minh họa)
  const ADMIN_PASS = "ngoctuanbui2003";

  // Hàm chuyển đổi từ yyyy-mm-dd sang dd/mm/yyyy
  function formatDate(isoDate) {
      if (!isoDate) return "";
      let parts = isoDate.split("-");
      return `${parts[2]}/${parts[1]}/${parts[0]}`; // Đổi sang định dạng dd/mm/yyyy
  }

  // Lấy danh sách tài khoản từ LocalStorage (nếu có)
  let taiKhoanList = JSON.parse(localStorage.getItem("taiKhoanList")) || [];

  function searchAccount() {
      let email = document.getElementById("email").value.trim();
      let account = taiKhoanList.find(acc => acc.tai_khoan === email);

      if (account) {
          document.getElementById("tk").innerText = account.tai_khoan;
          document.getElementById("batdau").innerText = account.thoi_gian_bat_dau;
          document.getElementById("ketthuc").innerText = account.thoi_gian_ket_thuc;
          document.getElementById("giahan").innerText = account.so_lan_gia_han;
          document.getElementById("accountInfo").style.display = "block";
      } else {
          document.getElementById("accountInfo").style.display = "none";
      }
  }

  function addAccount() {
      let newEmail = document.getElementById("newEmail").value.trim();
      let newBatDau = document.getElementById("newBatDau").value;
      let newKetThuc = document.getElementById("newKetThuc").value;
      let newGiaHan = document.getElementById("newGiaHan").value;
      let adminPassInput = document.getElementById("adminPass").value;

      if (!newEmail || !newBatDau || !newKetThuc || !newGiaHan || !adminPassInput) {
          alert("Vui lòng nhập đầy đủ thông tin!");
          return;
      }

      // Kiểm tra mật khẩu admin
      if (adminPassInput !== ADMIN_PASS) {
          alert("Mật khẩu admin không đúng! Không được phép thêm tài khoản.");
          return;
      }

      // Kiểm tra nếu email đã tồn tại
      if (taiKhoanList.some(acc => acc.tai_khoan === newEmail)) {
          alert("Tài khoản này đã tồn tại!");
          return;
      }

      let newAccount = {
          tai_khoan: newEmail,
          thoi_gian_bat_dau: formatDate(newBatDau), // Lưu theo dd/mm/yyyy
          thoi_gian_ket_thuc: formatDate(newKetThuc),
          so_lan_gia_han: newGiaHan
      };

      taiKhoanList.push(newAccount);
      localStorage.setItem("taiKhoanList", JSON.stringify(taiKhoanList));

      alert("Thêm tài khoản thành công!");
      // Xóa các trường nhập sau khi thêm thành công
      document.getElementById("newEmail").value = "";
      document.getElementById("newBatDau").value = "";
      document.getElementById("newKetThuc").value = "";
      document.getElementById("newGiaHan").value = "";
      document.getElementById("adminPass").value = "";
  }
</script>

</body>
</html>
