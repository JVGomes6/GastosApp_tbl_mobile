RELATÓRIO TÉCNICO: APLICATIVO "GASTOS APP"
Equipe: Gabriel Trichez Dias, João Vitor de Oliveira Gomes, Rafael Aranão Corrêa, Samuel Wesley Magalhães
1. INTRODUÇÃO
O relatório apresenta a estrutura do aplicativo Gastos App, uma aplicação Android desenvolvida por nossa equipe no ambiente do MIT App Inventor com o propósito de auxiliar no controle financeiro pessoal. O aplicativo possui funcionalidades que permitem o cadastro de contas bancárias, o registro de transações financeiras classificadas como entradas ou saídas, a atualização local e automática do saldo, a utilização da câmera do dispositivo para anexar fotos de comprovantes, e a listagem de lançamentos com filtros por datas e categorias. O foco do nosso projeto foi a construção de uma estrutura funcional básica operando com persistência de dados local.

2. FUNCIONAMENTO DO APLICATIVO
O fluxo principal de utilização foi desenvolvido com navegação simples, distribuída em quatro telas principais:
Tela Inicial (Screen1): Apresenta o menu principal de navegação contendo botões que direcionam o usuário para o cadastro de contas, lançamentos ou área de relatórios(figura1), (figura2), (figura3).

figura 1

figura 2

figura 3



Cadastro de Contas (TelaContas): Permite ao usuário registrar uma instituição bancária informando o nome do banco, uma senha eletrônica e o saldo inicial da conta(figura4).(figura5).

figura 4

figura 5


Registro de Lançamentos (TelaLancamentos): O usuário seleciona uma conta já cadastrada, define o tipo de transação (Entrada ou Saída), escolhe a data (através de um seletor visual), informa o valor, seleciona uma categoria e adiciona uma descrição. O usuário também pode acionar a câmera nativa do celular para capturar uma foto de um comprovante, que é exibida na tela. Após registrar a transação, o sistema atualiza o saldo da respectiva conta(figura6),(figura7),(figura8).

figura 6

figura7

figura8




Relatórios (TelaRelatorios): Responsável por exibir uma listagem textual dos lançamentos. O usuário pode gerar filtros, selecionando uma data inicial, uma data final e uma categoria específica, para visualizar apenas as informações desejadas(figura9),(figura10).

figura 9

figura 10


Histórico de Lançamentos (TelaHistorico): Tela dedicada a exibir a lista completa de transações. Ao tocar em um item, o usuário visualiza os detalhes isolados e ganha acesso aos botões de Editar ou Excluir (figura11), (figura12) .

figura 11

figura 12




3. ARQUITETURA E ESTRUTURA DE DADOS
O gerenciamento de dados do Gastos App baseia-se em uma estrutura simples, utilizando o armazenamento local através do componente TinyDB para a operação principal e o componente CloudDB para a realização de backups pontuais. A estrutura de dados utiliza listas e sublistas (arrays):
Tag "contas": Armazena os dados das contas. Os registros formam uma lista onde cada item é uma sublista estruturada nos índices: 1- Nome do Banco, 2- Senha Eletrônica, 3- Saldo Inicial e 4- Saldo Atual.
Tag "lancamentos": Armazena o histórico financeiro. O registro é composto por sublistas nos índices: 1- Conta selecionada, 2- Tipo (Entrada/Saída), 3- Data, 4- Valor, 5- Categoria, 6- Descrição, 7- Caminho da imagem do comprovante.
Relacionamento e Atualização (CRUD): Para registrar um novo lançamento, os blocos identificam a posição da conta na lista, recuperam o Saldo Atual (índice 4), realizam a soma ou subtração e sobrescrevem a lista no TinyDB. Nas operações de exclusão ou edição de dados já gravados, a lógica identifica a transação anterior e realiza o estorno no saldo da conta vinculada (operação matemática inversa) antes de salvar a lista atualizada.
Armazenamento em Nuvem (Backup): Implementamos o CloudDB como um mecanismo de backup manual, sem sincronização em tempo real. O usuário digita um código no aplicativo, e os blocos concatenam esse texto às tags originais (por exemplo, contas_1234). O sistema envia as listas completas do TinyDB para o servidor do CloudDB ou as recupera, substituindo os dados locais.

4. COMPONENTES DO MIT APP INVENTOR EMPREGADOS
A tabela abaixo consolida os componentes estruturais utilizados no desenvolvimento do nosso projeto:
Categoria
Componentes Empregados
Finalidade no Aplicativo
 
