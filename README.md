# Contoh kode sumber

# Controller: Todo.php

class Todo extends CI_Controller {

    public function __construct() {
        parent::__construct();
        $this->load->model('Todo_model');
    }

    public function index() {
        $data['todos'] = $this->Todo_model->get_all();
        $this->load->view('todo/index', $data);
    }

    public function tambah() {
        $this->load->view('todo/tambah');
    }

    public function simpan() {
        $this->Todo_model->insert($this->input->post('judul'));
        redirect('todo');
    }

    public function selesai($id) {
        $this->Todo_model->selesai($id);
        redirect('todo');
    }

    public function hapus($id) {
        $this->Todo_model->hapus($id);
        redirect('todo');
    }
}

# 2. Model: Todo_model.php

class Todo_model extends CI_Model {

    public function get_all() {
        return $this->db->get('todo')->result();
    }

    public function insert($judul) {
        $this->db->insert('todo', ['judul' => $judul, 'status' => 0]);
    }

    public function selesai($id) {
        $this->db->update('todo', ['status' => 1], ['id' => $id]);
    }

    public function hapus($id) {
        $this->db->delete('todo', ['id' => $id]);
    }
}

# 3. View: index.php

<h2>Daftar Tugas</h2>
<a href="<?= site_url('todo/tambah') ?>">Tambah Tugas</a>
<table border="1" cellpadding="10">
<tr>
    <th>Judul</th>
    <th>Status</th>
    <th>Aksi</th>
</tr>
<?php foreach($todos as $t): ?>
<tr>
    <td><?= $t->judul ?></td>
    <td><?= $t->status ? 'Selesai' : 'Belum' ?></td>
    <td>
        <?php if (!$t->status): ?>
            <a href="<?= site_url('todo/selesai/'.$t->id) ?>">Selesai</a> |
        <?php endif; ?>
        <a href="<?= site_url('todo/hapus/'.$t->id) ?>" onclick="return confirm('Hapus tugas ini?')">Hapus</a>
    </td>
</tr>
<?php endforeach; ?>
</table>

# 4. View: tambah.php

<h2>Tambah Tugas Baru</h2>
<form method="post" action="<?= site_url('todo/simpan') ?>">
    <input type="text" name="judul" placeholder="Judul tugas" required>
    <button type="submit">Simpan</button>
</form>

