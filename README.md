# Projek Pemrograman Web 2 To Do List

|Nama|NIM|Kelas|Mata Kuliah|
|----|---|-----|------|
|**Akbar Rizky Ramadhan**|**312310696**|**TI.23.A6**|**Pemrograman Web 2**|
|**Annisa Saidah Mubarokah**|**312310631**|**TI.23.A6**|**Pemrograman Web 2**|
|**Arbi Bagaskara Putra**|**312310671**|**TI.23.A6**|**Pemrograman Web 2**|
|**Bagas Prihantoro Putra**|**312310599**|**TI.23.A6**|**Pemrograman Web 2**|
|**Sekar Ayu Aulia Nazwa**|**312310654**|**TI.23.A6**|**Pemrograman Web 2**|

# Halaman Utama

```css
/* Reset basic styles */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

/* Basic styling for the body */
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    color: #333;
    line-height: 1.6;
}

/* Container for the page content */
.container {
    width: 80%;
    margin: 0 auto;
    background-color: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

/* Styling for the header */
header {
    text-align: center;
    margin-bottom: 20px;
}

header h1 {
    font-size: 2rem;
    margin-bottom: 10px;
    color: #5c6bc0;
}

header .btn {
    display: inline-block;
    background-color: #5c6bc0;
    color: white;
    padding: 10px 20px;
    text-decoration: none;
    border-radius: 5px;
    font-size: 1rem;
}

header .btn:hover {
    background-color: #3f51b5;
}

/* Table styling */
table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

table th,
table td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}

table th {
    background-color: #5c6bc0;
    color: white;
}

table td {
    background-color: #fafafa;
}

table tr:hover {
    background-color: #f1f1f1;
}

/* Button styles for Edit and Delete */
.btn-edit {
    background-color: #ff9800;
}

.btn-edit:hover {
    background-color: #f57c00;
}

.btn-delete {
    background-color: #f44336;
}

.btn-delete:hover {
    background-color: #e53935;
}
```
```php
<?php
include 'db.php';

$sql = "SELECT * FROM tugas";
$result = $conn->query($sql);
?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daftar Tugas</title>
    <link rel="stylesheet" href="style.css"> <!-- Link ke file CSS -->
</head>

<body>
    <div class="container">
        <header>
            <h1>Daftar Tugas</h1>
            <a href="tambah.php" class="btn">Tambah Tugas</a>
        </header>

        <table>
            <thead>
                <tr>
                    <th>Tugas</th>
                    <th>Status</th>
                    <th>Action</th>
                </tr>
            </thead>
            <tbody>
                <?php while ($row = $result->fetch_assoc()): ?>
                    <tr>
                        <td><?php echo $row['tugas']; ?></td>
                        <td><?php echo $row['status']; ?></td>
                        <td>
                            <a href="edit.php?id=<?php echo $row['id']; ?>" class="btn btn-edit">Edit</a>
                            <a href="hapus.php?id=<?php echo $row['id']; ?>" class="btn btn-delete">Hapus</a>
                        </td>
                    </tr>
                <?php endwhile; ?>
            </tbody>
        </table>
    </div>
</body>

</html>

<?php $conn->close(); ?>
```
<img width="1919" height="1003" alt="Screenshot 2025-07-22 162728" src="https://github.com/user-attachments/assets/d298d978-4224-492e-b38a-c0285752f3f0" />

# Halaman untuk menambahkan
```php
<?php
include 'db.php';

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $tugas = $_POST['tugas'];
    $status = $_POST['status'];

    $sql = "INSERT INTO tugas (tugas, status) VALUES ('$tugas', '$status')";

    if ($conn->query($sql) === TRUE) {
        header('Location: index.php');
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
}
?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tambah Tugas</title>
</head>

<body>
    <h1>Tambah Tugas</h1>
    <form method="POST">
        <label for="tugas">Tugas:</label>
        <input type="text" name="tugas" id="tugas" required><br>

        <label for="status">Status:</label>
        <select name="status" id="status" required>
            <option value="Belum Selesai">Belum Selesai</option>
            <option value="Selesai">Selesai</option>
        </select><br>

        <button type="submit">Simpan</button>
    </form>
    <a href="index.php">Kembali ke Daftar Tugas</a>
</body>

</html>

<?php $conn->close(); ?>
```
<img width="1919" height="1005" alt="Screenshot 2025-07-22 170056" src="https://github.com/user-attachments/assets/d4dc3443-9ab3-40f0-9ca0-289081e81792" />

# Halaman untuk melakukan aksi edit
```php
<?php
include 'db.php';

if (isset($_GET['id'])) {
    $id = $_GET['id'];

    // Ambil data tugas dari database
    $sql = "SELECT * FROM tugas WHERE id = $id";
    $result = $conn->query($sql);
    $row = $result->fetch_assoc();
}

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $tugas = $_POST['tugas'];
    $status = $_POST['status'];

    $sql = "UPDATE tugas SET tugas = '$tugas', status = '$status' WHERE id = $id";

    if ($conn->query($sql) === TRUE) {
        header('Location: index.php');
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
}
?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Edit Tugas</title>
</head>

<body>
    <h1>Edit Tugas</h1>
    <form method="POST">
        <label for="tugas">Tugas:</label>
        <input type="text" name="tugas" id="tugas" value="<?php echo $row['tugas']; ?>" required><br>

        <label for="status">Status:</label>
        <select name="status" id="status" required>
            <option value="Belum Selesai" <?php echo $row['status'] == 'Belum Selesai' ? 'selected' : ''; ?>>Belum Selesai
            </option>
            <option value="Selesai" <?php echo $row['status'] == 'Selesai' ? 'selected' : ''; ?>>Selesai</option>
        </select><br>

        <button type="submit">Simpan Perubahan</button>
    </form>
    <a href="index.php">Kembali ke Daftar Tugas</a>
</body>

</html>

<?php $conn->close(); ?>
```
<img width="1919" height="1002" alt="image" src="https://github.com/user-attachments/assets/ca3b53bd-4e24-4268-ab03-603e8a70d7ac" />

# Kode untuk melakukan penghapusan
```php
<?php
include 'db.php';

if (isset($_GET['id'])) {
    $id = $_GET['id'];

    $sql = "DELETE FROM tugas WHERE id = $id";

    if ($conn->query($sql) === TRUE) {
        header('Location: index.php');
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
}

$conn->close();
?>
```

# Link Poster: https://www.canva.com/design/DAGun9fS_TE/XtzhvAK-LatEUwAad9lG5Q/edit?utm_content=DAGun9fS_TE&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton
