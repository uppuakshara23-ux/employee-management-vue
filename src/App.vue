<template>
  <div class="container mt-5">
    <h2 class="text-center mb-4">Employee Management System</h2>

    <!-- Form -->
    <div class="card mb-4">
      <div class="card-body">
        <h5>{{ isEdit ? "Update Employee" : "Add Employee" }}</h5>

        <div class="row">
          <div class="col-md-6 mb-2">
            <input v-model="employee.name" class="form-control" placeholder="Name" />
          </div>
          <div class="col-md-6 mb-2">
            <input v-model="employee.designation" class="form-control" placeholder="Designation" />
          </div>
          <div class="col-md-6 mb-2">
            <input v-model="employee.department" class="form-control" placeholder="Department" />
          </div>
          <div class="col-md-6 mb-2">
            <input v-model="employee.salary" type="number" class="form-control" placeholder="Salary" />
          </div>
        </div>

        <button class="btn btn-primary mt-2" @click="saveEmployee">
          {{ isEdit ? "Update" : "Add" }}
        </button>
        <button v-if="isEdit" class="btn btn-secondary mt-2 ms-2" @click="resetForm">
          Cancel
        </button>
      </div>
    </div>

    <!-- Table -->
    <table class="table table-bordered table-striped">
      <thead class="table-dark">
        <tr>
          <th>ID</th>
          <th>Name</th>
          <th>Designation</th>
          <th>Department</th>
          <th>Salary</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="emp in employees" :key="emp.id">
          <td>{{ emp.id }}</td>
          <td>{{ emp.name }}</td>
          <td>{{ emp.designation }}</td>
          <td>{{ emp.department }}</td>
          <td>{{ emp.salary }}</td>
          <td>
            <button class="btn btn-warning btn-sm me-2" @click="editEmployee(emp)">Edit</button>
            <button class="btn btn-danger btn-sm" @click="deleteEmployee(emp.id)">Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      employees: [],
      employee: {
        name: "",
        designation: "",
        department: "",
        salary: ""
      },
      isEdit: false,
      editId: null,
      apiUrl: "https://abcd1234.mockapi.io/employees"
    };
  },

  methods: {
    fetchEmployees() {
      axios.get(this.apiUrl).then(res => {
        this.employees = res.data;
      });
    },

    saveEmployee() {
      if (this.isEdit) {
        axios.put(`${this.apiUrl}/${this.editId}`, this.employee).then(() => {
          this.fetchEmployees();
          this.resetForm();
        });
      } else {
        axios.post(this.apiUrl, this.employee).then(() => {
          this.fetchEmployees();
          this.resetForm();
        });
      }
    },

    editEmployee(emp) {
      this.employee = { ...emp };
      this.editId = emp.id;
      this.isEdit = true;
    },

    deleteEmployee(id) {
      if (confirm("Are you sure?")) {
        axios.delete(`${this.apiUrl}/${id}`).then(() => {
          this.fetchEmployees();
        });
      }
    },

    resetForm() {
      this.employee = {
        name: "",
        designation: "",
        department: "",
        salary: ""
      };
      this.isEdit = false;
      this.editId = null;
    }
  },

  mounted() {
    this.fetchEmployees();
  }
};
</script>

<style>
body {
  background-color: #f8f9fa;
}
</style>