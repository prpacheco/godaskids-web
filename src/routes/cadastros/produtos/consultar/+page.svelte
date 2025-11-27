<script>
  let descricao = "";
  let categoria = "";
  let faixa = "";
  let tamanho = "";

  const tamanhosInfantil = ["1", "2", "4", "6", "8", "10", "12", "14"];
  const tamanhosAdulto = ["P", "M", "G", "GG"];
  let tamanhosFiltrados = [];

  let produtos = [];

  async function pesquisar() {
    const params = new URLSearchParams({
      descricao,
      categoria,
      faixa,
      tamanho
    });

    const resp = await fetch("http://localhost:8080/produtos/consulta?" + params + "&page=0");
    const data = await resp.json();
    produtos = data.content || [];

  }

  function limpar() {
    descricao = "";
    categoria = "";
    faixa = "";
    tamanho = "";
    tamanhosFiltrados = [];
    produtos = [];
  }

  function novo() {
    window.location.href = "/cadastros/produtos/novo";
  }

  // Atualiza o combo conforme faixa
    function atualizarTamanhos() {
      if (faixa === "INFANTIL") {
        tamanhosFiltrados = tamanhosInfantil;
      } else if (faixa === "ADULTO") {
        tamanhosFiltrados = tamanhosAdulto;
      } else {
        tamanhosFiltrados = [];
      }

      tamanho = ""; // limpa seleção atual
    }

    import { goto } from "$app/navigation";

      function irParaEdicao(id) {
        goto('/cadastros/produtos/editar/${id}');
      }

</script>

<div class="page-container">
  <div class="card">
    <form class="form">

      <h1>Consulta de Produtos</h1>

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

      </div>

      <div class="botoes">
        <button class="btn azul" on:click|preventDefault={pesquisar}>Pesquisar</button>
        <button class="btn cinza" on:click|preventDefault={limpar}>Limpar</button>
        <button class="btn verde" on:click|preventDefault={novo}>Novo</button>
      </div>

    </form>
  </div>

  {#if produtos.length > 0}
    <table>
      <thead>
        <tr>
          <th>Descrição</th>
          <th>Categoria</th>
          <th>Faixa</th>
          <th>Tamanho</th>
          <th>Preço</th>
        </tr>
      </thead>
      <tbody>
        {#each produtos as p}
        <tr on:click={() => irParaEdicao(p.id)} class="linha-click">
          <td>{p.descricao}</td>
          <td>{p.categoria}</td>
          <td>{p.faixa}</td>
          <td>{p.tamanho}</td>
          <td>{p.preco}</td>
        </tr>
        {/each}
      </tbody>
    </table>
  {/if}

</div>
