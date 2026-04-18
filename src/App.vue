<script setup lang="ts"></script>

<template>
  <div id="container">

    <div id="containerFilho">
      <TheHeader/>
      <div :id="idDivAdd">
        <input type="text" :id="idInput" v-model="novoTexto" @keyup.enter="addNewTasks">
        <span @click="addNewTasks" :id="idDivBotao">+</span>
      </div>
      <div v-if="registros.length > 0" id="divRegistros">
        <div v-for="item in registros" >
          <div :class="classTable">
              <span @click="checkToDo(item.id)" :id="`registro_${item.id}`" :class="['check', (item.checked) ? 'checked' : 'unchecked']">✓</span>
              <input type="text" v-model="item.texto" :id="`input_${item.id}`" placeholder="Digite suas tarefas">
              <span @click="removeTask(item.id)" :class="classRemoveButton">-</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import TheHeader from "./components/TheHeader.vue";

  export default{
    name: 'app',
    components:{
      TheHeader,
    },
    data() {
      return {
        idx: 1,
        registros: [],
        novoTexto: '',

        // IDS
        idInput: 'idNovoCampo',
        idDivAdd: 'idDivAdd',
        idDivBotao: 'idDivBotao',
        classTable: 'classTable',
        classRemoveButton: 'classRemoveButton',

      }
      
    },
    methods:{
        addNewTasks(){

          if(this.novoTexto.trim() !== ''){
            this.registros.push({
              id: this.idx++,
              texto: this.novoTexto,
              checked: false
            });

            this.novoTexto = ''; // limpa input
          }

        },
        checkToDo(id){
            const checkToDo = this.registros.find(i => i.id === id);
            const registro = document.getElementById(`registro_${id}`);
            const input = document.getElementById(`input_${id}`);
            if(registro.classList.contains('checked')){
              checkToDo.checked = false;  
              input?.removeAttribute('disabled');
            } else {
              checkToDo.checked = true;
              input?.setAttribute('disabled', 'disabled');
            }
        },

        removeTask(id){
          const registro = document.getElementById(`registro_${id}`);
          registro.parentElement.remove();

        }
    }
  }
</script>

<style scoped></style>
<style>


  #container{
    display: flex;
    justify-content: center;
    margin-top: 20%;
  }

  #idDivAdd{
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 30px,
  }  
  #idDivBotao {
    background-color: #59c765;
    border-radius: 5px;
    margin-left: 1rem;
    height: 1.5rem;
    width: 1.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: 800;
    color: #fff;
    font-size: 25px;
    cursor: pointer
  }

  .classTable{
    margin-top: 1rem;
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px,

  }
  .checked{
    background-color: #59c765;

  }
  .unchecked{
    background-color: #cce5af;

  }
  .unchecked:hover{
    background-color: #59c765;

  }
  .check{
    cursor:pointer;
    margin-right: 1rem;
    border-radius: 5px;
    display: flex;
    text-align: center;
    justify-content: center;
    height: 1.5rem;
    width: 1.5rem;
    color: #fff;

  } 
  .classRemoveButton{
    cursor: pointer;
    margin-left: 1rem;
    height: 1.5rem;
    width: 1.5rem;
    border-radius: 5px;
    color: #fff;
    font-weight: 700;
    font-size: 25px;
    text-align: center;

    display: flex;
    align-items: center;
    justify-content: center;
    background-color: #FF2C2C;
  }

  #idNovoCampo{
    margin-left: 2.5rem;
  }

  #divRegistros{
    margin-top: 1rem;
    max-height: 200px;
    overflow: auto;

  }
</style>
