# Lab3Web
<p> Nama : Viena Dwi Putri Maulina <br> Nim : 312110469 <br> Kelas: TI.21.C1 <br> Mata Kuliah : Pemrograman Web 2 <br> Tugas Pertemuan 4 </p>
<h2> PHP dan Database MySQL <h2>

<p> Jalankan MySQL Server dari menu XAMPP Control <p>

![gambar 1](ss/1.png)

<p>buka http://locallhost/phpmyadmin/<p>

<p> buat database latihan1 <p>

![gambar 2](ss/2.png)

<p> buat tabel <p>

```php
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

![gambar 3](ss/3.png)

<p> menambah data <p>

```php
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, 
stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 
2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

![gambar 4](ss/4.png)

![gambar 5](ss/5.png)
<p> buat folder lab3_php_database pada root directoryweb server (c:\xampp\htdocs)<p>
<p> untuk mengakses directory tersebut buka http://localhost/lab3_php_database/ <p>

![gambar 6](ss/6.png)

<p> Membuat file koneksi database <p>
<p> koneksi.php <p>

```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false) {
    echo "Koneksi ke server gagal.";
    die();
} else echo "Koneksi berhasil";
?>
```

![gambar 7](ss/7.png)

<p> Membuat file index untuk menampilkan data <p>
<p> index.php <p>

```php
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
            <a href="tambah.php"> Tambah data </a>
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
            <?php if($result): ?>
            <?php while($row = mysqli_fetch_array($result)): ?>
            <tr>
                <td><img src="gambar/<?= $row['gambar'];?>" alt="<?=
$row['nama'];?>"></td>
                <td><?= $row['nama'];?></td>
                <td><?= $row['kategori'];?></td>
                <td><?= $row['harga_beli'];?></td>
                <td><?= $row['harga_jual'];?></td>
                <td><?= $row['stok'];?></td>
                <td><a href="ubah.php?id=<?=$row['id_barang'];?>">ubah data</a>
                <a href="hapus.php?id=<?=$row['id_barang'];?>">hapus data</a>
            </tr>
            <?php endwhile; else: ?>
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

![gambar 8](ss/8.png)

<p> Menambah Data <p>
<p> tambah.php <p>

```php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';
if (isset($_POST['submit']))
{
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_',$file_gambar['name']);
        $destination = dirname(__FILE__) .'/gambar/' . $filename;
        if(move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
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
        <form method="post" action="tambah.php" enctype="multipart/form￾data">
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

![gambar 9](ss/9.png)
![gambar 10](ss/10.png)
![gambar 11](ss/11.png)

<p> Mengubah Data <p>
<p> ubah.php <p>

```php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $id = $_POST['id'];
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
    
    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;
        if (move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
        $gambar = 'gambar/' . $filename;;
        }
    }
    $sql = 'UPDATE data_barang SET ';
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', 
stok = '{$stok}' ";
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
function is_select($var, $val) {
 if ($var == $val) return 'selected="selected"';
 return false; }
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
 <form method="post" action="ubah.php" enctype="multipart/formdata">
 <div class="input">
 <label>Nama Barang</label>
 <input type="text" name="nama" value="<?php echo 
$data['nama'];?>" />
</div>
 <div class="input">
 <label>Kategori</label>
 <select name="kategori">
 <option <?php echo is_select
('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
 <option <?php echo is_select
('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
 <option <?php echo is_select
('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
 </select>
 </div>
 <div class="input">
 <label>Harga Jual</label>
 <input type="text" name="harga_jual" value="<?php echo 
$data['harga_jual'];?>" />
 </div>
 <div class="input">
 <label>Harga Beli</label>
 <input type="text" name="harga_beli" value="<?php echo 
$data['harga_beli'];?>" />
 </div>
 <div class="input">
 <label>Stok</label>
 <input type="text" name="stok" value="<?php echo 
$data['stok'];?>" />
 </div>
 <div class="input">
 <label>File Gambar</label>
 <input type="file" name="file_gambar" />
 </div>
 <div class="submit">
 <input type="hidden" name="id" value="<?php echo 
$data['id_barang'];?>" />
 <input type="submit" name="submit" value="Simpan" />
 </div>
 </form>
 </div>
</div>
</body>
</html>
```
<p>localhost/lab3_php_database/ubah.php?id=4 <p>

![gambar 12](ss/12.png)
![gambar 13](ss/13.png)

<p> Mengapus data <p>
<p> hapus.php <p>

```php
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```

![gambar 14](ss/14.png)
