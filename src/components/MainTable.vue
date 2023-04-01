<template>
  <!--This is the beginning of the basepage(banner & empty table)-->
  <div class="container">
    <div class="table-wrapper">
      <div class="table-title" id="tableTitle">
        <h4 id="checklistTitle"><i class="fa-solid fa-list"></i> Checklist</h4>
        <button
          id="openModalButton"
          class="btn btn-primary"
          @click="showModal = true"
        >
          <i class="fa-solid fa-circle-plus"></i> Add
        </button>
      </div>
      <table id="dynamicTable" class="table table-striped table-hover">
        <thead>
          <tr>
            <th>Title</th>
            <th>Description</th>
            <th>Deadline</th>
            <th>Priority</th>
            <th>Is Complete</th>
            <th>Action</th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="(todos, index) in todos"
            :key="index"
            :
            @add-todo="addTodo"
            :
            @edit-todo="editTodo(todos, $event)"
          >
            <td>{{ todos.toDoTitle }}</td>
            <td>{{ todos.toDoDescription }}</td>
            <td>{{ todos.deadline }}</td>
            <td>{{ todos.priority }}</td>
            <td>
              <input
                type="checkbox"
                class="checkbox"
                @click="toggleTodo(todos)"
              />
            </td>
            <td>
              <div>
                <button
                  id="updateButton"
                  type="button"
                  @click="
                    showEditModal = true;
                    todoToUpdate = todos;
                  "
                  class="btn btn-primary btn-sm"
                >
                  <i class="fa-solid fa-pen-to-square"></i>Update
                </button>
              </div>
              <div>
                <button
                  id="deleteButton"
                  type="button"
                  class="btn btn-primary btn-sm"
                  @click="deleteToDo(todos)"
                >
                  <i class="fa-solid fa-circle-xmark"></i>Delete
                </button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <!-- adding modal-->
    <modal :show="showModal" @close="showModal = false" : @add-todo="addTodo">
      <template #header>
        <div id="modalTitle">
          <h3><i class="fa-solid fa-circle-plus"></i> Add Task</h3>
        </div>
      </template>
    </modal>
    <!-- editting modal-->
    <editModal
      :editShow="showEditModal"
      @close="showEditModal = false"
      :todos="todoToUpdate"
      @edit-todo="editTodo"
    >
      <template #header>
        <h3><i class="fa-solid fa-pen-to-square"></i> Edit Task</h3>
      </template>
    </editModal>
  </div>
</template>

<script>
import Modal from './Modal.vue';
import EditModal from './EditModal.vue';
import toastr from 'toastr';
import 'toastr/build/toastr.css';
export default {
  components: {
    Modal,
    EditModal,
  },
  data() {
    return {
      showModal: false,
      showEditModal: false,
      todos: [],
      todoToUpdate: null,
    };
  },
  methods: {
    deleteToDo(deletedTodo) {
      this.todos = this.todos.filter(
        (todos) => todos.toDoTitle !== deletedTodo.toDoTitle
      );
      toastr.options = {
        closeButton: false,
        debug: false,
        newestOnTop: false,
        progressBar: false,
        positionClass: 'toast-bottom-right',
        preventDuplicates: true,
        onclick: null,
        showDuration: '300',
        hideDuration: '1000',
        timeOut: '5000',
        extendedTimeOut: '1000',
        showEasing: 'swing',
        hideEasing: 'linear',
        showMethod: 'fadeIn',
        hideMethod: 'fadeOut',
      };
      toastr.success('Task deleted successfully!');
    },
    addTodo(newTodo) {
      const todoExists = this.todos.some(
        (todo) => todo.toDoTitle === newTodo.toDoTitle
      );
      if (todoExists) {
        toastr.error('A task with the same title already exists!');
        return;
      } else {
        // Add the new todo to the list
        this.todos.push(newTodo);
        this.showModal = false;
        toastr.success('Task added successfully!');
      }
    },
    toggleTodo(completedTodo) {
      completedTodo.completed = !completedTodo.completed;
      var updateButton = document.getElementById('updateButton');
      if (completedTodo.completed) {
        updateButton.style.display = 'none';
      } else {
        updateButton.style.display = 'block';
      }
    },
    editTodo(oldTodo, updatedTodo) {
      const index = this.todos.findIndex((todos) => todos === oldTodo);
      const editedTodo = {
        toDoTitle: oldTodo.toDoTitle, // Keep the original title
        toDoDescription: updatedTodo.toDoDescription,
        deadline: updatedTodo.deadline,
        priority: updatedTodo.priority,
        completed: updatedTodo.completed,
      };
      this.todos.splice(index, 1, editedTodo);
      toastr.success('Task updated successfully!');
    },
  },
};
</script>

