<script>
  import "../styles/global.css";
  let fechado = false;
</script>

<!-- Ícones Remix -->
<link
  href="https://cdn.jsdelivr.net/npm/remixicon/fonts/remixicon.css"
  rel="stylesheet"
/>

<div class="app">
  <!-- MENU -->
  <aside class:fechado={fechado}>

    <!-- LOGO -->
    <div class="logo">
        <img src="/icons/apple-touch-icon-57x57.png" alt="Logo" class="logo-img" />
      <span>Gódas Kids</span>
    </div>

    <!-- LINKS -->
    <nav>
      <a href="/cadastros/clientes">
        <i class="ri-user-line"></i>
        <span>Clientes</span>
      </a>

      <a href="/cadastros/fornecedores">
        <i class="ri-store-line"></i>
        <span>Fornecedores</span>
      </a>

      <a href="/cadastros/produtos/consultar">
        <i class="ri-shirt-line"></i>
        <span>Produtos</span>
      </a>

      <a href="/estoque">
        <i class="ri-box-3-line"></i>
        <span>Estoque</span>
      </a>
    </nav>

    <!-- BOTÃO ABRIR/FECHAR -->
    <button class="toggle" on:click={() => (fechado = !fechado)}>
      <i
        class={
          fechado
            ? "ri-arrow-right-s-line"
            : "ri-arrow-left-s-line"
        }
      ></i>
      <span>{fechado ? "Abrir" : "Fechar"}</span>
    </button>
  </aside>

  <!-- CONTEÚDO PRINCIPAL -->
  <main>
    <slot />
  </main>
</div>
