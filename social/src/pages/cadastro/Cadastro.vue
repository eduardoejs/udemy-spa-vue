<template>
  <login-template>

    <span slot="menuesquerdo">
      <img src="https://static.quizur.com/i/b/5b035c77702c13.471891555b035c775c3ff9.54480388.png" alt="social" class="responsive-img">
    </span>

    <span slot="principal">

      <h2>Cadastro</h2>
      <input type="text" placeholder="Nome" v-model="usuario.name">
      <input type="email" placeholder="E-Mail" v-model="usuario.email">
      <input type="password" placeholder="Senha" v-model="usuario.password">
      <input type="password" placeholder="Confirme sua Senha" v-model="usuario.password_confirmation">
      <button type="button" class="btn waves-effect waves-light" v-on:click="cadastrar()">Enviar</button>
      <router-link to="/login" class="btn orange waves-effect waves-light">Já tenho conta</router-link>

    </span>

  </login-template>
</template>

<script>
  import LoginTemplate from '@/templates/LoginTemplate'
  import axios from 'axios';

  export default {
    name: 'Cadastro',
    data () {
      return {
        usuario:{name:'', email:'', password:'', password_confirmation:''}
      }
    },
    components:{
      LoginTemplate
    },
    methods:{
      cadastrar(){
        axios.post('http://127.0.0.1:8000/api/cadastro', {
            name: this.usuario.name,
            email: this.usuario.email,
            password: this.usuario.password,
            password_confirmation: this.usuario.password_confirmation
          })
          .then(response => {
            //console.log(response);
            if(response.data.token){
              //cadastro com sucesso
              //console.log('sucesso');
              sessionStorage.setItem('usuario',JSON.stringify(response.data));
              this.$router.push('/');
            }else if(response.data.status == false){
              //login não existe
              //console.log('erro ao cadastrar');
              alert('Erro ao inserir registro');
            }else{
              //errors de validação
              //console.log('erro de validacao');
              let erros = '';
              for(let erro of Object.values(response.data)){
                erros += erro + ' ';
              }
              alert(erros);
            }
          })
          .catch(e => {
            //this.errors.push(e)
            //console.log(e);
            alert('Não foi possível realizar a operação. Tente novamente mais tarde!')
          })
      }
    }

  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
