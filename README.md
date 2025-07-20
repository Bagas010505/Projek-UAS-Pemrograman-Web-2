# Projek-UAS-Pemrograman-Web-2

Berikut ini adalah contoh project To-Do List sederhana menggunakan CodeIgniter 4 (CI4) — framework PHP yang populer dan ringan. Proyek ini mencakup fitur dasar: menambahkan, menampilkan, menandai selesai, dan menghapus tugas.

# Struktur Proyek

app/
├── Controllers/
│   └── Todo.php
├── Models/
│   └── TodoModel.php
├── Views/
│   └── todo/
│       ├── index.php
├── Config/
│   └── Routes.php
public/
├── index.php

# 1.Buat Tabel Database
   
CREATE TABLE todos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    task VARCHAR(255) NOT NULL,
    is_done TINYINT(1) DEFAULT 0,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

# 2.Model (app/Models/TodoModel.php)

<?php namespace App\Models;

use CodeIgniter\Model;

class TodoModel extends Model
{
    protected $table = 'todos';
    protected $primaryKey = 'id';
    protected $allowedFields = ['task', 'is_done'];
}

# 3.Controller (app/Controllers/Todo.php)

<?php namespace App\Controllers;

use App\Models\TodoModel;

class Todo extends BaseController
{
    public function index()
    {
        $model = new TodoModel();
        $data['todos'] = $model->findAll();
        return view('todo/index', $data);
    }

    public function add()
    {
        $model = new TodoModel();
        $model->save(['task' => $this->request->getPost('task')]);
        return redirect()->to('/todo');
    }

    public function delete($id)
    {
        $model = new TodoModel();
        $model->delete($id);
        return redirect()->to('/todo');
    }

    public function done($id)
    {
        $model = new TodoModel();
        $model->update($id, ['is_done' => 1]);
        return redirect()->to('/todo');
    }
}

# 4.View (app/Views/todo/index.php)

<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <title>To-Do List - CodeIgniter</title>
</head>
<body>
    <h1>To-Do List</h1>

    <form action="/todo/add" method="post">
        <input type="text" name="task" placeholder="Tugas baru..." required>
        <button type="submit">Tambah</button>
    </form>

    <ul>
        <?php foreach($todos as $todo): ?>
            <li>
                <?= $todo['is_done'] ? '<s>' . esc($todo['task']) . '</s>' : esc($todo['task']) ?>
                <?php if (!$todo['is_done']): ?>
                    <a href="/todo/done/<?= $todo['id'] ?>">[Selesai]</a>
                <?php endif; ?>
                <a href="/todo/delete/<?= $todo['id'] ?>">[Hapus]</a>
            </li>
        <?php endforeach; ?>
    </ul>
</body>
</html>

# 5.Routing (app/Config/Routes.php)

$routes->get('/todo', 'Todo::index');
$routes->post('/todo/add', 'Todo::add');
$routes->get('/todo/delete/(:num)', 'Todo::delete/$1');
$routes->get('/todo/done/(:num)', 'Todo::done/$1');

# Jalankan Proyek

Pastikan koneksi database diatur di .env atau Config/Database.php

Jalankan server lokal dengan:

php spark serve

Akses di browser: http://localhost:8080/todo
