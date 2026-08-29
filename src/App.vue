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

    <form @submit.prevent="buscar">
       <input type="text" v-model="termoBusca" placeholder="Digite sua busa..." class="field"/>
    </form>

    <ul class="lista-contatos">
      <li v-for="contato in filtrados" :key="contato.id">
        <p><strong>Nome: </strong>{{contato.nome}}</p>
        <p><strong>E-mail:</strong>{{contato.email }}</p>
        <div class="actions">
          <button class="btn-editar">Editar</button>
          <button class="btn-remover">Excluir</button>
        </div>
      </li>
    </ul>

    <p v-if="termoBusca && filtrados.length === 0">
      Nenhum contato encontrado.
    </p>

  </section>
</template>

<script setup>
import {ref, computed} from 'vue';
const modalResposta=ref(false)

const formulario = ref({
  nome:'',
  email:''
})

const contatos = ref([])

const termoBusca = ref('')

const adicionarUsuario = () =>{
 if (!formulario.value.nome.trim() || !formulario.value.email.trim()) {
    alert("Preencha o nome e o e-mail!");
    return;
  }
 
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


const remover=()=>{
  modalResposta.value = true;
  tempoMensagemResposta();
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

const filtrados = computed(()=>{
  return contatos.value.filter(u=>u.nome.toLowerCase().includes(termoBusca.value.toLowerCase()) || u.email.toLowerCase().includes(termoBusca.value.toLowerCase() ))
})

 const totalFiltrados = filtrados.value.length;

 if(totalFiltrados < 0){
  
 }


</script>
