<template>
  <div class="container mt-2">
    <b-form>
      <b-form-group label="Titulo" label-for="subject">
        <b-form-input id="subject" v-model="form.subject" type="text" 
        placeholder="Ex: Lavar carro"
        required autocomplete="off">
        </b-form-input>
      </b-form-group>
      <b-form-group label="Descrição" label-for="description">
        <b-form-textarea id="description" v-model="form.description" type="text" 
        placeholder="Ex: Preciso levar o carro para lavar"
        required autocomplete="off">
        </b-form-textarea>
      </b-form-group>
      <b-button type="submite" variant="outline-primary" @click="saveTask">Salvar</b-button>
    </b-form>
  </div>
</template>

<script>
import ToastMixin from "@/mixins/ToastMixin.js"
export default {
  name: "Form",
  mixins: [ToastMixin],

  data() {
    return {
      form: {
        subject: "",
        description: ""
      },
      methodSave: "new"
    }
  },
  created() {
    if(this.$route.params.index === 0 || this.$route.params.index != undefined) {
      this.methodSave = "update"
      let tasks = JSON.parse(localStorage.getItem("tasks"))
      this.form = tasks[this.$route.params.index]
    }
  },
  methods: {
    saveTask() {
      // Verificando se é uma tarefa que será atualizada, ou uma tarefa nova
      if(this.methodSave === "update") {
        let tasks = JSON.parse(localStorage.getItem("tasks"))
        tasks[this.$route.params.index] = this.form
        localStorage.setItem("tasks", JSON.stringify(tasks))
        this.showToast("success", "Sucesso!", "Tarefa atualizada.")
        this.$router.push({name: "list"})
        return
      }
      // Testando se existem tarefas no localStorage
      let tasks = (localStorage.getItem("tasks")) ? JSON.parse(localStorage.getItem("tasks")) : []
      // Adicionando task ao array de tarefas
      tasks.push(this.form)
      // Definindo array dentro do localStorage e exibindo mensagem de sucesso
      localStorage.setItem("tasks", JSON.stringify(tasks))
      this.showToast("success", "Sucesso!", "Tarefa criada.")
      // Após salvar task, leva o usuário à lista de tarefas
      this.$router.push({name: "list"})
    }
  }
}
</script>
