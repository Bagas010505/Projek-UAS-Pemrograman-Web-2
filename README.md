# Berikut ini adalah contoh sederhana "Project To-Do List" menggunakan CodeIgniter 4 (bisa juga disesuaikan untuk CI3 jika perlu). Proyek ini memungkinkan pengguna menambahkan, menandai selesai, dan menghapus tugas.

# 1.Buat Database dan Tabel

CREATE DATABASE todo_db;

USE todo_db;

CREATE TABLE todos (
    id INT AUTO_INCREMENT PRIMARY KEY,
    task VARCHAR(255) NOT NULL,
    is_done TINYINT(1) DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

# 2.Konfigurasi Database di CodeIgniter

Edit file .env:

database.default.hostname = localhost
database.default.database = todo_db
database.default.username = root
database.default.password =
database.default.DBDriver = MySQLi

# 3.Buat Model:TodoModel.php

<?php

namespace App\Models;
use CodeIgniter\Model;

class TodoModel extends Model
{
    protected $table = 'todos';
    protected $primaryKey = 'id';
    protected $allowedFields = ['task', 'is_done', 'created_at'];
}

# 4.Buat Controller:Todo.php

<?php

namespace App\Controllers;
use App\Models\TodoModel;

class Todo extends BaseController
{
    public function index()
    {
        $model = new TodoModel();
        $data['todos'] = $model->findAll();
        return view('todo_view', $data);
    }

    public function add()
    {
        $model = new TodoModel();
        $model->save([
            'task' => $this->request->getPost('task')
        ]);
        return redirect()->to('/');
    }

    public function done($id)
    {
        $model = new TodoModel();
        $model->update($id, ['is_done' => 1]);
        return redirect()->to('/');
    }

    public function delete($id)
    {
        $model = new TodoModel();
        $model->delete($id);
        return redirect()->to('/');
    }
}

# 5.Buat View:todo_view.php di app/Views

<!DOCTYPE html>
<html>
<head>
    <title>To-Do List</title>
</head>
<body>
    <h1>To-Do List</h1>
    <form method="post" action="/todo/add">
        <input type="text" name="task" placeholder="Tugas baru" required>
        <button type="submit">Tambah</button>
    </form>

    <ul>
        <?php foreach ($todos as $todo): ?>
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
