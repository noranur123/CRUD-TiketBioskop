<template>
  <div>
    <h1 class="page-title">CRUD Tiket Bioskop</h1>

    <section class="form-section">
      <h2>{{ isEditing ? 'Edit Tiket' : 'Pesan Tiket' }}</h2>
      <form @submit.prevent="saveOrUpdateTicket" class="ticket-form">
        <div class="form-group">
          <label for="namaPembeli">Nama Pembeli:</label>
          <input v-model="formData.namaPembeli" type="text" id="namaPembeli" required>
        </div>

        <div class="form-group">
          <label for="judulFilm">Judul Film:</label>
          <input v-model="formData.judulFilm" type="text" id="judulFilm" required>
        </div>

        <div class="form-group">
          <label for="tanggal">Tanggal:</label>
          <input v-model="formData.tanggal" type="date" id="tanggal" required>
        </div>

        <div class="form-group">
          <label for="jamMulai">Jam Mulai:</label>
          <input v-model="formData.jamMulai" type="time" id="jamMulai" required>
        </div>

        <div class="form-group">
          <label for="jumlahTiket">Jumlah Tiket:</label>
          <input v-model.number="formData.jumlahTiket" type="number" id="jumlahTiket" min="1" required>
        </div>

        <div class="form-group">
          <label for="hargaTiket">Harga (per tiket):</label>
          <input v-model.number="formData.hargaTiket" type="number" id="hargaTiket" min="0" required>
        </div>

        <div class="form-actions">
          <button type="submit">{{ isEditing ? 'Update Data' : 'Tambah Data' }}</button>
          <button type="button" @click="clearForm">{{ isEditing ? 'Batal Edit' : 'Bersihkan Form' }}</button>
        </div>
      </form>
    </section>

    <section class="table-section">
      <h2>Daftar Tiket</h2>
      <table>
        <thead>
          <tr>
            <th>Nama Pembeli</th>
            <th>Judul Film</th>
            <th>Tanggal</th>
            <th>Jam Mulai</th>
            <th>Jumlah Tiket</th>
            <th>Harga (per tiket)</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="ticket in tickets" :key="ticket.id">
            <td>{{ ticket.namaPembeli }}</td>
            <td>{{ ticket.judulFilm }}</td>
            <td>{{ ticket.tanggal }}</td>
            <td>{{ ticket.jamMulai }}</td>
            <td>{{ ticket.jumlahTiket }}</td>
            <td>{{ ticket.hargaTiket }}</td>
            <td>
              <button @click="fillFormModal(ticket.id)" class="edit">Edit</button>
              <button @click="deleteTicket(ticket.id)" class="delete">Hapus</button>
            </td>
          </tr>
        </tbody>
      </table>
    </section>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tickets: [],
      formData: {
        namaPembeli: '',
        judulFilm: '',
        tanggal: '',
        jamMulai: '',
        jumlahTiket: 0,
        hargaTiket: 0,
      },
      isEditing: false,
      editingTicketId: null,
    };
  },
  methods: {
    saveOrUpdateTicket() {
      if (this.isEditing) {
        // Update existing ticket
        const updatedTicket = { ...this.formData, id: this.editingTicketId };
        const existingTicketIndex = this.tickets.findIndex((ticket) => ticket.id === this.editingTicketId);
        this.tickets[existingTicketIndex] = updatedTicket;
        this.isEditing = false;
        this.editingTicketId = null;
      } else {
        // Save new ticket
        this.tickets.push({ ...this.formData, id: this.generateId() });
      }

      this.clearForm();
    },
    clearForm() {
      this.formData = {
        namaPembeli: '',
        judulFilm: '',
        tanggal: '',
        jamMulai: '',
        jumlahTiket: 0,
        hargaTiket: 0,
      };
    },
    deleteTicket(ticketId) {
      this.tickets = this.tickets.filter((ticket) => ticket.id !== ticketId);
    },
    fillFormModal(ticketId) {
      const ticket = this.tickets.find((ticket) => ticket.id === ticketId);
      if (ticket) {
        this.formData = { ...ticket };
        this.isEditing = true;
        this.editingTicketId = ticketId;
      }
    },
    generateId() {
      return '_' + Math.random().toString(36).substr(2, 9);
    },
  },
};
</script>

<style scoped>
.page-title {
  background-color: #3498db;
  color: white;
  padding: 15px;
  border-radius: 8px;
  display: block; /* Change display to block */
  width: 100%;    /* Set width to 100% */
  box-sizing: border-box; /* Include padding and border in the width calculation */
}

.form-section,
.table-section {
  margin-top: 20px;
}

.ticket-form {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 10px;
}

.form-group {
  margin-bottom: 15px;
}

.form-group label {
  display: block;
  margin-bottom: 5px;
}

.form-group input {
  width: 100%;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-sizing: border-box;
}

.form-actions {
  grid-column: span 2;
  text-align: right;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 20px;
}

th,
td {
  border: 2px solid #ddd;
  padding: 12px;
  text-align: left;
}

th {
  background-color: #b94ee1;
  color: white;
}

button {
  padding: 8px 12px;
  cursor: pointer;
  border: none;
  border-radius: 5px;
  background-color: #ff1db4;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #0c84c0;
}

button.edit {
  background-color: #0079c1;
  color: white;
}

button.edit:hover {
  background-color: #005f9e;
}

button.delete {
  background-color: #d9534f;
  color: white;
}
</style>