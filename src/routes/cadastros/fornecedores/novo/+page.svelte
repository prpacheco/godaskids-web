<script>
  let nome = "";
  let telefone = "";
  let cpf = "";
  let email = "";

  // endereço
  let cep = "";
  let rua = "";
  let bairro = "";
  let cidade = "";
  let uf = "";
  let numero = "";
  let complemento = "";

  import Endereco from "$lib/components/EnderecoForm.svelte";

  let fornecedor = {
    nome: "",
    telefone: "",
    cpf: "",
    email: "",
    endereco: {}
  };

</script>

<h1>Cadastro de Fornecedor</h1>

<form class="form">
  <div class="campo">
    <label>Nome</label>
    <input bind:value={nome} placeholder="Nome completo" />
  </div>

  <div class="campo">
    <label>Telefone</label>
    <input bind:value={telefone} placeholder="(47) 99999-0000" />
  </div>

  <div class="campo">
    <label>CPF</label>
    <input bind:value={cpf} placeholder="000.000.000-00" />
  </div>

  <div class="campo">
    <label>E-mail</label>
    <input type="email" bind:value={email} placeholder="email@dominio.com" />
  </div>

  <Endereco bind:endereco={fornecedor.endereco} onChange={(e) => (fornecedor.endereco = e)}/>
  
  <button class="btn">Salvar</button>
</form>
