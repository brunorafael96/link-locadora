==============================================================
  LINK LOCADORA — Sistema de Controle Diário de Carros e Serviços
==============================================================

Sistema web para organizar o dia a dia da Link Locadora (Link Blindados):
serviços agendados, motoristas e frota de carros, com dados sincronizados
em tempo real entre todos os usuários autorizados.


--------------------------------------------------------------
1. VISÃO GERAL
--------------------------------------------------------------
- Arquivo único: index.html (HTML + CSS + JavaScript, sem build)
- Hospedagem: GitHub Pages
- Banco de dados: Firebase Cloud Firestore (tempo real)
- Login: Firebase Authentication (e-mail e senha)
- Uso interno e restrito (acesso apenas para usuários cadastrados)
- Funciona no computador e no celular (layout responsivo em cartões)


--------------------------------------------------------------
2. FUNCIONALIDADES
--------------------------------------------------------------

[ Aba "Serviços do Dia" ]
- Cadastro de serviços com: cliente, dia, horário, carro, tipo de
  serviço (Transfer, Disposição, Orçamento), itinerário, motorista,
  status, observações (até 100 caracteres), valor (R$) e status de
  pagamento (Pago / Não pago)
- Serviços agrupados por dia, ordenados por horário
- Ordem dos grupos: hoje primeiro, depois dias futuros, e dias que
  já passaram no final
- Resumo do dia: quantidade de serviços e motoristas em campo,
  com chips "Carro → Motorista"
- Filtro por mês (seletor próprio de mês/ano, compatível com Safari
  e iOS) e opção "Todos os meses"
- Busca por cliente, carro, serviço, itinerário, motorista ou
  observações
- Exportação para CSV (compatível com Excel, com acentuação)
- Avanço automático de status (verificado a cada minuto):
    Agendado     -> Em andamento  (quando chega o horário de início)
    Em andamento -> Concluído     (quando o dia do serviço passa)
  O status nunca retrocede automaticamente.
- Badges coloridos para status e pagamento
- Contador de serviços pendentes de hoje no título da aba e no
  favicon do navegador

[ PDF da Ordem de Serviço ]
- Gerado automaticamente ao cadastrar um serviço novo
- Também disponível pelo botão "PDF" em qualquer serviço já salvo
- Conteúdo: logo e nome "Link Blindados", dados do serviço, dados e
  foto do carro, nome, telefone e foto do motorista
- Ajusta o tamanho das fotos para caber sempre em uma página A4
- Opções: Compartilhar (WhatsApp, e-mail etc. pelo menu nativo do
  aparelho), Imprimir e Baixar

[ Aba "Motoristas" ]
- Cadastro com nome, telefone, onde mora, categoria da carteira
  (B ou D) e foto (recortada em quadrado e comprimida)
- Botão "Chamar" que abre conversa no WhatsApp (adiciona +55
  automaticamente quando necessário)
- Indicação do carro que o motorista está usando hoje

[ Aba "Carros" ]
- Cadastro com modelo, placa, cor, ano e foto (recorte 4:3)
- Os carros cadastrados alimentam a lista do formulário de serviço
  no formato "Modelo - PLACA"

[ Geral ]
- Tela de login com recuperação de senha por e-mail
- Tema claro/escuro (segue o sistema e pode ser alternado manualmente;
  a escolha fica salva no navegador)
- Confirmação personalizada antes de excluir qualquer item
- Edição de qualquer registro, motorista ou carro


--------------------------------------------------------------
3. TECNOLOGIAS
--------------------------------------------------------------
- HTML5, CSS3 e JavaScript puro (sem frameworks)
- Firebase SDK 10.14.1 (compat): App, Firestore e Auth
- jsPDF 2.5.1 (geração de PDF no navegador)
- Fonte Manrope (Google Fonts)


--------------------------------------------------------------
4. ESTRUTURA DO BANCO (FIRESTORE)
--------------------------------------------------------------
Coleção "registros"
  cliente, dia (AAAA-MM-DD), horario (HH:MM), carro, servico,
  itinerario, motorista, status, observacoes, valor (número),
  pagamento, criadoEm (timestamp)

Coleção "motoristas"
  nome, telefone, endereco, carteira, foto (base64), criadoEm

Coleção "carros"
  modelo, placa, cor, ano, foto (base64), criadoEm

Observação: as fotos são salvas comprimidas em base64 dentro do
próprio documento (limite do Firestore: 1 MB por documento).


--------------------------------------------------------------
5. COMO PUBLICAR NO GITHUB PAGES
--------------------------------------------------------------
1. Crie um repositório no GitHub e envie o arquivo index.html
   para a raiz.
2. Vá em Settings > Pages.
3. Em "Source", escolha "Deploy from a branch", selecione a branch
   "main" e a pasta "/ (root)". Salve.
4. Após alguns minutos o site fica disponível em:
   https://SEU-USUARIO.github.io/NOME-DO-REPOSITORIO/
5. No Console do Firebase, em Authentication > Settings >
   Authorized domains, adicione o domínio "SEU-USUARIO.github.io".


--------------------------------------------------------------
6. CONFIGURAÇÃO DO FIREBASE
--------------------------------------------------------------
1. Crie um projeto em https://console.firebase.google.com
2. Ative o Cloud Firestore.
3. Ative o Authentication com o provedor "E-mail/senha" e crie
   manualmente as contas dos usuários autorizados.
4. Copie a configuração web do projeto (firebaseConfig) para o
   index.html.
5. Crie os índices se o Firestore solicitar (ordenação por "dia"
   e por "criadoEm").


--------------------------------------------------------------
7. SEGURANÇA
--------------------------------------------------------------
A chave "apiKey" do Firebase que aparece no index.html não é uma
senha: ela apenas identifica o projeto e é pública por natureza.
O que protege os dados são as Regras de Segurança do Firestore.
Recomenda-se, no mínimo, liberar leitura e escrita apenas para
usuários logados:

  rules_version = '2';
  service cloud.firestore {
    match /databases/{database}/documents {
      match /{document=**} {
        allow read, write: if request.auth != null;
      }
    }
  }

Para mais segurança, restrinja também pelo e-mail ou UID dos
usuários autorizados e desative o cadastro público de contas.


--------------------------------------------------------------
8. AUTOR
--------------------------------------------------------------
Desenvolvido por Bruno Rafael para uso interno da Link Locadora
de Veículos (Link Blindados) — Rio de Janeiro, Brasil.

==============================================================
