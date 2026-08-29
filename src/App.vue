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
      <input type="text" v-model="termoBusca" placeholder="Digite sua busa..." class="field" />
    </form>

    <ul class="lista-contatos">
      <li v-for="contato in filtrados" :key="contato.id">
        <p><strong>Nome: </strong>{{ contato.nome }}</p>
        <p><strong>E-mail:</strong>{{ contato.email }}</p>
        <div class="actions">
          <button class="btn-editar" @click="editarDados(contato.id)">Editar</button>
          <button class="btn-remover" @click="remover(contato.id)">Excluir</button>
        </div>
      </li>
    </ul>

    <p v-if="termoBusca && filtrados.length === 0">
      Nenhum contato encontrado.
    </p>



    <div class="window-editar" v-if="exibirModalEditar">
      <div class="modal-content">
        <div class="formulario-header">
          <h1>Formulário de Atualização</h1>
        </div>

        <form @submit.prevent="salvarEdicao" class="formulario-add-contato">
          <input type="text" v-model="formEditar.nome" placeholder="Nome completo" />
          <input type="text" v-model="formEditar.email" placeholder="E-mail" />
          <div class="modal-actions">
            <button type="submit" class="btn-add" @click="salvarEdicao(contato.id)">Atualizar</button>
            <button class="btn-close" @click="cancelarEdicao">Cancelar</button>
          </div>
        </form>
      </div>
    </div>



  </section>
</template>

<script setup>
import {ref, computed} from 'vue';
const modalResposta=ref(false);

const exibirModalEditar = ref(false);

const editarDados=(id)=>{
 const contato = contatos.value.find(contato=>contato.id===id)
 if(contato){
  exibirModalEditar.value=true;
  formEditar.value = {...contato}
 }
}

const salvarEdicao =()=>{
  const index = contatos.value.findIndex(u=>u.id===formEditar.value.id);

  if(index !== -1){
    contatos.value[index]={...formEditar.value}
      exibirModalEditar.value=false;
  }
}

const cancelarEdicao=()=>{
  exibirModalEditar.value=false;
}

const formulario = ref({
  nome:'',
  email:''
})

const formEditar = ref({
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
  
const remover=(id)=>{
  if(confirm("Deseja cancelar?")){
       contatos.value = contatos.value.filter(usuario=>usuario.id !== id)
  } 
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
 
</script>
