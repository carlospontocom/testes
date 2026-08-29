<script setup>
</script>

<template>

  <section class="container">

    <div class="formulario-header">
      <h1>Formulário de cadastro de usuário</h1>
      <p>Adicioe e gerencie sua lista telefônica</p>
      <span>Organize seus números e e-mais</span>
    </div>

    <form @submit.prevent="adicionarUsuario" class="formulario-add-contato">
      <input type="text" v-model="formulario.nome" placeholder="Nome completo" />
      <input type="text" v-model="formulario.email" placeholder="E-mail" />
      <button type="submit" class="btn-add">Adicionar</button>
    </form>

    <ul class="lista-contatos">

      <li>
        <p><strong>Nome: </strong>Carlos Nascimento</p>
        <p><strong>E-mail:</strong>carlosnascimento@gmail.com</p>
        <div class="actions">
          <button class="btn-editar" @click="abrirModalEdicao">Editar</button>
          <button class="btn-remover" @click="remover">Excluir</button>
        </div>
      </li>

      <li>
        <p><strong>Nome: </strong>Carlos Nascimento</p>
        <p><strong>E-mail:</strong>carlosnascimento@gmail.com</p>
        <div class="actions">
          <button class="btn-editar">Editar</button>
          <button class="btn-remover">Excluir</button>
        </div>
      </li>

    </ul>

    <div v-if="modalResposta" class="avisoSucesso">Sucesso....</div>

  </section>
</template>

<script setup>
import {ref} from 'vue';
const modalResposta=ref(false)

const formulario = ref({
  nome:'',
  email:''
})

const contatos = ref([])

const adicionarUsuario = () =>{
  alert("Adicionado...")
}

const abrirModalEdicao=()=>{
   modalResposta.value = true;
   tempoMensagemResposta();
  }


const remover=()=>{
  modalResposta.value = true;
  tempoMensagemResposta();
}

const tempoMensagemResposta =()=>{
  setTimeout(() => {
    modalResposta.value =false
   }, 1000);
}

</script>
