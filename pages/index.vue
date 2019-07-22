<template>
<div class="container">
  <div class="jumbotron">
    <h1>
      Cliente para API REST de banca
    </h1>
  </div>
  
  <!-- Botón para mostrar formulario de creación de cliente -->
  <b-button @click="buildCreateForm()">Crear cliente</b-button>
  
  <table class="table mt-3">
    <thead>
      <tr>
        <th scope="col">Nombre</th>
        <th scope="col">Apellidos</th>
        <th scope="col">Teléfono</th>
        <th scope="col">Acciones</th>
      </tr>
    </thead>
    
    <tbody>
      <tr v-for="client in clients">
        <td>{{client.givenName}}</td>
        <td>{{client.familyName}}</td>
        <td>{{client.phone}}</td>
        <td>
          <button class="btn btn-primary" @click="buildEditForm(client)">
            Editar 
          </button>
          <button class="btn btn-danger" @click="clientDelete(client)">
            Borrar 
          </button>
        </td>
      </tr>
      
    </tbody>
    
  </table>
  
  <!-- Formulario de creación -->
  
  <b-modal id="modal-1" 
           v-model="modalShow"
           ref="modal"
           title="Submit Your Name"
           @hidden="resetModal"
           @ok="handleOk"
           >
    <form ref="form" novalidate @submit.stop.prevent ="handleSubmit">
      <b-form-group
        :state="givenNameState"
        label="Nombre"
        label-for="givenName-input"
        invalid-feedback="El nombre es obligatorio"
        >
        <b-form-input
          id="givenName-input"
          v-model="formData.givenName"
          :state="givenNameState"
          required
          ></b-form-input>
      </b-form-group>
      <b-form-group
        :state="familyNameState"
        label="Apellidos"
        label-for="familyName-input"
        invalid-feedback="Los apellidos son obligatorios"
        >
        <b-form-input
          id="familyName-input"
          v-model="formData.familyName"
          :state="familyNameState"
          required
          ></b-form-input>
      </b-form-group>
      <b-form-group
        :state="phoneState"
        label="Teléfono"
        label-for="phone-input"
        invalid-feedback="El teléfono es obligatorio"
        >
        <b-form-input
          id="phone-input"
          v-model="formData.phone"
          :state="phoneState"
          required
          ></b-form-input>
      </b-form-group>
    </form>
  </b-modal>
  
  
</div>
</template>

<script>

export default {
  data() {
    return {
      // Variable para mostrar / ocultar modal
      modalShow: false,
      // Variables para validación del formulario
      givenNameState: null,
      familyNameState: null,
      phoneState: null,
      // Cliente que se va a actualizar
      clientToEdit: null,
      // Variable que almacena los datos del cliente que se va a crear / actualizar
      // Está enlazada con el formulario
      formData: {
        givenName: "",
        familyName: "",
        phone: ""
      }
    }
  },
  
  // Cargar datos de la API vía AJAX
  async asyncData ({ $axios, context }) {
    let clients = await $axios.$get(`/api/clients`)
    return { clients: clients}
  },
  
  methods: {
    // Construir formulario para creación
    buildCreateForm() {
      // Mostrar modal
      this.modalShow = true;
    },
    // Construir formulario para edición
    buildEditForm(client) {
      // Mostrar modal
      this.modalShow = true;
      // Almacenar cliente a editar
      this.clientToEdit = client;
      // Actualizar datos del formulario para que aparezcan los datos del cliente actual
      this.formData.givenName = client.givenName;
      this.formData.familyName = client.familyName;
      this.formData.phone = client.phone;
    },
    // Validar formulario
    checkFormValidity() {
      const valid = this.$refs.form.checkValidity()
      this.givenNameState = this.formData.givenName ? 'valid' : 'invalid'
      this.familyNameState = this.formData.familyName ? 'valid' : 'invalid'
      this.phoneState = this.formData.phone ? 'valid' : 'invalid'
      return valid
    },
    // Cerrar modal
    resetModal() {
      // Ocultar modal
      this.modalShow = false;
      // Eliminar variable que almacenaba el cliente a editar
      this.clientToEdit = null;
      // Limpiar datos del formulario
      this.formData.givenName = ''
      this.formData.familyName = ''
      this.formData.phone = ''
      // Limpiar variables de validación del formulario
      this.familyNameState = null
      this.givenNameState = null
      this.phoneState = null
    },
    // Pulsar OK en modal
    handleOk(bvModalEvt) {
      // Impedir cierre del modal
      bvModalEvt.preventDefault()
      // Llamar a función de envío
      this.handleSubmit()
    },
    // Procesar envío
    async handleSubmit() {
      // Si el formulario no es válido, salir
      if (!this.checkFormValidity()) {
        return
      }
      // Acción a realizar: puede ser para crear un nuevo elemento o para actualizar uno existente
      if (this.clientToEdit) {
        // Si existe esa variable significa que estamos editando un cliente. En ese caso, llamamos a la función de edición
        await this.clientUpdate(this.clientToEdit)
      } else {
        // Si no existe esa variable significa que estamos creando un cliente. En ese caso, llamamos a la función de creación
        await this.clientCreate();
      }
      // Ocultar el modal
      this.resetModal();
    },
    
    // FUNCIONES AJAX para interactuar con la API
    
    // Función AJAX para crear un cliente
    clientCreate: async function() {
      await this.$axios.$post(`/api/clients/`, this.formData)
      this.clients= await this.$axios.$get(`/api/clients`)
    },
    
    // Función AJAX para actualizar cliente
    clientUpdate: async function(client) {
      var clientData = {
        // La API espera el id también
        id: client.id,
        givenName: this.formData.givenName,
        familyName: this.formData.familyName,
        phone: this.formData.phone
      }
      await this.$axios.$put(`/api/clients/` + client.id , clientData)
      this.clients= await this.$axios.$get(`/api/clients`)
    },
    
    // Función AJAX para borrar cliente
    clientDelete: async function(client) {
      if (confirm("¿Desea borrar el cliente " + client.givenName + " " + client.familyName + "?")) {
        await this.$axios.$delete(`/api/clients/` + client.id)
        this.clients = await this.$axios.$get(`/api/clients`)
      }
    }
  }
}
</script>

<style>

</style>
