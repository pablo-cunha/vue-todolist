<template>
  <div class="container mt-2">
    <div v-for="(task, index) in tasks" :key="index">
      <b-card :title="task.subject" class="card mb-2">
        <b-card-text>{{ task.description }}</b-card-text>
        <b-button class="edit-button mr-2" @click="editTask(index)">Editar</b-button>
        <b-button class="delete-button mr-2" @click="deleteTask(task, index)">Excluir</b-button>
      </b-card>
    </div>
    <div>
      <b-modal class="modal" ref="modalDelete" hide-footer title="Deletar Tarefa">
        <div class="d-block text-center">
          Deseja deletar a tarefa "{{ taskSelected.subject }}"?
        </div>
        <div class="mt-3 d-flex justify-content-end">
          <b-button class="cancel-button mr-2" @click="hideModal">Cancelar</b-button>
          <b-button class="delete-button mr-2" @click="confirmDelete">Excluir</b-button>
        </div>
      </b-modal>
    </div>
  </div>
</template>

<script>
export default {
  name: "List",
  data() {
    return {
      tasks: [],
      taskSelected: []
    }
  },
  created() {
    this.tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : []
  },
  methods: {
    editTask(index) {
      // Redireciona ao formulário, passando o index da tarefa como parametro
      this.$router.push({ name: "form", params: { index }})
    },
    deleteTask(task, index) {
      this.taskSelected = task
      this.taskSelected.index = index
      this.$refs.modalDelete.show()
    },
    hideModal() {
      this.$refs.modalDelete.hide()
    },
    confirmDelete() {
      this.tasks.splice(this.taskSelected.index, 1)
      localStorage.setItem("tasks", JSON.stringify(this.tasks))
      this.hideModal()
    }
  }
}
</script>

<style scoped>
.card {
  color: #ffffff;
  background: linear-gradient(to right, #5c258d, #4389a2);
}
.card .edit-button {
  background-color:#0437F2;
}
.card .edit-button:hover {
  background-color: #191970;
}
.card .delete-button {
  background-color: #e8000d;
}
.card .delete-button:hover {
  background-color: #cf1020;
}
.modal .cancel-button {
  background-color:#0437F2;
}
.modal .cancel-button:hover {
  background-color: #191970;
}
.modal .delete-button {
  background-color: #e8000d;
}
.modal .delete-button:hover {
  background-color: #cf1020;
}
</style>