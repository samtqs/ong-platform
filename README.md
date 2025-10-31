# ong-platform
ong-platform/
├─ index.html
├─ projetos.html
├─ cadastro.html
├─ README.md
├─ assets/
│  ├─ css/
│  │  └─ styles.css
│  ├─ js/
│  │  └─ masks.js
│  └─ img/
│     └─ (imagens otimizada: logo.webp, projeto1-400w.webp...)
```

Observações técnicas:
- Mobile-first: marcação pensada para telas pequenas; estilo CSS inicial incluso em `styles.css` (muito simples).
- Acessibilidade: uso de labels, fieldset/legend, aria-describedby para erros e aria-live para feedback.
- Formulário: `cadastro.html` contém validação nativa (required, pattern, maxlength) e máscaras JS para CPF, telefone e CEP.

--- FILE: assets/css/styles.css ---
/* Arquivo CSS mínimo apenas para estrutura. Expandir conforme necessário. */
:root{
  --max-width: 1100px;
  --gap: 1rem;
}
*{box-sizing:border-box}
html,body{margin:0;padding:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,'Helvetica Neue',Arial;line-height:1.4}
.container{max-width:var(--max-width);margin:0 auto;padding:1rem}
header,nav,main,footer{display:block}
.header-brand{display:flex;align-items:center;gap:.5rem}
nav ul{list-style:none;padding:0;margin:0;display:flex;gap:1rem}
a{color:inherit;text-decoration:none}
button{cursor:pointer}
form{display:block;margin-top:1rem}
.field-group{display:grid;grid-template-columns:1fr;gap:.5rem;margin-bottom:1rem}
@media(min-width:720px){
  .grid-2{display:grid;grid-template-columns:1fr 1fr;gap:1rem}
}
input,select,textarea{padding:.5rem;border:1px solid #cfcfcf;border-radius:6px}
input:focus,select:focus,textarea:focus{outline:3px solid Highlight;outline-offset:2px}
.error{color:#b00020;font-size:.95rem}
.sr-only{position:absolute;width:1px;height:1px;padding:0;margin:-1px;overflow:hidden;clip:rect(0,0,0,0);white-space:nowrap;border:0}

--- FILE: assets/js/masks.js ---
// masks.js - máscara simples e leve para CPF, telefone e CEP
(function(){
  function setCursorToEnd(el){ try{ el.selectionStart = el.selectionEnd = el.value.length }catch(e){} }

  function maskCPF(v){ v = v.replace(/\D/g,''); v = v.replace(/(\d{3})(\d)/,'$1.$2'); v = v.replace(/(\d{3})(\d)/,'$1.$2'); v = v.replace(/(\d{3})(\d{1,2})$/,'$1-$2'); return v }
  function maskPhone(v){ v = v.replace(/\D/g,''); if(v.length>10) v = v.replace(/(\d{2})(\d{5})(\d{4})/,'($1) $2-$3'); else if(v.length>5) v = v.replace(/(\d{2})(\d{4})(\d{0,4})/,'($1) $2-$3'); else if(v.length>2) v = v.replace(/(\d{2})(\d{0,5})/,'($1) $2'); return v }
  function maskCEP(v){ v = v.replace(/\D/g,''); v = v.replace(/(\d{5})(\d)/,'$1-$2'); return v }

  function attach(id, fn){ var el = document.getElementById(id); if(!el) return; el.addEventListener('input', function(e){ var pos = el.selectionStart; var old = el.value; el.value = fn(el.value); setCursorToEnd(el); }); }

  document.addEventListener('DOMContentLoaded', function(){
    attach('cpf', maskCPF);
    attach('telefone', maskPhone);
    attach('cep', maskCEP);
  });
})();

--- FILE: index.html ---
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>ONG Exemplo — Missão e Contato</title>
  <meta name="description" content="ONG Exemplo: missão, projetos e contato. Apoie e conheça nosso trabalho.">
  <link rel="stylesheet" href="assets/css/styles.css">
</head>
<body>
  <header class="site-header" role="banner">
    <div class="container header-brand">
      <img src="assets/img/logo.webp" alt="Logo ONG Exemplo" width="64" height="64">
      <div>
        <h1>ONG Exemplo</h1>
        <p class="sr-only">Presença institucional e projetos sociais</p>
      </div>
    </div>
    <nav aria-label="Navegação principal">
      <div class="container">
        <ul>
          <li><a href="index.html">Início</a></li>
          <li><a href="projetos.html">Projetos</a></li>
          <li><a href="cadastro.html">Cadastre-se</a></li>
        </ul>
      </div>
    </nav>
  </header>

  <main class="container" id="main" tabindex="0">
    <section aria-labelledby="quem-somos">
      <h2 id="quem-somos">Quem somos</h2>
      <p>Nossa missão é promover inclusão social e desenvolvimento comunitário por meio de ações educativas e de saúde.</p>
    </section>

    <section aria-labelledby="valores">
      <h2 id="valores">Missão, Visão e Valores</h2>
      <ul>
        <li>Missão: Fortalecer comunidades.</li>
        <li>Visão: Comunidades autossuficientes.</li>
        <li>Valores: Transparência, respeito, colaboração.</li>
      </ul>
    </section>

    <section aria-labelledby="contato">
      <h2 id="contato">Contato</h2>
      <address>
        <p>Endereço: Rua Exemplo, 123 — Cidade/UF</p>
        <p>Telefone: (00) 00000-0000</p>
        <p>E-mail: <a href="mailto:contato@ongexemplo.org">contato@ongexemplo.org</a></p>
      </address>
    </section>

    <aside aria-labelledby="doe">
      <h2 id="doe">Apoie nosso trabalho</h2>
      <p>Contribua com doações pontuais ou mensais. <a href="projetos.html#doacoes">Saiba como doar</a>.</p>
    </aside>
  </main>

  <footer class="container" role="contentinfo">
    <p>&copy; <span aria-hidden="true">2025</span> ONG Exemplo — Todos os direitos reservados.</p>
  </footer>

  <script src="assets/js/masks.js" defer></script>
</body>
</html>

--- FILE: projetos.html ---
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Projetos — ONG Exemplo</title>
  <meta name="description" content="Conheça os projetos da ONG Exemplo, oportunidades de voluntariado e como doar.">
  <link rel="stylesheet" href="assets/css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container header-brand">
      <h1>Projetos</h1>
    </div>
    <nav aria-label="Navegação principal">
      <div class="container">
        <ul>
          <li><a href="index.html">Início</a></li>
          <li><a href="projetos.html">Projetos</a></li>
          <li><a href="cadastro.html">Cadastre-se</a></li>
        </ul>
      </div>
    </nav>
  </header>

  <main class="container" id="main" tabindex="0">
    <section aria-labelledby="lista-projetos">
      <h2 id="lista-projetos">Nossos projetos</h2>
      <article aria-labelledby="proj-educacao">
        <h3 id="proj-educacao">Projeto Educação para Todos</h3>
        <p>Descrição breve do projeto, objetivos, público atendido e indicadores de impacto.</p>
        <figure>
          <img src="assets/img/projeto1-400w.webp" srcset="assets/img/projeto1-400w.webp 400w, assets/img/projeto1-800w.webp 800w" sizes="(max-width:720px) 100vw, 50vw" alt="Voluntários em atividade educativa" loading="lazy">
          <figcaption>Atividade educativa com crianças</figcaption>
        </figure>
      </article>

      <article aria-labelledby="proj-saude">
        <h3 id="proj-saude">Projeto Saúde Comunitária</h3>
        <p>Descrição do projeto de saúde, campanhas e como participar.</p>
      </article>
    </section>

    <section aria-labelledby="voluntariado">
      <h2 id="voluntariado">Voluntariado</h2>
      <p>Encontre oportunidades e inscreva-se. <a href="cadastro.html">Cadastre-se aqui</a> para receber notificações.</p>
    </section>

    <section id="doacoes" aria-labelledby="como-doar">
      <h2 id="como-doar">Como doar</h2>
      <p>Explicação sobre canais de doação, campanhas e prestação de contas.</p>
    </section>
  </main>

  <footer class="container">
    <p>Transparência e prestação de contas — relatórios disponíveis mediante solicitação.</p>
  </footer>

  <script src="assets/js/masks.js" defer></script>
</body>
</html>

--- FILE: cadastro.html ---
<!doctype html>
<html lang="pt-BR">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Cadastro — ONG Exemplo</title>
  <meta name="description" content="Cadastre-se como voluntário ou doador na ONG Exemplo.">
  <link rel="stylesheet" href="assets/css/styles.css">
</head>
<body>
  <header class="site-header">
    <div class="container header-brand">
      <h1>Cadastro</h1>
    </div>
    <nav aria-label="Navegação principal">
      <div class="container">
        <ul>
          <li><a href="index.html">Início</a></li>
          <li><a href="projetos.html">Projetos</a></li>
          <li><a href="cadastro.html">Cadastre-se</a></li>
        </ul>
      </div>
    </nav>
  </header>

  <main class="container" id="main" tabindex="0">
    <form action="#" method="post" novalidate aria-describedby="form-desc">
      <p id="form-desc">Preencha seus dados. Campos obrigatórios marcados com *</p>

      <fieldset>
        <legend>Dados pessoais</legend>
        <div class="field-group">
          <label for="nome">Nome completo *</label>
          <input id="nome" name="nome" type="text" required maxlength="100" placeholder="Seu nome completo">
        </div>

        <div class="grid-2">
          <div class="field-group">
            <label for="email">E‑mail *</label>
            <input id="email" name="email" type="email" required placeholder="exemplo@dominio.com">
          </div>

          <div class="field-group">
            <label for="cpf">CPF *</label>
            <input id="cpf" name="cpf" type="text" required maxlength="14" pattern="\d{3}\.\d{3}\.\d{3}-\d{2}" placeholder="000.000.000-00" aria-describedby="cpf-help">
            <small id="cpf-help">Formato: 000.000.000-00</small>
          </div>
        </div>

        <div class="grid-2">
          <div class="field-group">
            <label for="telefone">Telefone *</label>
            <input id="telefone" name="telefone" type="tel" required maxlength="16" pattern="\(?\d{2}\)?\s?\d{4,5}-?\d{4}" placeholder="(00) 90000-0000">
          </div>

          <div class="field-group">
            <label for="nascimento">Data de nascimento *</label>
            <input id="nascimento" name="nascimento" type="date" required max="2007-12-31">
          </div>
        </div>
      </fieldset>

      <fieldset>
        <legend>Endereço</legend>
        <div class="field-group">
          <label for="endereco">Endereço *</label>
          <input id="endereco" name="endereco" type="text" required maxlength="150">
        </div>

        <div class="grid-2">
          <div class="field-group">
            <label for="cep">CEP *</label>
            <input id="cep" name="cep" type="text" required pattern="\d{5}-\d{3}" maxlength="9" placeholder="00000-000">
          </div>

          <div class="field-group">
            <label for="cidade">Cidade *</label>
            <input id="cidade" name="cidade" type="text" required>
          </div>
        </div>

        <div class="field-group">
          <label for="estado">Estado *</label>
          <select id="estado" name="estado" required>
            <option value="">Selecione</option>
            <option value="AC">AC</option>
            <option value="AL">AL</option>
            <option value="AP">AP</option>
            <option value="AM">AM</option>
            <option value="BA">BA</option>
            <option value="CE">CE</option>
            <option value="DF">DF</option>
            <option value="ES">ES</option>
            <option value="GO">GO</option>
            <option value="MA">MA</option>
            <option value="MT">MT</option>
            <option value="MS">MS</option>
            <option value="MG">MG</option>
            <option value="PA">PA</option>
            <option value="PB">PB</option>
            <option value="PR">PR</option>
            <option value="PE">PE</option>
            <option value="PI">PI</option>
            <option value="RJ">RJ</option>
            <option value="RN">RN</option>
            <option value="RS">RS</option>
            <option value="RO">RO</option>
            <option value="RR">RR</option>
            <option value="SC">SC</option>
            <option value="SP">SP</option>
            <option value="SE">SE</option>
            <option value="TO">TO</option>
          </select>
        </div>
      </fieldset>

      <fieldset>
        <legend>Preferências</legend>
        <div class="field-group">
          <label for="interesse">Interesse</label>
          <select id="interesse" name="interesse">
            <option value="voluntariado">Voluntariado</option>
            <option value="doacao">Doação</option>
            <option value="parceria">Parceria</option>
            <option value="newsletter">Receber newsletter</option>
          </select>
        </div>

        <div class="field-group">
          <label for="mensagem">Mensagem (opcional)</label>
          <textarea id="mensagem" name="mensagem" rows="4" maxlength="1000" placeholder="Conte-nos o que motiva seu interesse..."></textarea>
        </div>
      </fieldset>

      <div class="field-group">
        <button type="submit">Enviar cadastro</button>
      </div>

      <div aria-live="polite" id="form-feedback" class="sr-only"></div>
    </form>
  </main>

  <footer class="container">
    <p>&copy; 2025 ONG Exemplo</p>
  </footer>

  <script src="assets/js/masks.js" defer></script>
</body>
</html>

--- END OF FILES ---
