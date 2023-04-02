<template>
  <Transition name="editModal">
    <div v-if="editShow" class="modal-mask">
      <div class="modal-container">
        <div class="modal-header">
          <slot name="header">default header</slot>
        </div>
        <form @submit.prevent>
          <div class="modal-body">
            <slot name="body">
              <div class="panel panel-default">
                <input
                  v-model="editedToDo.toDoDescription"
                  placeholder="Description"
                  id="descriptionPanel"
                />
              </div>
              <div><label>Deadline:</label></div>
              <div>
                <input
                  type="date"
                  class="form-c"
                  v-model="editedToDo.deadline"
                  id="datePanel"
                />
              </div>
              <div><h5>Priority:</h5></div>
              <div>
                <input
                  type="radio"
                  id="low"
                  value="Low"
                  v-model="editedToDo.priority"
                />
                <label for="low">Low</label>
                <input
                  type="radio"
                  id="med"
                  value="Medium"
                  v-model="editedToDo.priority"
                />
                <label for="med">Medium</label>
                <input
                  type="radio"
                  id="high"
                  value="High"
                  v-model="editedToDo.priority"
                />
                <label for="high">High</label>
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
                <i class="fa-solid fa-ban"></i>Cancel</button
              ><span></span>
              <button
                id="editButton"
                class="btn btn-primary"
                @click="finishEditing()"
              >
                <i class="fa-solid fa-pen-to-square"></i>Edit
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
    todos: {
      type: Object,
      default: () => ({}),
    },
    editShow: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      editedToDo: {
        ...this.todos,
        priority: 'Medium',
      },
    };
  },
  methods: {
    finishEditing() {
      const newDescription = this.editedToDo.toDoDescription;
      const newDeadline = this.editedToDo.deadline;

      // Remove previous error messages
      const errorElements = document.querySelectorAll('.error-message');
      errorElements.forEach((element) => element.remove());

      if (!newDescription || !newDeadline) {
        if (!newDescription) {
          const descriptionPanel = document.getElementById('descriptionPanel');
          descriptionPanel.style.border = '1px solid red';
          const descriptionError = document.createElement('div');
          descriptionError.style.color = 'red';
          descriptionError.innerHTML = 'Description is required';
          descriptionError.classList.add('error-message'); // add class to new error message
          descriptionPanel.parentNode.insertBefore(
            descriptionError,
            descriptionPanel.nextSibling
          );
        } else {
          const descriptionPanel = document.getElementById('descriptionPanel');
          descriptionPanel.style.border = '';
        }
        if (!newDeadline) {
          const datePanel = document.getElementById('datePanel');
          datePanel.style.border = '1px solid red';
          const dateError = document.createElement('div');
          dateError.style.color = 'red';
          dateError.innerHTML = 'Deadline is required';
          dateError.classList.add('error-message'); // add class to new error message
          datePanel.parentNode.insertBefore(dateError, datePanel.nextSibling);
        } else {
          const datePanel = document.getElementById('datePanel');
          datePanel.style.border = '';
        }
        return;
      }
      this.$emit('edit-todo', this.todos, this.editedToDo);
      this.$emit('close');
    },
  },
};
</script>

<style>
#editButton {
  background-color: #5398ed;
  color: white;
  font-size: 19px;
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
  margin: 0 10px 0 10px;
  margin-right: 7px;
}
</style>
