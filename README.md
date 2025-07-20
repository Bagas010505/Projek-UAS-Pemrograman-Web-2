# 1. Model – TodoModel.php

<?php

namespace App\Models;
use CodeIgniter\Model;

class TodoModel extends Model
{
    protected $table = 'todos';
    protected $primaryKey = 'id';
    protected $allowedFields = ['task', 'is_done'];
}

# 2. Controller - Todo.php

<?php

namespace App\Controllers;
use App\Models\TodoModel;

class Todo extends BaseController
{
    public function add()
    {
        $model = new TodoModel();
        $task = $this->request->getPost('task');

        if (!empty($task)) {
            $model->save([
                'task' => $task,
                'is_done' => 0
            ]);
        }

        return redirect()->to('/');
    }
}

# 3. View – Form Tambah Tugas (todo_view.php)

<form method="post" action="/todo/add">
    <input type="text" name="task" placeholder="Masukkan tugas baru" required>
    <button type="submit">Tambah</button>
</form>
# 4. Routing – app/Config/Routes.php

$routes->post('todo/add', 'Todo::add');
