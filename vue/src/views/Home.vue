<template>
	<div>
		<div class="container">
			<h1 class="mt-5 mb-5">Data Siswa</h1>

			<form @submit.prevent="saveData" class="mb-5" v-if="!edit">
				<input type="number" class="form-control" placeholder="NIS" v-model="formData.nis">
				<input type="text" class="form-control" placeholder="Nama" v-model="formData.name">
				
				<button type="submit" class="btn btn-primary">Save</button>
			</form>

			<form @submit.prevent="editData" class="mb-5" v-if="edit">
				<input type="number" class="form-control" placeholder="NIS" v-model="formEditData.nis">
				<input type="text" class="form-control" placeholder="Nama" v-model="formEditData.name">
				
				<button type="submit" class="btn btn-primary">Update</button>
				<button type="button" class="btn btn-danger" @click="edit = false">Cancel</button>
			</form>

			<table class="table">
				<thead>
					<tr>
					<th scope="col">ID</th>
					<th scope="col">NIS</th>
					<th scope="col">Name</th>
					<th scope="col">Action</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="student in students" :key="student.id">
						<th scope="row">{{ student.id }}</th>
						<td>{{ student.nis }}</td>
						<td>{{ student.name }}</td>
						<td>
							<button type="button" class="btn btn-warning" @click="edit = true; formEditData = student">Edit</button>
							<button type="button" class="btn btn-danger" @click="deleteStudent(student.id)">Delete</button>
						</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>
</template>

<script>
import axios from 'axios'

export default {
	data() {
		return {
			students: null,
			formData: {
				nis: null,
				name: null
			},
			edit: false,
			formEditData: {}
		}
	},
	methods: {
		getStudents() {
			axios.get(`http://localhost:8000/api/students`)
				.then(res => {
					this.students = res.data;
				})
				.catch(err => {
					alert(err.response.data.message);
				});
		},
		deleteStudent(id) {
			axios.delete(`http://localhost:8000/api/students/${id}`)
				.then(res => {
					this.getStudents();
				})
				.catch(err => {
					alert(err.response.data.message);
				}); 
		},
		saveData() {
			axios.post(`http://localhost:8000/api/students`, this.formData)
				.then(res => {
					this.getStudents();

					this.formData.nis = null;
					this.formData.name = null;
				})
				.catch(err => {
					alert(err.response.data.message);
				}); 
		},
		editData() {
			axios.put(`http://localhost:8000/api/students/${this.formEditData.id}`, this.formEditData)
				.then(res => {
					this.getStudents();

					this.formEditData.nis = null;
					this.formEditData.name = null;


					this.edit = false;
				})
				.catch(err => {
					alert(err.response.data.message);
				}); 
		}
	},
	created() {
		this.getStudents();
	}
}
</script>