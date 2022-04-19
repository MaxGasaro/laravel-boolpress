<template>
  <div class="container">
    <form @submit.prevent="sendForm">

       <div v-if="success" class="alert alert-success">
          Email inviata con successo!!
        </div>


      <div class="form-group">
        <label for="name">What's your name?</label>
        <input type="text" class="form-control" id="name" :class="{'is-invalid':errors.name}" name="name" placeholder="name@example.com" v-model="name">
        <p v-for="(error, index) in errors.name" :key="'error_name'+index" class="invalid-feedback">
          {{error}}
        </p>
      </div>
      <div class="form-group">
        <label for="email">Inserisci la tua email</label>
        <input type="email" class="form-control" :class="{'is-invalid':errors.email}" id="email" name="email" placeholder="name@example.com" v-model="email">
        <p v-for="(error, index) in errors.email" :key="'error_email'+index" class="invalid-feedback">
          {{error}}
        </p>        
      </div>
      <div class="form-group">
        <label for="message">Cosa vuoi dirci?</label>
        <textarea class="form-control" :class="{'is-invalid':errors.message}" id="message" name="message" rows="7" v-model="message"></textarea>
        <p v-for="(error, index) in errors.message" :key="'error_message'+index" class="invalid-feedback">
          {{error}}
        </p>
      </div>

      <button type="submit" class="btn btn-primary">{{sendingProgress?'Invio in corso...':'Invia'}}</button>
    </form>
  </div>
</template>

<script>
export default {
  name: "Contact",
  data() {
    return {
      name: '',
      email: '',
      message: '',
      sendingProgress: false,
      errors: {},
      success: false
    }
  },
  methods: {
    sendForm() {

      this.sendingProgress = true;

      axios.post('/api/contacts', {
        'name': this.name,
        'email': this.email,
        'message': this.message
      }).then((response) => {
        this.sendingProgress = false;
        if (response.data.error) {
          this.errors = response.data.errors;
          this.success = false;
        } else {
          this.success = true;
          this.name = '';
          this.email = '';
          this.message = '';
          this.errors = {};
        }
        console.log(response);
      }).catch((err) => {
        //
      });

    }
  }
};
</script>

<style>
</style>