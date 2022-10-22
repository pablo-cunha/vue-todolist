<template>
  <div class="container mt-2">
    <div v-if="this.tasks.length < 1">
      <b-card title="Sem tarefas no momento" class="card mb-2">
        <b-card-text>Clique no botão abaixo para criar a primeira tarefa.</b-card-text>
        <a href="/form">
          <b-button variant="info">Criar Tarefa</b-button>
        </a>
      </b-card>
    </div>
    <div v-for="(task, index) in tasks" :key="index">
      <b-card :title="task.subject" class="card mb-2">
        <b-card-text>{{ task.description }}</b-card-text>
        <b-button class="mr-2" variant="info" @click="editTask(index)">Editar</b-button>
        <b-button class="mr-2" variant="danger" @click="deleteTask(task, index)">Excluir</b-button>
      </b-card>
    </div>
    <div>
      <b-modal class="modal" ref="modalDelete" hide-footer title="Deletar Tarefa">
        <div class="d-block text-center">
          Deseja deletar a tarefa "{{ taskSelected.subject }}"?
        </div>
        <div class="mt-3 d-flex justify-content-end">
          <b-button class="mr-2" variant="info" @click="hideModal">Cancelar</b-button>
          <b-button class="mr-2" variant="danger" @click="confirmDelete">Excluir</b-button>
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
</style>