<style>
td {
  text-align: center;
}
#checklistTitle {
  position: relative;
  top: 20px;
}
#deleteButton {
  background-color: #e3160b;
  margin: auto;
}
#updateButton {
  background-color: #5398ed;
  color: white;
  margin: auto;
}
#openModalButton {
  position: relative;
  left: 45%;
  border-radius: 4px;
  font-size: 20px;
  color: white;
}
#dynamicTable {
  width: 100%;
}
#tableTitle {
  text-align: center;
}
body {
  color: #566787;
  background: #f5f5f5;
  font-family: 'Varela Round', sans-serif;
  font-size: 13px;
}
.table-wrapper {
  background: #fff;
  padding: 20px 25px;
  margin: 30px 0;
  border-radius: 3px;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
}
.table-title {
  padding-bottom: 15px;
  background: #387fd6;
  color: #fff;
  padding: 16px 30px;
  margin: -20px -25px 10px;
  border-radius: 3px 3px 0 0;
  text-align: center;
}
.table-title h2 {
  margin: 5px 0 0;
  font-size: 24px;
}
.table-title .btn-group {
  float: right;
}
table.table tr th,
table.table tr td {
  border-color: #e9e9e9;
  vertical-align: middle;
}
table.table tr th:first-child {
  width: 60px;
}
table.table tr th:last-child {
  width: 100px;
}
table.table-striped tbody tr:nth-of-type(odd) {
  background-color: #fcfcfc;
}
table.table-striped.table-hover tbody tr:hover {
  background: #f5f5f5;
}
table.table th i {
  font-size: 13px;
  margin: 0 5px;
  cursor: pointer;
}
table.table td:last-child i {
  opacity: 0.9;
  font-size: 22px;
  margin: 0 5px;
}
table.table td a {
  font-weight: bold;
  color: #566787;
  display: inline-block;
  text-decoration: none;
  outline: none !important;
}
table.table td a:hover {
  color: #2196f3;
}
table.table td a.edit {
  color: #ffc107;
}
table.table td a.delete {
  color: #f44336;
}
table.table td i {
  font-size: 19px;
}
table.table .avatar {
  border-radius: 50%;
  vertical-align: middle;
  margin-right: 10px;
}
.pagination {
  float: right;
  margin: 0 0 5px;
}
.pagination li a {
  border: none;
  font-size: 13px;
  min-width: 30px;
  min-height: 30px;
  color: #999;
  margin: 0 2px;
  line-height: 30px;
  border-radius: 2px !important;
  text-align: center;
  padding: 0 6px;
}
.pagination li a:hover {
  color: #666;
}
.pagination li.active a,
.pagination li.active a.page-link {
  background: #03a9f4;
}
.pagination li.active a:hover {
  background: #0397d6;
}
.pagination li.disabled i {
  color: #ccc;
}
.pagination li i {
  font-size: 16px;
  padding-top: 6px;
}
.hint-text {
  float: left;
  margin-top: 10px;
  font-size: 13px;
}
/* Custom checkbox */
.custom-checkbox {
  position: relative;
}
.custom-checkbox input[type='checkbox'] {
  opacity: 0;
  position: absolute;
  margin: 5px 0 0 3px;
  z-index: 9;
}
.custom-checkbox label:before {
  width: 18px;
  height: 18px;
}
.custom-checkbox label:before {
  content: '';
  margin-right: 10px;
  display: inline-block;
  vertical-align: text-top;
  background: white;
  border: 1px solid #bbb;
  border-radius: 2px;
  box-sizing: border-box;
  z-index: 2;
}
.custom-checkbox input[type='checkbox']:checked + label:after {
  content: '';
  position: absolute;
  left: 6px;
  top: 3px;
  width: 6px;
  height: 11px;
  border: solid #000;
  border-width: 0 3px 3px 0;
  transform: inherit;
  z-index: 3;
  transform: rotateZ(45deg);
}
.custom-checkbox input[type='checkbox']:checked + label:before {
  border-color: #03a9f4;
  background: #03a9f4;
}
.custom-checkbox input[type='checkbox']:checked + label:after {
  border-color: #fff;
}
.custom-checkbox input[type='checkbox']:disabled + label:before {
  color: #b8b8b8;
  cursor: auto;
  box-shadow: none;
  background: #ddd;
}
</style>
