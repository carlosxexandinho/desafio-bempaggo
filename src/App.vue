<script setup>
  import { ref } from '@vue/reactivity'
  //import { RouterLink, RouterView } from 'vue-router'

  // controle das mensagens de status
  const is_msg = ref(false)
  const is_loading = ref(true)
  const is_loading_msg = ref('Efetuando pedido')

  // Dados formato json que podem ser obtidos através de uma API
  const observacoes = ref('');
  const db_adesivos = ref([
    {
      name: 'Vue.Js',
      icon: 'fa-brands fa-vuejs fa-5x',
      quant: 0
    },
    {
      name: 'Html5',
      icon: 'fa-brands fa-html5 fa-5x',
      quant: 0
    },
    {
      name: 'CSS',
      icon: 'fa-brands fa-css3 fa-5x',
      quant: 0
    },
    {
      name: 'Angular',
      icon: 'fa-brands fa-angular fa-5x',
      quant: 0
    },
    {
      name: 'Java',
      icon: 'fa-brands fa-java fa-5x',
      quant: 0
    },
  ])



  // função de incremento do valor (campo quantidade)
  const increment_quant = function (key){
    db_adesivos.value[key].quant += 1;
  }
  // função de decremento do valor (campo quantidade)
  const decrement_quant = function (key){
    var quant = db_adesivos.value[key].quant
    if(quant > 0){
      db_adesivos.value[key].quant -= 1;
    }
  }
  // Verifica se a quantidade está com valor negativo
  const verificar_quantidade = function (key){
    var quant = db_adesivos.value[key].quant
    if(quant < 0){
      db_adesivos.value[key].quant = Math.abs(quant);
    }
  }

  // Operação de coleta dos dados inseridos pelo usuario e posteriormente adicionar ao banco de dados via API

  const save = function (){
    var dados = db_adesivos.value; // contem todas informações dos produtos e quantidades selecionadas para ser salvas no banco de dados
    var obser = observacoes.value; // dados será salvo no banco de dados
    is_msg.value = true;

    const verificação = () =>{
        return dados.reduce((acc, som) => acc += som.quant, 0)
    }
    console.log(verificação());

    // timeout para simução de carregando
    setTimeout(() => {
      // verifica se algum se existe ao menos um item adicionado
      if(verificação() < 1){
          alert('É necessário adicionar pelo menos um adesivo para prosseguir')
          is_msg.value = false;
      }else{
        // Executa este escopo quando a validação foi bem sucedida
        // Usar as variaveis "dados" e "obser" para enviar os dados para backend
          is_loading.value = false;
          is_loading_msg.value = 'Pedido realizado com sucesso!'
      }
    }, 1200);

  }
</script>

<template>

  <section class="container">
    
      <div class="title">
          <p class="sup">Formulário para</p>
          <p class="sub">Compra de Pacote <br> de adesivos</p>
      </div>
      <div class="content">
          <p style="margin: 15px 0"><b>Selecione os adesivos:</b></p>
          <div class="products">
              
                <div class="item" v-for="(adesivo, key) in db_adesivos" :key="adesivo.nome">
                  <div class="title">{{ adesivo.name }}</div>
                  <div> <i :class="adesivo.icon" style="color: #004ba2"></i> </div>
                  <div>
                      <input type="button" value="-" @click="decrement_quant(key)">
                      <input type="text" name="" id="" class="inputquant" @change="verificar_quantidade(key)" v-model="db_adesivos[key].quant">
                      <input type="button" value="+" @click="increment_quant(key)">
                  </div>
                </div>

          </div>
      </div>
      <div class="observacoes">
          <b>Observações:</b>
          <p>
              <textarea v-model="observacoes" name="" id="" cols="30" rows="10" placeholder="Alguma dúvida? Recado?"></textarea>
          </p>
      </div>
      <div class="footer">
          <div class="msg">
            <b v-show="is_msg"> <span v-show="is_loading"><i class="fas fa-circle-notch fa-spin"></i></span> {{is_loading_msg}}...</b>
          </div>
          <div>
            <input type="button" value="COMPRAR" @click="save">
          </div>
      </div>

  </section>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@300;400&display=swap');

@import url('css/content.css');
@import url('css/observacoes.css');


* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: 'Roboto', sans-serif;
}

#app{
    display: flex;
    align-items: center;
    justify-content: center;
}


/* Estilo universal dos botões */
/*-----------------------------------------------------*/
button, input[type=button]{
  padding: 8px;
  background: #004fa3;
  color: white;
  border-radius: 5px;
  border: 0;
}

button, input[type=button]:active{
    background:#0b8ca3;
}
/*-----------------------------------------------------*/
/* end style button */


body{
  display: flex;
  justify-content: center;
  align-items: center;
  background: rgb(0,70,160);
  background: linear-gradient(132deg, rgba(0,70,160,1) 0%, rgba(0,169,197,1) 100%);
  background-repeat: no-repeat;
  background-attachment: fixed;
  height: auto;
}
section.container{
  display: grid;
  grid-template-rows: 3.5fr auto auto 1.5fr;
  width: 500px;
  height: auto;
  background: white;
  border-radius: 15px;
  padding: 0;
  margin: 60px 0;
  -webkit-box-shadow: 4px 14px 28px -9px rgba(0,0,0,0.53);
  -moz-box-shadow: 4px 14px 28px -9px rgba(0,0,0,0.53);
  box-shadow: 4px 14px 28px -9px rgba(0,0,0,0.53);
}

/* ******************************************/
/* ************* Estilos Titulo *************/
/* ******************************************/
section.container .title{
  margin-top: 0;
  border-radius: 15px 15px 100% 100% / 15px 15px 80% 80%;
  background: #4086d7;
  padding: 19px 40px;
  text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.30);
}

section.container .title .sub{
  font-weight: 400;
  font-size: 25px;
  color:white
}
section.container .title .sup{
  font-weight: 300;
  font-size: 15px;
  color:white
}


/* ******************************************/
/* ************* Estilos Footer *************/
/* ******************************************/
section.container .footer{
  padding: 15px 20px;
  background: #cef4ff;
  border-radius: 0 0 15px 15px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}



</style>