Interface de Usuário (UI)
TextBox, PasswordTextBox, Button, Label, ListView, DatePicker, Spinner, ListPicker
Entrada de texto numérico/alfanumérico, seleção padronizada de datas e listas suspensas (categorias), e exibição de informações de relatório em lista.
Armazenamento
TinyDB, CloudDB
Persistência e recuperação de dados locais (contas e lançamentos); o CloudDB realiza o backup baseado no código do usuário.
Sensores e Mídia
Camera, Image,  Sharing
Câmera para digitalizar comprovantes. O componente Sharing exporta o relatório financeiro textual para outros aplicativos.
Alertas e Interação
Notifier
Exibir alertas (avisos) em caso de erro no preenchimento dos campos ou confirmações de sucesso.


5. REQUISITOS FUNCIONAIS
Abaixo apresentamos o status de implementação dos requisitos exigidos pela atividade:
Requisito Funcional
Situação
Justificativa / Observação
 
RF01 - Cadastro de contas
Implementado
Ocorre na TelaContas, salvando os dados no TinyDB.
RF02 - Registro de lançamentos
Implementado
Ocorre na TelaLancamentos, registrando entradas/saídas.
RF03 - Atualização de saldo
Implementado
Rotina matemática implementada junto à gravação do lançamento.
RF04 - Persistência entre sessões
Implementado
Uso do banco local TinyDB em todas as operações de dados.
RF05 - Sincronização/backup
Implementado
Contas e lançamentos podem ser enviados ao CloudDB e posteriormente restaurados utilizando um código.  
RF06 - Exportação de relatório
Implementado
Os relatórios podem ser compartilhados utilizando o componente Sharing. 
RF07 - Recurso do dispositivo
Implementado
Uso do componente de Câmera na tela de lançamentos.
RF08 - CRUD de Lançamentos
Implementado
O aplicativo permite cadastrar, consultar, editar e excluir lançamentos. 


6. REQUISITOS NÃO FUNCIONAIS E DECISÕES DE PROJETO
RNF01 - Segurança e Privacidade: Para proporcionar discrição visual simples e de fácil leitura no cadastro, adotamos o componente PasswordTextBox, que mascara os caracteres digitados na tela. Como a aplicação não dispõe de métodos de criptografia adicionais, as informações são gravadas em texto puro no TinyDB.
RNF02 - Confiabilidade e Integridade: Para evitar o corrompimento dos saldos e a geração de listas em branco, criamos verificadores através de blocos lógicos. Nas telas de cadastro e lançamentos, o sistema confere se algum campo está vazio ("is empty") e emite alertas. Também implementamos um bloqueio para impedir a gravação se o valor financeiro inserido for menor ou igual a zero.
RNF03 - Usabilidade: Implementamos listas e seletores nativos (Spinner, ListPicker e DatePicker) em vez de digitação livre para datas e categorias. Nossa decisão visou facilitar a utilização do aplicativo e minimizar erros do usuário, a fim de manter a padronização no banco de dados e facilitar o desenvolvimento dos filtros na tela de relatórios.
RNF04 - Desempenho: A conversão da data (DD/MM/YYYY) para um valor numérico inteiro (Ex: YYYYMMDD) na lógica de relatórios foi uma estratégia técnica que adotamos para simplificar a filtragem por período ("maior que" e "menor que"). Isso evitou a necessidade de manipular bibliotecas de tempo adicionais e otimizou o carregamento dos resultados.

7. SEGURANÇA
O aplicativo utiliza o componente PasswordTextBox, que oculta visualmente a senha durante a digitação. Entretanto, nesta versão acadêmica não foram implementados algoritmos de hash ou criptografia para o armazenamento das credenciais. Essa característica é considerada uma limitação do protótipo e uma possibilidade de melhoria futura.
8. CONCLUSÃO
Concluímos que o Gastos App atende à proposta de desenvolver um aplicativo de controle financeiro funcional no MIT App Inventor, integrando interface gráfica, armazenamento local, regras de negócio, recursos do dispositivo, relatórios e armazenamento em nuvem. Como melhorias futuras, podem ser adicionados mecanismos mais robustos de autenticação e proteção das informações em senhas. 

9. GITHUB E ENTREGÁVEIS
Repositório GitHub: JVGomes6/GastosApp_tbl_mobile: Aplicativo para o trabalho de Desenvolvimento mobile 
Código-fonte: Arquivo GastosApp.aia anexado na entrega.
Aplicativo: Arquivo instalável GastosApp.apk.
Demonstração: Link do vídeo (3 a 5 minutos) detalhando a utilização: https://drive.google.com/file/d/1Mhx26DWytSn4MyPdmgWwb507wFhn0TMr/view?usp=sharing

