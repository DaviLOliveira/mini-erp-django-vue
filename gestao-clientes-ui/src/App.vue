<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

// --- TODA A LÓGICA JAVASCRIPT CONTINUA EXATAMENTE A MESMA ---

const clientes = ref([])
const loading = ref(true)
const error = ref(null)

const novoCliente = ref({
  nome: '',
  email: '',
  telefone: '',
})

const editingClientId = ref(null)
const editingClientData = ref({})

async function fetchClientes() {
  try {
    loading.value = true
    const response = await axios.get('http://127.0.0.1:8000/api/clientes/')
    clientes.value = response.data
  } catch (err) {
    console.error('Erro ao buscar clientes:', err)
    error.value = 'Não foi possível carregar os clientes. Verifique se a API está rodando.'
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchClientes()
})

async function criarCliente() {
  error.value = null
  try {
    const response = await axios.post('http://127.0.0.1:8000/api/clientes/', novoCliente.value)
    clientes.value.unshift(response.data)
    novoCliente.value.nome = ''
    novoCliente.value.email = ''
    novoCliente.value.telefone = ''
  } catch (err) {
    console.error('Erro ao criar cliente:', err)
    if (err.response && err.response.data) {
      error.value = `Erro: ${JSON.stringify(err.response.data)}`
    } else {
      error.value = 'Ocorreu um erro ao tentar criar o cliente.'
    }
  }
}

async function deletarCliente(clienteId) {
  if (!window.confirm('Você tem certeza que deseja excluir este cliente?')) {
    return
  }
  try {
    await axios.delete(`http://127.0.0.1:8000/api/clientes/${clienteId}/`)
    clientes.value = clientes.value.filter((cliente) => cliente.id !== clienteId)
  } catch (err) {
    console.error('Erro ao deletar cliente:', err)
    error.value = 'Ocorreu um erro ao tentar excluir o cliente.'
  }
}

function habilitarEdicao(cliente) {
  editingClientId.value = cliente.id
  editingClientData.value = { ...cliente }
}

function cancelarEdicao() {
  editingClientId.value = null
}

async function salvarEdicao(clienteId) {
  try {
    const response = await axios.put(
      `http://127.0.0.1:8000/api/clientes/${clienteId}/`,
      editingClientData.value,
    )
    const index = clientes.value.findIndex((c) => c.id === clienteId)
    if (index !== -1) {
      clientes.value[index] = response.data
    }
    cancelarEdicao()
  } catch (err) {
    console.error('Erro ao salvar edição:', err)
    error.value = 'Ocorreu um erro ao tentar salvar as alterações.'
  }
}
</script>

<template>
  <main id="app-container">
    <h1>Gestão de Clientes</h1>

    <form @submit.prevent="criarCliente" class="form-container">
      <h3>Adicionar Novo Cliente</h3>
      <div class="form-group">
        <input type="text" v-model="novoCliente.nome" placeholder="Nome" required />
        <input type="email" v-model="novoCliente.email" placeholder="Email" required />
        <input type="text" v-model="novoCliente.telefone" placeholder="Telefone" />
      </div>
      <button type="submit" class="btn-submit">Adicionar</button>
    </form>

    <div v-if="error" class="error-box">
      <p>{{ error }}</p>
    </div>

    <hr />

    <h2>Clientes Cadastrados</h2>
    <div v-if="loading">
      <p>Carregando...</p>
    </div>

    <div v-if="!loading">
      <table>
        <thead>
          <tr>
            <th>Nome</th>
            <th>Email</th>
            <th>Telefone</th>
            <th>Ações</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="cliente in clientes" :key="cliente.id">
            <td>
              <input
                v-if="editingClientId === cliente.id"
                v-model="editingClientData.nome"
                type="text"
              />
              <span v-else>{{ cliente.nome }}</span>
            </td>
            <td>
              <input
                v-if="editingClientId === cliente.id"
                v-model="editingClientData.email"
                type="email"
              />
              <span v-else>{{ cliente.email }}</span>
            </td>
            <td>
              <input
                v-if="editingClientId === cliente.id"
                v-model="editingClientData.telefone"
                type="text"
              />
              <span v-else>{{ cliente.telefone }}</span>
            </td>
            <td>
              <div v-if="editingClientId === cliente.id">
                <button @click="salvarEdicao(cliente.id)" class="btn-save">Salvar</button>
                <button @click="cancelarEdicao" class="btn-cancel">Cancelar</button>
              </div>
              <div v-else>
                <button @click="habilitarEdicao(cliente)" class="btn-edit">Editar</button>
                <button @click="deletarCliente(cliente.id)" class="btn-delete">Excluir</button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </main>
