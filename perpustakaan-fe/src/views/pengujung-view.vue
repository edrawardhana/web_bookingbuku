<template>
  <div class="container">
    <h1 class="title">Data Pengunjung</h1>

    <!-- Form Tambah/Edit Pengunjung -->
    <div class="form-container card">
      <form @submit.prevent="handleSubmit" class="form">
        <div class="form-group">
          <label for="name" class="form-label">Nama:</label>
          <input
            type="text"
            v-model="form.name"
            id="name"
            required
            class="form-input"
          />
        </div>
        <div class="form-group">
          <label for="email" class="form-label">Email:</label>
          <input
            type="email"
            v-model="form.email"
            id="email"
            required
            class="form-input"
          />
        </div>
        <div class="form-group">
          <label for="phone_number" class="form-label">No. Telepon:</label>
          <input
            type="text"
            v-model="form.phone_number"
            id="phone_number"
            class="form-input"
          />
        </div>
        <div class="form-group">
          <label for="address" class="form-label">Alamat:</label>
          <textarea
            v-model="form.address"
            id="address"
            class="form-textarea"
          ></textarea>
        </div>
        <div class="button-group">
          <button type="submit" class="button submit-button">
            {{ isEdit ? "Update" : "Tambah" }}
          </button>
          <button v-if="isEdit" @click="resetForm" class="button cancel-button">
            Batal
          </button>
        </div>
      </form>
    </div>

    <!-- Tabel Data Pengunjung -->
    <div class="table-container card">
      <table class="table">
        <thead class="table-header-group">
          <tr class="table-header-row">
            <th class="table-header">Nama</th>
            <th class="table-header">Email</th>
            <th class="table-header">No. Telepon</th>
            <th class="table-header">Alamat</th>
            <th class="table-header">Aksi</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="pengunjung in pengunjungs"
            :key="pengunjung.id"
            class="table-row"
          >
            <td class="table-cell">{{ pengunjung.name }}</td>
            <td class="table-cell">{{ pengunjung.email }}</td>
            <td class="table-cell">{{ pengunjung.phone_number }}</td>
            <td class="table-cell">{{ pengunjung.address }}</td>
            <td class="table-cell action-buttons">
              <button
                @click="editPengunjung(pengunjung)"
                class="button edit-button"
              >
                Edit
              </button>
              <button
                @click="deletePengunjung(pengunjung.id)"
                class="button delete-button"
              >
                Hapus
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      pengunjungs: [],
      form: {
        id: null,
        name: "",
        email: "",
        phone_number: "",
        address: "",
      },
      isEdit: false,
    };
  },
  methods: {
    fetchPengunjungs() {
      axios
        .get("http://127.0.0.1:8000/api/pengunjung")
        .then((response) => {
          this.pengunjungs = response.data;
        })
        .catch((error) => {
          console.error("Error fetching pengunjungs:", error);
        });
    },
    handleSubmit() {
      if (this.isEdit) {
        axios
          .put(
            `http://127.0.0.1:8000/api/pengunjung/${this.form.id}`,
            this.form
          )
          .then(() => {
            this.fetchPengunjungs();
            this.resetForm();
          })
          .catch((error) => {
            console.error("Error updating pengunjung:", error);
          });
      } else {
        axios
          .post("http://127.0.0.1:8000/api/pengunjung", this.form)
          .then(() => {
            this.fetchPengunjungs();
            this.resetForm();
          })
          .catch((error) => {
            console.error("Error adding pengunjung:", error);
          });
      }
    },
    editPengunjung(pengunjung) {
      this.form = { ...pengunjung };
      this.isEdit = true;
    },
    deletePengunjung(id) {
      axios
        .delete(`http://127.0.0.1:8000/api/pengunjung/${id}`)
        .then(() => {
          this.fetchPengunjungs();
        })
        .catch((error) => {
          console.error("Error deleting pengunjung:", error);
        });
    },
    resetForm() {
      this.form = {
        id: null,
        name: "",
        email: "",
        phone_number: "",
        address: "",
      };
      this.isEdit = false;
    },
  },
  mounted() {
    this.fetchPengunjungs();
  },
};
</script>

<style scoped>
.container {
  width: 90%;
  max-width: 1200px;
  margin: 20px auto;
  font-family: "Arial", sans-serif;
}

.title {
  text-align: center;
  margin-bottom: 30px;
  color: #333;
  font-size: 2rem;
  font-weight: 600;
}

.card {
  background-color: #fff;
  padding: 25px;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.08);
  margin-bottom: 20px;
}

/* Form Styles */
.form-container {
}

.form {
  display: flex;
  flex-direction: column;
}

.form-group {
  margin-bottom: 20px;
}

.form-label {
  display: block;
  margin-bottom: 8px;
  font-weight: 500;
  color: #555;
}

.form-input,
.form-textarea {
  width: 100%;
  padding: 12px;
  border: 1px solid #ddd;
  border-radius: 5px;
  box-sizing: border-box;
  font-size: 1rem;
}

.form-textarea {
  resize: vertical;
  min-height: 100px;
}

.button-group {
  display: flex;
  gap: 10px;
  justify-content: flex-start;
}

.button {
  padding: 12px 18px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 1rem;
  transition: background-color 0.3s ease, transform 0.1s ease;
}

.submit-button {
  background-color: #4caf50;
  color: white;
}

.submit-button:hover {
  background-color: #45a049;
  transform: scale(1.02);
}

.cancel-button {
  background-color: #f44336;
  color: white;
}

.cancel-button:hover {
  background-color: #d32f2f;
  transform: scale(1.02);
}

/* Table Styles */
.table-container {
  overflow-x: auto;
}

.table {
  width: 100%;
  border-collapse: collapse;
  background-color: #fff;
  border: 1px solid #eee;
}

.table-header-group {
  background-color: #f8f8f8;
}

.table-header,
.table-cell {
  border: 1px solid #eee;
  padding: 15px;
  text-align: left;
  font-size: 0.95rem;
}

.table-header {
  font-weight: 600;
  color: #333;
}

.table-row:nth-child(even) {
  background-color: #f9f9f9;
}

.action-buttons {
  display: flex;
  gap: 5px;
}

.edit-button {
  background-color: #ffc107;
  color: #333;
}

.edit-button:hover {
  background-color: #e0b300;
  color: #333;
}

.delete-button {
  background-color: #f44336;
  color: white;
}

.delete-button:hover {
  background-color: #d32f2f;
}

/* Responsive adjustments */
@media (max-width: 768px) {
  .container {
    width: 95%;
  }
  .title {
    font-size: 1.75rem;
    margin-bottom: 20px;
  }
  .form-group {
    margin-bottom: 15px;
  }
  .button-group {
    flex-direction: column;
    gap: 8px;
  }
}
</style>
