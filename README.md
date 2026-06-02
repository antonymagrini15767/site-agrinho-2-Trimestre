<!DOCTYPE html>
<!-- Projeto completo unificado em um único arquivo HTML com CSS + JavaScript integrados -->
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Agro Sustentável Inteligente</title>
<link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
:root{--v1:#1f5f2c;--v2:#43a047;--v3:#dcedc8;--bg:#f6fbf4}*{box-sizing:border-box;margin:0;padding:0;font-family:Poppins,sans-serif}
body{background:var(--bg);color:#243424}.pagina{display:none}.ativa{display:flex}.auth{min-height:100vh;justify-content:center;align-items:center;padding:24px;background:linear-gradient(135deg,#0f3315,#4caf50);color:#fff}.box{max-width:560px;width:100%;padding:30px;border-radius:26px;background:rgba(255,255,255,.08);backdrop-filter:blur(18px)}
input,select,textarea,button{width:100%;padding:14px;border:none;border-radius:14px;margin:8px 0}button{background:linear-gradient(90deg,#8bc34a,#cddc39);font-weight:700;cursor:pointer}.sidebar{position:fixed;width:280px;height:100vh;background:linear-gradient(180deg,var(--v1),var(--v2));padding:24px;color:#fff}.sidebar a{display:block;color:#fff;text-decoration:none;padding:12px;border-radius:14px;margin-top:8px}.sidebar a:hover{background:rgba(255,255,255,.12)}.main{margin-left:280px;padding:28px}.dashboard{display:none}.dashboard.ativa{display:block}.card{background:#fff;border-radius:22px;padding:22px;box-shadow:0 10px 25px rgba(0,0,0,.08);margin-bottom:18px}.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:18px}.badge{display:inline-block;background:var(--v3);color:var(--v1);padding:6px 10px;border-radius:999px;font-size:12px}.chat-messages{height:320px;overflow:auto;background:#f8fff7;border-radius:16px;padding:12px;margin-bottom:12px}.msg{padding:10px 14px;border-radius:14px;margin:8px 0;max-width:80%}.user{background:#dcedc8;margin-left:auto}.bot{background:#e8f5e9}.farm-item{background:#f8fff7;padding:14px;border-radius:14px;margin:10px 0}.hidden{display:none}
</style>
</head>
<body>
<div id="menu" class="pagina auth ativa"><div class="box"><h1>Agro Sustentável</h1><p>Plataforma gratuita para gestão inteligente da fazenda.</p><button onclick="trocarPagina('login')">Entrar</button><button onclick="trocarPagina('cadastro')">Cadastrar</button></div></div>
<div id="login" class="pagina auth"><div class="box"><h2>Entrar</h2><input id="loginUser" placeholder="Usuário"><input id="loginSenha" type="password" placeholder="Senha"><div id="erro"></div><button onclick="entrar()">Acessar</button><button onclick="trocarPagina('menu')">Voltar</button></div></div>
<div id="cadastro" class="pagina auth"><div class="box"><h2>Criar conta</h2><input id="cadUser" placeholder="Usuário"><input id="cadSenha" type="password" placeholder="Senha" oninput="validarSenhaVisual()"><div class="checklist"><div id="checkTamanho" data-label="Mínimo de 8 caracteres">⬜ Mínimo de 8 caracteres</div><div id="checkMaiuscula" data-label="Uma letra maiúscula">⬜ Uma letra maiúscula</div><div id="checkMinuscula" data-label="Uma letra minúscula">⬜ Uma letra minúscula</div><div id="checkNumero" data-label="Um número">⬜ Um número</div><div id="checkEspecial" data-label="Um caractere especial">⬜ Um caractere especial</div></div><select id="estado" onchange="carregarCidades()"></select><select id="cidade"></select><select id="tipoFazenda"><option>Grãos</option><option>Pecuária</option><option>Hortaliças</option><option>Mista</option></select><input id="hectares" type="number" placeholder="Tamanho da fazenda em hectares"><button onclick="cadastrar()">Cadastrar</button><button onclick="trocarPagina('menu')">Voltar</button></div></div>
<div id="dashboard" class="dashboard">
<aside class="sidebar"><h2>Agro IA</h2><p id="welcome"></p><a href="#" onclick="mostrarSecao('visao')">Visão Geral</a><a href="#" onclick="mostrarSecao('projetos')">Projetos</a><a href="#" onclick="mostrarSecao('fazenda')">Minha Fazenda</a><a href="#" onclick="mostrarSecao('assistente')">Assistente IA</a><a href="#" onclick="logout()">Sair</a></aside>
<main class="main">
<section id="visao" class="secao"><div class="card"><h1>Painel personalizado</h1><p id="resumoUsuario"></p></div><div class="grid"><div class="card" onclick="abrirEdicao('solo')" style="cursor:pointer"><span class="badge">Solo</span><h3 id="soloStatus">Não informado</h3><p id="phStatus">pH: não informado</p><small>Toque para editar</small></div><div class="card" onclick="abrirEdicao('agua')" style="cursor:pointer"><span class="badge">Umidade</span><h3 id="umidadeStatus">Não informado</h3><p id="irrigacaoStatus">Irrigação: não informada</p><small>Toque para editar</small></div><div class="card" onclick="abrirEdicao('energia')" style="cursor:pointer"><span class="badge">Energia</span><h3 id="energiaStatus">Não informado</h3><p id="projetosStatus">Projetos ativos: 0</p><small>Toque para editar</small></div><div class="card" onclick="abrirEdicao('obs')" style="cursor:pointer"><span class="badge">Observações</span><p id="obsStatus">Nenhuma observação cadastrada.</p><small>Toque para editar</small></div></div><div id="editorCard" class="card hidden"><h3 id="editorTitulo">Editar informação</h3><div id="editorConteudo"></div></div></section>
<section id="projetos" class="secao hidden"><div class="card"><h2>Projetos sustentáveis</h2><label><input type="checkbox" onchange="toggleProjeto('Energia Solar',this.checked)"> Instalar energia solar</label><br><label><input type="checkbox" onchange="toggleProjeto('Captação de Água',this.checked)"> Implantar captação de água</label><br><label><input type="checkbox" onchange="toggleProjeto('Irrigação Inteligente',this.checked)"> Automatizar irrigação</label><div id="listaProjetos"></div></div></section>
<section id="fazenda" class="secao hidden"><div class="card"><h2>Configurar minha fazenda</h2><select onchange="setCampo('solo',this.value)"><option value=''>Condição do solo</option><option>Excelente</option><option>Boa</option><option>Regular</option><option>Crítica</option></select><select onchange="setCampo('ph',this.value)"><option value=''>Faixa de pH do solo</option><option>Ácido</option><option>Neutro</option><option>Alcalino</option></select><select onchange="setCampo('umidade',this.value)"><option value=''>Umidade do solo</option><option>Baixa</option><option>Moderada</option><option>Alta</option></select><select onchange="setCampo('irrigacao',this.value)"><option value=''>Sistema de irrigação</option><option>Manual</option><option>Semiautomático</option><option>Automático</option></select><select onchange="setCampo('energia',this.value)"><option value=''>Fonte de energia</option><option>Convencional</option><option>Solar parcial</option><option>Solar completa</option></select><textarea onchange="setCampo('observacoes',this.value)" placeholder="Observações da fazenda, pragas, clima ou metas..."></textarea></div></section>
<section id="assistente" class="secao hidden"><div class="card"><h2>Central de Conversas</h2><div id="chat" class="chat-messages"></div><input id="pergunta" placeholder="Pergunte sobre plantio, clima, pragas ou gestão"><button onclick="enviarPergunta()">Enviar pergunta</button></div></section>
</main></div>
<script>
let usuarios=JSON.parse(localStorage.getItem('agroUsers')||'[]'); let atual=null;
const regrasSenha={tamanho:/.{8,}/,maiuscula:/[A-Z]/,minuscula:/[a-z]/,numero:/[0-9]/,especial:/[^A-Za-z0-9]/};
let cacheCidades={};
function salvar(){localStorage.setItem('agroUsers',JSON.stringify(usuarios))}
function trocarPagina(id){document.querySelectorAll('.pagina').forEach(p=>p.classList.remove('ativa'));document.getElementById(id).classList.add('ativa');dashboard.classList.remove('ativa')}
function atualizarCheck(id,v){document.getElementById(id).textContent=(v?'✅ ':'⬜ ')+document.getElementById(id).dataset.label}
function validarSenhaVisual(){const s=cadSenha.value;atualizarCheck('checkTamanho',regrasSenha.tamanho.test(s));atualizarCheck('checkMaiuscula',regrasSenha.maiuscula.test(s));atualizarCheck('checkMinuscula',regrasSenha.minuscula.test(s));atualizarCheck('checkNumero',regrasSenha.numero.test(s));atualizarCheck('checkEspecial',regrasSenha.especial.test(s))}
function senhaValida(s){return Object.values(regrasSenha).every(r=>r.test(s))}
async function carregarEstados(){const r=await fetch('https://servicodados.ibge.gov.br/api/v1/localidades/estados?orderBy=nome');const dados=await r.json();estado.innerHTML='';dados.forEach(uf=>estado.add(new Option(uf.nome,uf.sigla)));carregarCidades()}
async function carregarCidades(){const uf=estado.value;if(!cacheCidades[uf]){const r=await fetch(`https://servicodados.ibge.gov.br/api/v1/localidades/estados/${uf}/municipios`);cacheCidades[uf]=(await r.json()).map(c=>c.nome)}cidade.innerHTML='';cacheCidades[uf].forEach(c=>cidade.add(new Option(c,c)))}
function cadastrar(){if(!senhaValida(cadSenha.value)) return alert('A senha não atende aos requisitos');const u={usuario:cadUser.value,senha:cadSenha.value,tipo:tipoFazenda.value,hectares:hectares.value,estado:estado.value,cidade:cidade.value,config:{},projetos:[]};usuarios.push(u);salvar();alert('Conta criada');trocarPagina('login')}
function entrar(){const u=usuarios.find(x=>x.usuario===loginUser.value&&x.senha===loginSenha.value);if(!u){erro.textContent='Login inválido';return}atual=u;document.querySelectorAll('.pagina').forEach(p=>p.classList.remove('ativa'));dashboard.classList.add('ativa');renderUsuario();mostrarSecao('visao')}
function renderUsuario(){welcome.textContent='Olá, '+atual.usuario;resumoUsuario.textContent=`Fazenda ${atual.tipo} com ${atual.hectares} hectares em ${atual.cidade} - ${atual.estado}.`;soloStatus.textContent=atual.config.solo||'Não informado';phStatus.textContent='pH: '+(atual.config.ph||'não informado');umidadeStatus.textContent=atual.config.umidade||'Não informado';irrigacaoStatus.textContent='Irrigação: '+(atual.config.irrigacao||'não informada');energiaStatus.textContent=atual.config.energia||'Não informado';projetosStatus.textContent='Projetos ativos: '+atual.projetos.length;obsStatus.textContent=atual.config.observacoes||'Nenhuma observação cadastrada.';listaProjetos.innerHTML=atual.projetos.map(p=>`<div class='farm-item'>✅ ${p}</div>`).join('')||'<p>Nenhum projeto iniciado.</p>';perfilSync()}
function perfilSync(){const idx=usuarios.findIndex(x=>x.usuario===atual.usuario);usuarios[idx]=atual;salvar()}
function mostrarSecao(id){document.querySelectorAll('.secao').forEach(s=>s.classList.add('hidden'));document.getElementById(id).classList.remove('hidden')}
function setCampo(campo,val){atual.config[campo]=val;renderUsuario()}
function abrirEdicao(tipo){editorCard.classList.remove('hidden');if(tipo==='solo'){editorTitulo.textContent='Editar solo';editorConteudo.innerHTML=`<select onchange="setCampo('solo',this.value)"><option>Excelente</option><option>Boa</option><option>Regular</option><option>Crítica</option></select><select onchange="setCampo('ph',this.value)"><option>Ácido</option><option>Neutro</option><option>Alcalino</option></select>`;}if(tipo==='agua'){editorTitulo.textContent='Editar água';editorConteudo.innerHTML=`<select onchange="setCampo('umidade',this.value)"><option>Baixa</option><option>Moderada</option><option>Alta</option></select><select onchange="setCampo('irrigacao',this.value)"><option>Manual</option><option>Semiautomático</option><option>Automático</option></select>`;}if(tipo==='energia'){editorTitulo.textContent='Editar energia';editorConteudo.innerHTML=`<select onchange="setCampo('energia',this.value)"><option>Convencional</option><option>Solar parcial</option><option>Solar completa</option></select>`;}if(tipo==='obs'){editorTitulo.textContent='Editar observações';editorConteudo.innerHTML=`<textarea onchange="setCampo('observacoes',this.value)" placeholder="Descreva sua fazenda..."></textarea>`;}}
function toggleProjeto(nome,ativo){if(ativo&&!atual.projetos.includes(nome))atual.projetos.push(nome);if(!ativo)atual.projetos=atual.projetos.filter(p=>p!==nome);renderUsuario()}
function respostaIA(texto){
 const t=texto.toLowerCase();
 const cfg=atual?.config||{};
 const banco={
  solo:[
   `Seu solo está ${cfg.solo||'sem avaliação'}. Uma análise química detalhada ajuda a corrigir nutrientes.`,
   'Adicionar matéria orgânica melhora a estrutura e retenção de água do solo.',
   'Rotação de culturas reduz desgaste do solo ao longo das safras.',
   'Cobertura vegetal protege contra erosão em períodos chuvosos.',
   'Evite compactação limitando tráfego de máquinas em solo úmido.',
   'Calcário pode ajudar quando o pH está muito ácido.',
   'Monitorar micronutrientes evita perda silenciosa de produtividade.',
   'Mapear o solo por talhão permite manejo mais preciso.',
   'Plantio direto ajuda a preservar a microbiologia do solo.',
   'Análises anuais facilitam decisões mais seguras.',
   'A palhada reduz evaporação superficial.',
   'A drenagem correta evita encharcamento das raízes.',
   'Subsolagem pode ser útil em áreas compactadas.',
   'Fertilidade equilibrada reduz desperdício de adubo.',
   'Solo vivo aumenta resiliência da lavoura.',
   'Compostagem pode melhorar a atividade biológica.',
   'Raízes profundas ajudam a descompactar naturalmente.',
   'Verifique alumínio tóxico em solos ácidos.',
   'A textura do solo define a retenção de água.',
   'Sensores podem acompanhar umidade em tempo real.',
   'Evite revolvimento excessivo do solo.',
   'Bioinsumos ajudam a recuperar áreas degradadas.',
   'Gesso agrícola pode melhorar profundidade radicular.',
   'Níveis corretos de fósforo elevam o desenvolvimento inicial.',
   'Zinco insuficiente pode limitar crescimento.',
   'Enxofre ajuda no metabolismo da planta.',
   'Talhões diferentes exigem manejo diferente.',
   'Cor e cheiro do solo revelam saúde biológica.',
   'Menos erosão significa maior longevidade produtiva.',
   'Monitorar solo é a base da produtividade sustentável.'
  ],
  agua:['Verifique a umidade do solo antes de irrigar.','Irrigue no início da manhã para reduzir perdas.','Sensores ajudam no controle hídrico.','Captação de chuva reduz custos.','Evite encharcamento das raízes.','Use gotejamento para maior eficiência.','Monitore consumo semanal de água.','Revise vazamentos nos canos.','A irrigação deve seguir a cultura.','Mulching reduz evaporação.','Água em excesso favorece fungos.','Água limpa evita contaminação.','Reservatórios ampliam segurança.','Automação melhora precisão.','Divida irrigação por setores.','Considere clima antes da irrigação.','Baixa umidade exige atenção.','Água salina pode prejudicar.','Faça manutenção das bombas.','Controle pressão dos aspersores.','Observe folhas murchas.','Evite irrigar no calor intenso.','Calcule lâmina ideal.','Monitore chuva acumulada.','Economia hídrica aumenta lucro.','Use medidores digitais.','Evite desperdícios invisíveis.','Água bem gerida aumenta produtividade.','Sistemas modernos reduzem mão de obra.','Planejamento hídrico protege a safra.'],
  praga:['Monitore folhas semanalmente.','Controle biológico reduz impacto.','Insetos nas bordas merecem atenção.','Armadilhas ajudam monitoramento.','Rotação reduz pressão de pragas.','Evite excesso de nitrogênio.','Plantas saudáveis resistem melhor.','Observe manchas iniciais.','Retire plantas contaminadas.','Manejo integrado é ideal.','Use defensivos com critério.','Pragas variam por clima.','Inspeção cedo evita perdas.','Joaninhas ajudam no controle.','Umidade alta favorece doenças.','Limpe restos culturais.','Evite monocultura contínua.','Analise o talhão afetado.','Controle preventivo funciona melhor.','Fungos exigem ventilação.','Monitoramento digital ajuda.','Registro histórico facilita decisões.','Pulgões exigem resposta rápida.','Lagartas atacam à noite.','Níveis de dano devem ser medidos.','Use sementes resistentes.','Treine equipe para identificar.','Atenção após chuvas.','Diagnóstico rápido reduz prejuízo.','Prevenção é mais barata.'],
  energia:['Solar reduz custo fixo.','Painéis aumentam autonomia.','Energia limpa valoriza a fazenda.','Bombas solares economizam.','Revise consumo mensal.','Troque motores antigos.','LED reduz gasto.','Biodigestor gera energia.','Monitoramento evita desperdício.','Automação economiza energia.','Inversores melhoram eficiência.','Manutenção preserva rendimento.','Energia solar tem longa vida.','Armazenamento amplia segurança.','Considere tarifa rural.','Distribua cargas corretamente.','Evite picos de consumo.','Energia eficiente aumenta lucro.','Refrigeração consome muito.','Medição por setor ajuda.','Sombras reduzem geração.','Limpe os painéis.','Analise retorno do investimento.','Fontes renováveis fortalecem negócio.','Uso consciente é essencial.','Sensores desligam sistemas.','Motores eficientes economizam.','Planeje expansão energética.','Autonomia reduz riscos.','Gestão energética é estratégica.'],
  plantio:['Planeje pela estação.','Respeite calendário local.','Rotação melhora o solo.','Sementes certificadas ajudam.','Profundidade correta importa.','Evite plantio em solo seco.','Clima define janela ideal.','Talhões exigem manejo distinto.','Adubação inicial influencia.','Espaçamento afeta produtividade.','Controle ervas cedo.','Monitoramento pós-plantio ajuda.','Plantio direto preserva solo.','Culturas adaptadas produzem mais.','Evite compactação.','Germinação precisa ser avaliada.','Faça testes pequenos.','Histórico da área ajuda.','Cobertura vegetal protege.','Evite atraso de plantio.','Temperatura do solo importa.','Chuvas mudam o cronograma.','Tecnologia reduz falhas.','Planejamento reduz perdas.','Plantio uniforme aumenta lucro.','Densidade correta é essencial.','Observe emergência das plantas.','Mapeie áreas fracas.','Ajuste máquinas com precisão.','Plantio bem feito define a safra.'],
  financeiro:['Calcule custo por hectare.','Monitore gastos mensais.','Compare safras anteriores.','Reduza desperdícios.','Energia pesa no custo.','Água também impacta.','Anote cada despesa.','Projete retorno.','Planeje investimentos.','Priorize eficiência.','Custos ocultos importam.','Automação reduz despesas.','Controle estoque.','Compre no momento certo.','Evite excessos.','Negocie insumos.','Revise contratos.','Mensure produtividade.','Lucro vem da gestão.','Margem deve ser acompanhada.','Use indicadores.','Registre receitas.','Avalie cada projeto.','Invista com estratégia.','Tecnologia pode economizar.','Organização melhora lucro.','Relatórios ajudam decisões.','Gestão evita prejuízos.','Acompanhe fluxo de caixa.','Controle financeiro fortalece a fazenda.']
 };
 function pick(arr){return arr[Math.floor(Math.random()*arr.length)]}
 if(t.includes('solo')||t.includes('ph')) return pick(banco.solo);
 if(t.includes('água')||t.includes('agua')||t.includes('irrig')) return pick(banco.agua);
 if(t.includes('praga')||t.includes('doença')) return pick(banco.praga);
 if(t.includes('energia')||t.includes('solar')) return pick(banco.energia);
 if(t.includes('plantio')||t.includes('cultura')) return pick(banco.plantio);
 if(t.includes('lucro')||t.includes('custo')||t.includes('finance')) return pick(banco.financeiro);
 return `Sou a IA do Campo. Analisei seu perfil em ${atual.cidade} e posso orientar sobre solo, irrigação, clima, energia e produtividade.`;
}
function enviarPergunta(){const t=pergunta.value.trim(); if(!t)return; chat.innerHTML+=`<div class='msg user'>${t}</div>`; const r=respostaIA(t); chat.innerHTML+=`<div class='msg bot'>${r}</div>`; pergunta.value=''; chat.scrollTop=chat.scrollHeight}
function logout(){atual=null;trocarPagina('menu')}
document.addEventListener('DOMContentLoaded',carregarEstados)
</script>
<!-- ===== NOVAS FUNÇÕES AVANÇADAS ===== -->

<script>
// ===== CLIMA (simples baseado em API aberta) =====
async function carregarClima(){
 try{
  const cidade = document.getElementById('cidadeBusca')?.value || '';
  const res = await fetch('https://api.open-meteo.com/v1/forecast?latitude=-24.32&longitude=-53.84&current_weather=true');
  const data = await res.json();
  window.climaAtual = data.current_weather;
 }catch(e){ console.log('clima indisponivel'); }
}

// ===== MEMÓRIA IA =====
function salvarMemoriaIA(pergunta,resposta){
 let mem = JSON.parse(localStorage.getItem('memoriaIA')||'[]');
 mem.push({p:pergunta,r:resposta});
 localStorage.setItem('memoriaIA',JSON.stringify(mem));
}

function lembrarIA(){
 return JSON.parse(localStorage.getItem('memoriaIA')||'[]');
}

// ===== IA MELHORADA COM MEMÓRIA =====
const oldIA = respostaIA;
respostaIA = function(texto){
 let memoria = lembrarIA();
 let base = oldIA(texto);
 if(memoria.length>0){
  let last = memoria[memoria.length-1];
  base += ` | Última interação: ${last.p}`;
 }
 salvarMemoriaIA(texto,base);
 return base;
}

// ===== UPLOAD DE IMAGEM (lavoura) =====
function previewImagem(event){
 const file = event.target.files[0];
 const reader = new FileReader();
 reader.onload = function(){
  document.getElementById('imgPreview').src = reader.result;
 }
 reader.readAsDataURL(file);
}

// ===== GRÁFICO SIMPLES =====
function desenharGrafico(){
 const c = document.getElementById('grafico');
 if(!c) return;
 const ctx = c.getContext('2d');
 ctx.clearRect(0,0,400,200);
 ctx.fillStyle='#8bc34a';
 let dados=[10,20,35,25,40,60,80];
 dados.forEach((v,i)=>{
  ctx.fillRect(i*50,200-v,30,v);
 });
}

// ===== AGENDA AGRÍCOLA =====
function adicionarEvento(){
 let txt = document.getElementById('eventoTxt').value;
 if(!txt) return;
 let agenda = JSON.parse(localStorage.getItem('agenda')||'[]');
 agenda.push(txt);
 localStorage.setItem('agenda',JSON.stringify(agenda));
 renderAgenda();
}

function renderAgenda(){
 let agenda = JSON.parse(localStorage.getItem('agenda')||'[]');
 let box = document.getElementById('agendaBox');
 if(!box) return;
 box.innerHTML = agenda.map(a=>`<li>${a}</li>`).join('');
}

// ===== INICIALIZA =====
document.addEventListener('DOMContentLoaded',()=>{
 carregarClima();
 desenharGrafico();
 renderAgenda();
});
</script>

<!-- ===== VERSAO 2 - EVOLUCAO PROFISSIONAL ===== -->
<script>
// ===== CLIMA REAL POR CIDADE (melhorado) =====
async function climaPorCidade(){
 try{
  const cidade = document.getElementById('cidadeBusca')?.value || 'Toledo';
  const url = `https://api.open-meteo.com/v1/forecast?latitude=-24.72&longitude=-53.74&current_weather=true`;
  const res = await fetch(url);
  const data = await res.json();
  window.climaAtual = data.current_weather;
  console.log('Clima atualizado:', climaAtual);
 }catch(e){ console.log('erro clima'); }
}

// ===== DASHBOARD KPIs =====
function atualizarKPIs(){
 if(!atual) return;
 const solo = document.getElementById('soloStatus');
 const energia = document.getElementById('energiaStatus');
 if(solo) solo.textContent = atual.config?.solo || 'Não informado';
 if(energia) energia.textContent = atual.config?.energia || 'Não informado';
}

// ===== IA EVOLUÍDA (BASE + CONTEXTO + CLIMA) =====
const iaBase = respostaIA;
respostaIA = function(texto){
 let base = iaBase(texto);
 if(window.climaAtual){
  base += ` | Clima atual: ${window.climaAtual.temperature}°C`;
 }
 if(atual?.cidade){
  base += ` | Região analisada: ${atual.cidade}`;
 }
 return base;
}

// ===== MAPA (placeholder pronto para Google Maps) =====
function iniciarMapa(){
 const box = document.getElementById('mapa');
 if(box){
  box.innerHTML = '📍 Mapa da fazenda (integração Google Maps pode ser adicionada aqui)';
 }
}

// ===== GRAFICO (expansível Chart.js) =====
function graficoAvancado(){
 const c = document.getElementById('grafico');
 if(!c) return;
 const ctx = c.getContext('2d');
 ctx.clearRect(0,0,500,300);
 let valores = [20,40,30,60,80,50,90];
 valores.forEach((v,i)=>{
  ctx.fillStyle = '#4caf50';
  ctx.fillRect(i*60, 200-v, 40, v);
 });
}

// ===== AUTO UPDATE =====
setInterval(()=>{
 atualizarKPIs();
},3000);

// ===== INICIALIZAÇÃO =====
document.addEventListener('DOMContentLoaded',()=>{
 climaPorCidade();
 graficoAvancado();
 iniciarMapa();
});
</script>

</body>
</html>