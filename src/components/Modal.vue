<template>
  <Transition name="modal">
    <div v-if="show" class="modal-mask">
      <div class="modal-container">
        <div class="modal-header">
          <slot name="header">default header</slot>
        </div>
        <form id="modalForm" @submit.prevent>
          <div class="modal-body">
            <slot name="body">
              <div class="panel panel-default">
                <input
                  v-model="modalToDoTitle"
                  placeholder="Title"
                  id="titlePanel"
                />
              </div>
              <div class="panel panel-default">
                <input
                  v-model="modalToDoDescription"
                  placeholder="Description"
                  id="descriptionPanel"
                />
              </div>
              <div><label>Deadline:</label></div>
              <div>
                <input
                  type="date"
                  class="form-c"
                  v-model="modalDeadline"
                  id="datePanel"
                />
              </div>
              <div><h4>Priority:</h4></div>
              <div>
                <div id="radioPanel">
                  <input
                    type="radio"
                    id="low"
                    value="Low"
                    v-model="modalPriority"
                  />
                  <label for="low">Low</label>
                  <input
                    type="radio"
                    id="med"
                    value="Medium"
                    v-model="modalPriority"
                  />
                  <label for="med">Medium</label>
                  <input
                    type="radio"
                    id="high"
                    value="High"
                    v-model="modalPriority"
                  />
                  <label for="high">High</label>
                </div>
              </div>
            </slot>
          </div>

          <div class="modal-footer">
            <slot name="footer">
              <button
                id="cancelButton"
                class="btn btn-danger"
                @click="$emit('close')"
              >
                <i class="fa-solid fa-ban"></i> Cancel</button
              ><span></span>
              <button
                id="addButton"
                @click="addToDoModal()"
                class="btn btn-primary"
              >
                <i class="fa-solid fa-circle-plus"></i> Add
              </button>
            </slot>
          </div>
        </form>
      </div>
    </div>
  </Transition>
</template>

<script>
import toastr from 'toastr';
import 'toastr/build/toastr.css';
export default {
  props: {
    show: Boolean,
    addTodo: Function,
    todos: Array,
  },
  data() {
    return {
      modalPriority: 'Medium',
      modalToDoTitle: '',
      modalToDoDescription: '',
      modalDeadline: '',
    };
  },
  methods: {
    addToDoModal() {
      const newTodo = {
        toDoTitle: this.modalToDoTitle,
        toDoDescription: this.modalToDoDescription,
        deadline: this.modalDeadline,
        priority: this.modalPriority,
        completed: false,
      };
      const newTitle = this.modalToDoTitle;
      const newDescription = this.modalToDoDescription;
      const newDeadline = this.modalDeadline;
      if (!newTitle.trim() || !newDescription.trim() || !newDeadline) {
        if (!newTitle.trim()) {
          const titlePanel = document.getElementById('titlePanel');
          titlePanel.style.border = '1px solid red';
          const titleError = document.createElement('div');
          titleError.style.color = 'red';
          titleError.innerHTML = 'Title is required';
          titlePanel.parentNode.insertBefore(
            titleError,
            titlePanel.nextSibling
          );
        }
        if (!newDescription.trim()) {
          const descriptionPanel = document.getElementById('descriptionPanel');
          descriptionPanel.style.border = '1px solid red';
          const descriptionError = document.createElement('div');
          descriptionError.style.color = 'red';
          descriptionError.innerHTML = 'Description is required';
          descriptionPanel.parentNode.insertBefore(
            descriptionError,
            descriptionPanel.nextSibling
          );
        }
        if (!newDeadline) {
          const datePanel = document.getElementById('datePanel');
          datePanel.style.border = '1px solid red';
          const dateError = document.createElement('div');
          dateError.style.color = 'red';
          dateError.innerHTML = 'Deadline is required';
          datePanel.parentNode.insertBefore(dateError, datePanel.nextSibling);
        }
        return;
      }
      // Add new todo
      this.$emit('add-todo', newTodo);
      this.modalToDoTitle = '';
      this.modalToDoDescription = '';
      this.modalDeadline = '';
      this.modalPriority = 'Medium';
    },
  },
};
</script>

<style>
#cancelButton {
  margin: auto;
}
#addButton {
  margin: auto;
}
#descriptionPanel {
  line-height: 40px;
  height: 60px;
  width: 100%;
  padding: 10px 10px;
  margin: 8px 0;
  box-sizing: border-box;
}
#titlePanel {
  line-height: 70px;
  height: 60px;
  width: 100%;
  padding: 10px 10px;
  margin: 8px 0;
  box-sizing: border-box;
}
#datePanel {
  line-height: 70px;
  height: 60px;
  width: 100%;
  padding: 10px 10px;
  margin: 8px 0;
  box-sizing: border-box;
}
input[type='radio'] {
}
.modal-mask {
  position: fixed;
  z-index: 9998;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  transition: opacity 0.3s ease;
}

.modal-container {
  width: 300px;
  margin: auto;
  padding: 20px 30px;
  background-color: #fff;
  border-color: #387fd6;
  border-radius: 5px;
  border-style: solid;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.33);
  transition: all 0.3s ease;
}

.modal-header h3 {
  margin-top: 0;
  color: #387fd6;
}

.modal-body {
  margin: 20px 0;
}

.modal-default-button {
  float: right;
}

.modal-enter-from {
  opacity: 0;
}

.modal-leave-to {
  opacity: 0;
}

.modal-enter-from .modal-container,
.modal-leave-to .modal-container {
  -webkit-transform: scale(1.1);
  transform: scale(1.1);
}
</style>
