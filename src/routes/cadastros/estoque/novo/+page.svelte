<script>
  let categoria = "";
  let subcategoria = "";
  let nome = "";
  let quantidade = 0;
  let valorUnitario = 0;
  let valorTotal = 0;
  let largura = "";
  let comprimento = "";
  let unidade = "";
  let fornecedor = "";
  let observacoes = "";

  const subcategoriasPorCategoria = {
    tecido: [
      "Malha",
      "Suede",
      "Poliamida",
      "Viscolycra",
      "Tricoline",
      "Moletom Careca",
      "Moletom Peluciado"
    ],
    elastico: ["1 cm", "2 cm", "3 cm", "5 cm"],
    linha: ["Reta", "Overlock"],
    etiqueta: ["Marca", "Tamanho"],
    embalagem: [
      "Saquinho plástico",
      "Sacola plástica",
      "Sacola papel",
      "Envelope de envio"
    ],
    adesivo: ["Logo"],
    aviamento: ["Zíper", "Fitas decorativas", "Botões", "Ilhós"],
    outros: ["Agulha de máquina", "Óleo de máquina", "Papel kraft"]
  };

  let subcategorias = [];

  function atualizarSubcategorias() {
    subcategorias = subcategoriasPorCategoria[categoria] || [];
    subcategoria = "";
  }

  // Calcula valor total
  $: valorTotal = quantidade * valorUnitario;
</script>

<h1>Cadastro de Estoque</h1>

<form class="form">
  <!-- Categoria -->
  <div class="campo">
    <label>Categoria</label>
    <select bind:value={categoria} on:change={atualizarSubcategorias}>
      <option value="">Selecione…</option>
      <option value="tecido">Tecido</option>
      <option value="elastico">Elástico</option>
      <option value="etiqueta">Etiqueta</option>
      <option value="linha">Linha</option>
      <option value="embalagem">Embalagem</option>
      <option value="adesivo">Adesivo</option>
      <option value="aviamento">Aviamento</option>
      <option value="outros">Outros</option>
    </select>
  </div>

  <!-- Subcategoria -->
  <div class="campo">
    <label>Subcategoria</label>
    <select bind:value={subcategoria} disabled={subcategorias.length === 0}>
      <option value="">Selecione…</option>
      {#each subcategorias as s}
        <option>{s}</option>
      {/each}
    </select>
  </div>

  <!-- Nome -->
  <div class="campo">
    <label>Nome</label>
    <input bind:value={nome} placeholder="Ex: Malha rosa bebê" />
  </div>

  <!-- Quantidade -->
  <div class="campo">
    <label>Quantidade</label>
    <input type="number" bind:value={quantidade} min="0" />
  </div>

  <!-- Valor unitário -->
  <div class="campo">
    <label>Valor Unitário</label>
    <input type="number" step="0.01" bind:value={valorUnitario} />
  </div>

  <!-- Valor total -->
  <div class="campo">
    <label>Valor Total</label>
    <input type="number" step="0.01" bind:value={valorTotal} disabled />
  </div>

  <!-- Largura -->
  <div class="campo">
    <label>Largura</label>
    <input bind:value={largura} placeholder="Ex: 1.40m" />
  </div>

  <!-- Comprimento -->
  <div class="campo">
    <label>Comprimento</label>
    <input bind:value={comprimento} placeholder="Ex: 2.00m" />
  </div>

  <!-- Unidade de medida -->
  <div class="campo">
    <label>Unidade de medida</label>
    <select bind:value={unidade}>
      <option value="">Selecione…</option>
      <option value="metro">Metro</option>
      <option value="unidade">Unidade</option>
      <option value="cone">Cone</option>
      <option value="rolo">Rolo</option>
      <option value="cartela">Cartela</option>
    </select>
  </div>

  <!-- Fornecedor -->
  <div class="campo">
    <label>Fornecedor</label>
    <input bind:value={fornecedor} placeholder="Nome da loja" />
  </div>

  <!-- Observações -->
  <div class="campo">
    <label>Observações</label>
    <textarea bind:value={observacoes}></textarea>
  </div>

  <!-- Botão -->
  <button class="btn">Salvar</button>
</form>

