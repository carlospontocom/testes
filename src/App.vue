<script setup>
</script>

<template>

  <section class="container">

    <div v-if="modalResposta" class="avisoSucesso">Cadastrado com sucesso</div>

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

      <li v-for="contato in contatos" :key="contato.id">
        <p><strong>Nome: </strong>{{contato.nome}}</p>
        <p><strong>E-mail:</strong>{{contato.email }}</p>
        <div class="actions">
          <button class="btn-editar">Editar</button>
          <button class="btn-remover" @click="remover(contato.id)">Excluir</button>
        </div>
      </li>

    </ul>

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

  const contatoExistente = contatos.value.find(u=>u.email===formulario.value.email)

  if(!contatoExistente){
    contatos.value.push({
      id:Date.now(),
      nome:formulario.value.nome,
      email:formulario.value.email
    })
    //mostra resposta de sucesso ao usuário
    modalResposta.value = true;
    tempoMensagemResposta();
    limparCampos();
  }
  else{
    alert("Contato já cadastrado! Tente outro")
  }  
}

const abrirModalEdicao=()=>{
   modalResposta.value = true;
   tempoMensagemResposta();
  }


const remover=(id)=>{
  if(confirm("Deseja cancelar?")){
      contatos.value = contatos.value.find(usuario=>usuario.id !== id)
  }
  //modalResposta.value = true;
  //tempoMensagemResposta();
}

const tempoMensagemResposta =()=>{
  setTimeout(() => {
    modalResposta.value =false
   }, 1000);
}


const limparCampos =()=>{
  formulario.value.nome = ''
  formulario.value.email = ''
}
</script>
