<script lang="ts">
  // Tipo do produto
  type Produto = {
    id?: number;
    nome: string;
    quantidade: number;
  };

  let produtos: Produto[] = [];

  async function carregarProdutos() {
    try {
      const resp = await fetch("http://localhost:8080/api/produtos");
      produtos = await resp.json();
    } catch (e) {
      console.log("Backend ainda não está disponível.");
    }
  }

  carregarProdutos();
</script>

<style>
  .container {
    max-width: 600px;
    margin: 40px auto;
    padding: 20px;
    border-radius: 12px;
    background: #ffffff;
    box-shadow: 0 0 12px rgba(0,0,0,0.08);
  }

  h1 {
    margin-bottom: 20px;
    text-align: center;
  }

  table {
    width: 100%;
    border-collapse: collapse;
  }

  th, td {
    border-bottom: 1px solid #ddd;
    padding: 10px;
    text-align: left;
  }

  th {
    background: #eee;
  }

  .btn {
    background: #0077ff;
    color: white;
    padding: 10px 16px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    margin-bottom: 15px;
    font-size: 15px;
  }

  .btn:hover {
    background: #005fcc;
  }
</style>

<div class="container">
  <h1>📦 Controle de Estoque — Gódas Kids</h1>

  <button class="btn" on:click="{carregarProdutos}">
    Atualizar lista
  </button>

  {#if produtos.length === 0}
    <p>Nenhum produto encontrado ou API offline.</p>
  {/if}

  {#if produtos.length > 0}
    <table>
      <thead>
        <tr>
          <th>Produto</th>
          <th>Quantidade</th>
        </tr>
      </thead>

      <tbody>
        {#each produtos as p}
          <tr>
            <td>{p.nome}</td>
            <td>{p.quantidade}</td>
          </tr>
        {/each}
      </tbody>
    </table>
  {/if}
</div>