</template>

<style>
/* Esta tag de estilo NÃO tem 'scoped', então ela se aplica a toda a página */
:root {
  color-scheme: light; /* Força o tema claro */
}

body {
  margin: 0;
  font-family:
    'Inter',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    sans-serif;
  background-color: #f3f4f6; /* Fundo cinza claro para a PÁGINA INTEIRA */
  color: #111827;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  display: flex;
  justify-content: center;
  align-items: flex-start; /* Alinha no topo */
  min-height: 100vh;
  padding-top: 5rem; /* Espaço no topo */
}
</style>

<style scoped>
/* Esta tag de estilo TEM 'scoped', então só se aplica a este componente */

#app-container {
  width: 100%;
  max-width: 900px;
  padding: 2.5rem;
  background-color: #ffffff;
  border-radius: 12px;
  box-shadow:
    0 10px 15px -3px rgb(0 0 0 / 0.1),
    0 4px 6px -4px rgb(0 0 0 / 0.1);
}

h1,
h2 {
  border-bottom: 1px solid #e5e7eb;
  padding-bottom: 0.75rem;
  margin-bottom: 1.5rem;
}

h1 {
  font-size: 2rem;
  font-weight: 700;
}

h2 {
  font-size: 1.5rem;
  font-weight: 500;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 1.5rem;
}

th,
td {
  border-bottom: 1px solid #e5e7eb;
  padding: 1rem;
  text-align: left;
  vertical-align: middle;
}

thead {
  background-color: #f9fafb;
  color: #6b7280;
  font-size: 0.875rem;
  text-transform: uppercase;
}

tbody tr:hover {
  background-color: #f9fafb;
}

.form-container {
  background-color: #f9fafb;
  padding: 1.5rem;
  border-radius: 8px;
  margin-bottom: 2rem;
  border: 1px solid #e5e7eb;
}

.form-container h3 {
  margin-top: 0;
  font-size: 1.25rem;
}

.form-group {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 1rem;
  margin-bottom: 1.5rem;
}

input {
  width: 100%;
  box-sizing: border-box; /* Garante que padding não afete a largura */
  padding: 12px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
  font-size: 1rem;
  font-family: 'Inter', sans-serif;
  transition:
    border-color 0.2s,
    box-shadow 0.2s;
}

input:focus {
  outline: none;
  border-color: #4f46e5;
  box-shadow: 0 0 0 3px rgb(79 70 229 / 0.2);
}

button {
  padding: 10px 15px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 0.95rem;
  font-weight: 500;
  font-family: 'Inter', sans-serif;
  transition: all 0.2s;
  margin-right: 8px;
  color: white;
}
button:hover {
  opacity: 0.85;
}

.btn-submit {
  background-color: #4f46e5;
}
.btn-edit {
  background-color: #3b82f6;
}
.btn-save {
  background-color: #22c55e;
}
.btn-cancel {
  background-color: #6b7280;
}
.btn-delete {
  background-color: #ef4444;
}

.error-box {
  color: #991b1b;
  background-color: #fef2f2;
  border: 1px solid #ef4444;
  padding: 1rem;
  border-radius: 6px;
  margin-top: 1rem;
  text-align: center;
}
td > div {
  display: flex;
  flex-direction: row;
  gap: 8px; /* Adiciona um espaço consistente entre os botões */
}

/* Ajuste para que os botões não tenham margem desnecessária */
button {
  margin-right: 0;
}
</style>
