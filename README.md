<!DOCTYPE html>
<html lang="pt-br">
<head>
  <meta charset="UTF-8">
  <title>Espaço Tai Botelho</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background-color: #fff0f5;
      margin: 0;
      padding: 0;
    }

    header {
      background-color: #e91e63;
      color: white;
      padding: 20px;
      text-align: center;
    }

    main {
      padding: 20px;
    }

    form {
      background-color: #fce4ec;
      padding: 15px;
      border-radius: 8px;
      max-width: 400px;
      margin-bottom: 20px;
    }

    input, select, button {
      width: 100%;
      padding: 8px;
      margin-top: 5px;
    }

    ul {
      list-style: none;
      padding: 0;
    }

    li {
      background-color: #f8bbd0;
      margin: 5px 0;
      padding: 10px;
      border-radius: 5px;
    }
  </style>
</head>
<body>

  <header>
    <h1>Espaço Tai Botelho</h1>
  </header>

  <main>
    <h2>📅 Agendar Atendimento</h2>
    <form id="form-agendamento">
      <label>Nome do Cliente:</label><br>
      <select id="cliente" required>
        <option value="">Selecione um cliente</option>
      </select><br><br>

      <label>Serviço:</label><br>
      <input type="text" id="servico" required><br><br>

      <label>Data e Hora:</label><br>
      <input type="datetime-local" id="datahora" required><br><br>

      <label>Profissional:</label><br>
      <input type="text" id="profissional" required><br><br>

      <button type="submit">Agendar</button>
    </form>

    <ul id="lista-agendamentos"></ul>

    <h2>👩‍🦰 Cadastro de Clientes</h2>
    <form id="form-cliente">
      <label>Nome:</label><br>
      <input type="text" id="nome-cliente" required><br><br>

      <label>Telefone:</label><br>
      <input type="tel" id="telefone-cliente" required><br><br>

      <button type="submit">Cadastrar Cliente</button>
    </form>

    <ul id="lista-clientes"></ul>
  </main>

  <script>
    // AGENDAMENTO
    const form = document.getElementById('form-agendamento');
    const lista = document.getElementById('lista-agendamentos');

    form.addEventListener('submit', function (e) {
      e.preventDefault();

      const cliente = document.getElementById('cliente').value;
      const servico = document.getElementById('servico').value;
      const datahora = document.getElementById('datahora').value;
      const profissional = document.getElementById('profissional').value;

      const agendamento = { cliente, servico, datahora, profissional };
      adicionarNaLista(agendamento);
      salvarAgendamento(agendamento);
      form.reset();
    });

    function adicionarNaLista(agendamento) {
      const item = document.createElement('li');
      item.textContent = `${agendamento.datahora} - ${agendamento.cliente} | ${agendamento.servico} com ${agendamento.profissional}`;
      lista.appendChild(item);
    }

    function salvarAgendamento(agendamento) {
      const agendamentos = JSON.parse(localStorage.getItem('agendamentos')) || [];
      agendamentos.push(agendamento);
      localStorage.setItem('agendamentos', JSON.stringify(agendamentos));
    }

    // CLIENTES
    const formCliente = document.getElementById('form-cliente');
    const listaClientes = document.getElementById('lista-clientes');

    formCliente.addEventListener('submit', function (e) {
      e.preventDefault();

      const nome = document.getElementById('nome-cliente').value;
      const telefone = document.getElementById('telefone-cliente').value;

      const cliente = { nome, telefone };
      adicionarClienteNaLista(cliente);
      salvarCliente(cliente);
      formCliente.reset();
    });

    function adicionarClienteNaLista(cliente) {
      const item = document.createElement('li');
      item.textContent = `${cliente.nome} - ${cliente.telefone}`;
      listaClientes.appendChild(item);
    }

    function salvarCliente(cliente) {
      const clientes = JSON.parse(localStorage.getItem('clientes')) || [];
      clientes.push(cliente);
      localStorage.setItem('clientes', JSON.stringify(clientes));
      atualizarListaDeClientes();
    }

    function atualizarListaDeClientes() {
      const select = document.getElementById('cliente');
      if (!select) return;

      select.innerHTML = '<option value="">Selecione um cliente</option>';

      const clientes = JSON.parse(localStorage.getItem('clientes')) || [];
      clientes.forEach(cliente => {
        const option = document.createElement('option');
        option.value = cliente.nome;
        option.textContent = cliente.nome;
        select.appendChild(option);
      });
    }

    // CARREGAR DADOS AO INICIAR
    window.addEventListener('load', () => {
      const agendamentos = JSON.parse(localStorage.getItem('agendamentos')) || [];
      agendamentos.forEach(adicionarNaLista);

      const clientes = JSON.parse(localStorage.getItem('clientes')) || [];
      clientes.forEach(adicionarClienteNaLista);

      atualizarListaDeClientes();
    });
  </script>

</body>
</html>

