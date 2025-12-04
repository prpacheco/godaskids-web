<script>
  export let produto = {};
  export let modo = "novo";

  let id = produto.id;
  let descricao = produto.descricao ?? "";
  let categoria = produto.categoria ?? "";
  let faixa = produto.faixa ?? "";
  let tamanho = produto.tamanho ?? "";
  let preco = produto.preco ?? "";

  $: if (produto) {
    id = produto.id;
    descricao = produto.descricao ?? "";
    categoria = produto.categoria ?? "";
    faixa = produto.faixa ?? "";
    tamanho = produto.tamanho ?? "";
    preco = produto.preco ?? "";
    atualizarTamanhos(); // preenche ao carregar a edição
  }

  const tamanhosInfantil = ["1", "2", "4", "6", "8", "10", "12", "14"];
  const tamanhosAdulto = ["P", "M", "G", "GG"];

  let tamanhosFiltrados = [];

  function atualizarTamanhos() {
    tamanhosFiltrados =
      faixa === "INFANTIL"
        ? tamanhosInfantil
        : faixa === "ADULTO"
        ? tamanhosAdulto
        : [];

    if (!tamanhosFiltrados.includes(tamanho)) tamanho = "";
  }

  let mensagem = "";

  async function salvar() {
    const url =
      modo === "novo"
        ? "http://localhost:8080/produtos/salvar"
        : "http://localhost:8080/produtos/editar/" + id;

    const resp = await fetch(url, {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        id,
        descricao,
        categoria,
        faixa,
        tamanho,
        preco
      })
    });

    mensagem = await resp.text();
  }

  async function excluir() {
    if (!confirm("Excluir este produto?")) return;

    await fetch("http://localhost:8080/produtos/excluir/" + id, {
      method: "DELETE"
    });

    mensagem = "Produto removido!";
  }

  function mascaraPreco(e) {
    let valor = e.target.value;

    // tira tudo que não for número
    valor = valor.replace(/\D/g, "");

    // transforma em centavos
    valor = (Number(valor) / 100).toFixed(2);

    // formata no padrão BR
    valor = valor
      .toString()
      .replace(".", ",")
      .replace(/\B(?=(\d{3})+(?!\d))/g, ".");

    e.target.value = "R$ " + valor;

    preco = e.target.value; 
  }

</script>

<div class="page-container">
  <div class="card">
    <form class="form">

      {#if mensagem}
        <div class="msg-sucesso">{mensagem}</div>
      {/if}

      <h1>{modo === "novo" ? "Cadastrar Produto" : "Editar Produto"}</h1>

      <div class="grid">

              <div class="campo">
                <label for="descricao">Descrição</label>
                <input id="descricao" bind:value={descricao} />
              </div>

              <div class="campo">
                <label for="categoria">Categoria</label>
                <select id="categoria" bind:value={categoria}>
                  <option value="">Selecione...</option>
                  <option value="MASCULINO">Masculino</option>
                  <option value="FEMININO">Feminino</option>
                  <option value="UNISSEX">Unissex</option>
                </select>
              </div>

              <div class="campo">
                <label for="faixa">Faixa</label>
                <select id="faixa" bind:value={faixa} on:change={atualizarTamanhos}>
                  <option value="">Selecione...</option>
                  <option value="INFANTIL">Infantil</option>
                  <option value="ADULTO">Adulto</option>
                </select>
              </div>

              <div class="campo">
                <label for="tamanho">Tamanho</label>
                <select id="tamanho" bind:value={tamanho} disabled={tamanhosFiltrados.length === 0}>
                  <option value="">Selecione...</option>
                  {#each tamanhosFiltrados as t}
                    <option>{t}</option>
                  {/each}
                </select>
              </div>
              
              <div class="campo">
  <label for="preco">Preço</label>
  <input
    id="preco"
    bind:value={preco}
    on:input={mascaraPreco}
    placeholder="R$ 0,00"
  />
</div>



            </div>
      <div class="botoes">
        <button class="btn azul" on:click|preventDefault={salvar}>
          {modo === "novo" ? "Salvar" : "Atualizar"}
        </button>

        {#if modo === "editar"}
        <button class="btn vermelho" on:click|preventDefault={excluir}>
          Excluir
        </button>
        {/if}
      </div>

    </form>
  </div>
</div>
