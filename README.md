# Lab8Web

Angga Thifal Ananda
312010419
TI.20.B2

# PRAKTIKUM 8
Seperti praktikum 7, kita buat folder baru di htdocs dengan nama lab8_php_database
![Screenshot (191)](https://user-images.githubusercontent.com/73052649/170809005-23169b65-e3ff-46a2-ac82-58eba811bb40.png)
Setelah itu Jalankan MySql Server dari menu XAMPP Control.
![Screenshot (190)](https://user-images.githubusercontent.com/73052649/170809014-efea09b3-078a-433d-a18c-10efc0bb907f.png)

# A. Membuat Database
# Mengakses MySql Client menggunakan PHP MyAdmin.

Buka link melalui browser : http://localhost/phpmyadmin/
![Screenshot (192)](https://user-images.githubusercontent.com/73052649/170809046-a7f128ea-051a-4992-b45f-e86e3e493337.png)

# Membuat Tabel Database
```
CREATE TABLE data_barang (
 id_barang int(10) auto_increment Primary Key,
 kategori varchar(30),
 nama varchar(30),
 gambar varchar(100),
 harga_beli decimal(10,0),
 harga_jual decimal(10,0),
 stok int(4)
);
```
![Screenshot (193)](https://user-images.githubusercontent.com/73052649/170809062-91a2fabe-8d1a-44cf-b815-2be077bc6f81.png)

# Menambah Data
```
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```
![Screenshot (194)](https://user-images.githubusercontent.com/73052649/170809081-a2aa7e3f-a2e7-448e-9908-b3fc9dd8f8d9.png)

# B. Membuat Program CRUD
# Membuat file koneksi Database
```
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
 echo "Koneksi ke server gagal.";
 die();
} #else echo "Koneksi berhasil";
?>
```
![Screenshot (195)](https://user-images.githubusercontent.com/73052649/170809099-d025fb6d-c3c3-4898-8da2-497b5adc1497.png)

# Membuat file index untuk menampilkan data (Read)
Simpan file dengan nama index.php
```
<?php
include("koneksi.php");
// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <link href="style.css" rel="stylesheet" type="text/css" />
  <title>Data Barang</title>
</head>

<body>
  <div class="container">
    <h1>Data Barang</h1>
    <div class="main">
      <table>
        <tr>
          <th>Gambar</th>
          <th>Nama Barang</th>
          <th>Katagori</th>
          <th>Harga Jual</th>
          <th>Harga Beli</th>
          <th>Stok</th>
          <th>Aksi</th>
        </tr>
        <?php if ($result) : ?>
          <?php while ($row = mysqli_fetch_array($result)) : ?>
            <tr>
              <td><img src="gambar/<?= $row['gambar']; ?>" alt="<?=
                                                                $row['nama']; ?>"></td>
              <td><?= $row['nama']; ?></td>
              <td><?= $row['kategori']; ?></td>
              <td><?= $row['harga_beli']; ?></td>
              <td><?= $row['harga_jual']; ?></td>
              <td><?= $row['stok']; ?></td>
              <td><?= $row['id_barang']; ?></td>
            </tr>
          <?php endwhile;
        else : ?>
          <tr>
            <td colspan="7">Belum ada data</td>
          </tr>
        <?php endif; ?>
      </table>
    </div>
  </div>
</body>

</html>
``` 
![Screenshot (196)](https://user-images.githubusercontent.com/73052649/170809110-1ee9b642-7b08-4057-9a8e-1ddfa35dcf46.png)

# Menambah Data (Create)
Buat file dengan tambah.php
```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit'])) {
  $nama = $_POST['nama'];
  $kategori = $_POST['kategori'];
  $harga_jual = $_POST['harga_jual'];
  $harga_beli = $_POST['harga_beli'];
  $stok = $_POST['stok'];
  $file_gambar = $_FILES['file_gambar'];
  $gambar = null;
  if ($file_gambar['error'] == 0) {
    $filename = str_replace(' ', '_', $file_gambar['name']);
    $destination = dirname(__FILE__) . '/gambar/' . $filename;
    if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
      $gambar = 'gambar/' . $filename;;
    }
  }
  $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli, 
stok, gambar) ';
  $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}', 
'{$harga_beli}', '{$stok}', '{$gambar}')";
  $result = mysqli_query($conn, $sql);
  header('location: index.php');
}
?>

<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <link href="style.css" rel="stylesheet" type="text/css" />
  <title>Tambah Barang</title>
</head>

<body>
  <div class="container">
    <h1>Tambah Barang</h1>
    <div class="main">
      <form method="post" action="tambah.php" enctype="multipart/form-data">
        <div class="input">
          <label>Nama Barang</label>
          <input type="text" name="nama" />
        </div>
        <div class="input">
          <label>Kategori</label>
          <select name="kategori">
            <option value="Komputer">Komputer</option>
            <option value="Elektronik">Elektronik</option>
            <option value="Hand Phone">Hand Phone</option>
          </select>
        </div>
        <div class="input">
          <label>Harga Jual</label>
          <input type="text" name="harga_jual" />
        </div>
        <div class="input">
          <label>Harga Beli</label>
          <input type="text" name="harga_beli" />
        </div>
        <div class="input">
          <label>Stok</label>
          <input type="text" name="stok" />
        </div>
        <div class="input">
          <label>File Gambar</label>
          <input type="file" name="file_gambar" />
        </div>
        <div class="submit">
          <input type="submit" name="submit" value="Simpan" />
        </div>
      </form>
    </div>
  </div>
</body>

</html>
```
![Screenshot (197)](https://user-images.githubusercontent.com/73052649/170809136-8a22dd15-4e0e-40e9-aba8-796c11eef036.png)

# Mengubah data
```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit'])) {
  $id = $_POST['id'];
  $nama = $_POST['nama'];
  $kategori = $_POST['kategori'];
  $harga_jual = $_POST['harga_jual'];
  $harga_beli = $_POST['harga_beli'];
  $stok = $_POST['stok'];
  $file_gambar = $_FILES['file_gambar'];
  $gambar = null;

  if ($file_gambar['error'] == 0) {
    $filename = str_replace(' ', '_', $file_gambar['name']);
    $destination = dirname(__FILE__) . '/gambar/' . $filename;
    if (move_uploaded_file($file_gambar['tmp_name'], $destination)) {
      $gambar = 'gambar/' . $filename;;
    }
  }
  $sql = 'UPDATE data_barang SET ';
  $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
  $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok 
= '{$stok}' ";
  if (!empty($gambar))
    $sql .= ", gambar = '{$gambar}' ";
  $sql .= "WHERE id_barang = '{$id}'";
  $result = mysqli_query($conn, $sql);
  header('location: index.php');
}
$id = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
if (!$result) die('Error: Data tidak tersedia');
$data = mysqli_fetch_array($result);
function is_select($var, $val)
{
  if ($var == $val) return 'selected="selected"';
  return false;
}
?>
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <link href="style.css" rel="stylesheet" type="text/css" />
  <title>Ubah Barang</title>
</head>

<body>
  <div class="container">
    <h1>Ubah Barang</h1>
    <div class="main">
      <form method="post" action="ubah.php" enctype="multipart/form-data">
        <div class="input">
          <label>Nama Barang</label>
          <input type="text" name="nama" value="<?php echo
                                                $data['nama']; ?>" />
        </div>
        <div class="input">
          <label>Kategori</label>
          <select name="kategori">
            <option <?php echo is_select('Komputer', $data['kategori']); ?> value="Komputer">Komputer</option>
            <option <?php echo is_select('Komputer', $data['kategori']); ?> value="Elektronik">Elektronik</option>
            <option <?php echo is_select('Komputer', $data['kategori']); ?> value="Hand Phone">Hand Phone</option>
          </select>
        </div>
        <div class="input">
          <label>Harga Jual</label>
          <input type="text" name="harga_jual" value="<?php echo
                                                      $data['harga_jual']; ?>" />
        </div>
        <div class="input">
          <label>Harga Beli</label>
          <input type="text" name="harga_beli" value="<?php echo
                                                      $data['harga_beli']; ?>" />
        </div>
        <div class="input">
          <label>Stok</label>
          <input type="text" name="stok" value="<?php echo
                                                $data['stok']; ?>" />
        </div>
        <div class="input">
          <label>File Gambar</label>
          <input type="file" name="file_gambar" />
        </div>
        <div class="submit">
          <input type="hidden" name="id" value="<?php echo
                                                $data['id_barang']; ?>" />
          <input type="submit" name="submit" value="Simpan" />
        </div>
      </form>
    </div>
  </div>
</body>

</html>
```

# Menghapus data
```
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```


































